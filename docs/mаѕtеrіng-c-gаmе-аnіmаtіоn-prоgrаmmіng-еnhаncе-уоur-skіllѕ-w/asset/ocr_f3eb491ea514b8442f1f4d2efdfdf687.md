# Moving Animation Calculations from CPU to GPU

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