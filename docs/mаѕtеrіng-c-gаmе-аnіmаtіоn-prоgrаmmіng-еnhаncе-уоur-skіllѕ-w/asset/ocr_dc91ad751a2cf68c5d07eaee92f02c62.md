# # Creating the final node matrices

Before we can start the next compute shader, we must bind all buffers that will be used during the shader run. We have a total of four SSBOs:

```
```cpp
mAssimpMatrixComputeShader.use();
mShaderTRSMAtrixBuffer.bind(0);
modelType.second.at(0)->getModel()
->bindBoneParentBuffer(1);
modelType.second.at(0)->getModel()
->bindBoneMatrixOffsetBuffer(2);
mShaderBoneMatrixBuffer.bind(3);
```
```

Since all data already resides on the GPU, we don’t need any kind of upload here.

The second compute shader itself is called assimps_instance_matrix_mult.comp and can be found in the shader folder. The shader code starts – again – with a version and the local size definitions:

```
```glsl
#version 460 core
layout(local_size_x = 1, local_size_y = 32, local_size_z = 1) in;
```
```

A local size 32 is used since the code was developed on a machine with an NVIDIA GPU. For an AMD GPU, you should use a local size of 64, as explained in the section Calculating the node transforms in a shader.

Similar to the first compute shader, the SSBOs follow:

```
```glsl
layout (std430, binding = 0) readonly restrict
buffer TRSMAtrix {
	mat4 trsMat[];
};
layout (std430, binding = 1) readonly restrict
buffer ParentMatrixIndices {
	int parentIndex[];
};
layout (std430, binding = 2) readonly restrict
buffer BoneOffsets {
	mat4 boneOffset[];
};
```
```