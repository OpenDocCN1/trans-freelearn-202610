
For the compute shader, we need a different approach. Since all shader invocations run in parallel, we would need to cut down the number of invocations to one per model, allowing the known linear progression on the model matrices. To use a larger amount of shader invocations, we will create an int vector that contains the number of the parent node at each position. This “parent node vector” enables us to “walk” backward on the model skeleton in the shader, collecting all parent node matrices on the way.

We create the parent node vector in the loop with the bone offset matrices. First, we get the parent node of our current bone, then use a small lambda to get the position of the parent bone in the same bone list:

```
std::string parentNodeName = mNodeMap.at(
bone->getBoneName())->getParentNodeName();
const auto boneIter = std::find_if(mBoneList.begin(),
mBoneList.end(),
[parentNodeName](std::shared_ptr<AssimpBone>& bone)
{ return bone->getBoneName() == parentNodeName; });
```

If we don’t find a parent node in the bone list, we have found the root node of the model. In this case, we add a -1 to identify the root node. In all other cases, we add the index number of the parent bone:

```
if (boneIter == mBoneList.end()) {
boneParentIndexList.emplace_back(-1);
} else {
boneParentIndexList.emplace_back(
std::distance(mBoneList.begin(), boneIter));
}
```

The boneParentIndexList now contains a flat list of the parent nodes for all the nodes in the model, with the special parent -1 for the root node. By a repeated lookup of the parent node, we can ascend the skeleton tree from every node, until we reach the root node with the special number -1 as parent.

To make the parent bone list available in the compute shader, we create another SSBO in the AssimpModel class, and upload the boneParentIndexList to the GPU:

```
mShaderBoneParentBuffer.uploadSsboData(boneParentIndexList);
```

Back in the renderer, the parent bone buffer will be bound to a binding point of our compute shader:

```
modelType.second.at(0)->getModel()
->bindBoneParentBuffer(1);
```

We haven’t finished the workload transformation to the GPU yet. Some data needs to be handled in a different way when using a compute shader.

## Relocating data to another shader

Also missing from the calculations now is the instance world position. The updateAnimation() method contains the following line to set the transformation matrix for the root node of the model:

```
mAssimpModel->getNodeMap().at(
mAssimpModel->getBoneList().at(0)->getBoneName())
->setRootTransformMatrix(mLocalTransformMatrix *
mAssimpModel->getRootTransformationMatrix());
```

The root transformation matrix of the model contains general transformations that will be applied to the entire model, like a global scaling of the model. The other matrix, mLocalTransformMatrix, is used to set the user-controlled parameters of the model instance. The local transformation matrix allows us to rotate and move the model instance in the virtual world.

In contrast to the bone offset matrices, the root node transformation will be moved to the assimp_skinning.vert vertex shader, not to a compute shader. It does not matter which of the two shaders does the matrix multiplication, but moving the root node transformation to the vertex shader may lower the load of the compute shaders a bit. Also, the vertex shader only runs for objects that are drawn to the screen, not for instances that are culled before the rendering itself, or invisible instances, potentially lowering the overall computational load of the GPU.

## Doing the last preparations

And, at last, we can also decide how many distinct compute shaders we need:

We need – at least – two compute shaders.

To calculate the final TRS matrix for a node, we need to have all parent TRS matrices completed, with all matrices multiplied from the current node up to the model root. Since we can only control the amount of shader invocations we start, but not when or how long such a shader invocation runs, we need to set some sort of barrier between the calculation of the node TRS matrices, and the process of collecting the matrices along the skeleton.

## Moving Animation Calculations from CPU to GPU

The only way to create such a barrier is on the CPU side. A barrier will be added while submitting the compute shader to the graphics API, telling the GPU to wait for the first shaders to finish, before it starts the second batch.

So, we will have to start with the node transforms, wait until all node transform matrices are finished, and then start the calculation of the final node matrices.

After the theoretical part is done, we can start the shader-related implementation.

The process of loading a compute shader differs only slightly from a vertex or fragment shader. For OpenGL, we have to set the shader type in the glCreateShader() call:

```
glCreateShader(GL_COMPUTE_SHADER);
```

For Vulkan, we must set the correct shader stage during the creation of the VkShaderModule:

```
VkPipelineShaderStageCreateInfo computeShaderStageInfo{};
computeShaderStageInfo.sType =
VK_STRUCTURE_TYPE_PIPELINE_SHADER_STAGE_CREATE_INFO;
computeShaderStageInfo.stage =
VK_SHADER_STAGE_COMPUTE_BIT;
```

All the other steps of loading the shader code, linking, or creating the shader module, stay the same. Because we have only a single shader file, additional methods have been added to the Shader class. Loading a compute shader in OpenGL can now be achieved by calling the loadComputeShader() method of the Shader class with the relative file name of the shader source:

```
if (!AssimpTransformComputeShader.loadComputeShader(
"shader/assimp_instance_transform.comp")) {
return false;
}
```

Vulkan uses the Standard Portable Intermediate Representation (SPIR-V) format for shaders. Instead of the shader source, the precompiled shader code must be loaded into the Shader class for the Vulkan renderer.

As we compute new matrices in the compute shaders, and we have to move these matrices between different shaders, two additional SSBOs are required.

## Adding more shader storage buffers

The first SSBO will store the TRS matrices that we create from the node transforms. This SSBO is a simple buffer, defined in the header file for the renderer:

```
ShaderStorageBuffer mShaderTRSMatrixBuffer{};
```

The second SSBO will contain the final bone matrices that will be used in the skinning vertex shader. The bone matrix buffer is also added as a normal SSBO declaration in the header file of the renderer:

```
ShaderStorageBuffer mShaderBoneMatrixBuffer{};
```

One important step to use the SSBO in the shaders is the have a correct size set. If the SSBO is too small, not all data will be stored in the compute shader, and instances or body parts of instances may be missing. A wrong buffer size may be hard to debug – you may not even get a warning that the shader writes beyond the end of the buffer. We must calculate the buffer size according to the number of bones, the number of instances, and the size of the 4x4 matrix, as shown here:

```
size_t trsMatrixSize = numberOfBones *
numberOfInstances * sizeof(glm::mat4);
```

Then, we resize the two SSBOs to the final matrix size:

```
mShaderBoneMatrixBuffer.checkForResize(trsMatrixSize);
mShaderTRSMatrixBuffer.checkForResize(trsMatrixSize);
```

When drawing multiple models, both buffers will end up with the maximum size of all models. But this does not do any harm, as the buffers will be reused for the next model and filled only up to the real amount of data used in the new model.

## Calculating the node transforms in a shader

For the first compute shader, we must upload the node transform data to the first compute shader. We bind the SSBO storing the new TRS matrices created from the node transform to the proper binding point of the compute shader:

```
mAssimpTransformComputeShader.use();
mNodeTransformBuffer.uploadSsboData(
    mNodeTransFormData, 0);
mShaderTRSMatrixBuffer.bind(1)
```
