# Adjusting logic to shaders and the UI

As the next step for adding the highlight to the selected instance, the shaders have to be adjusted. In the assimp_skinning.vert vertex shader in the shader folder, the new SSBO must be added:

```
layout (std430, binding = 3) readonly restrict
buffer InstanceSelected {
float selected[];
};
```

The same addition is needed in the assimp.vert shader in the shader folder, used for non-animated model instances.

Please check the binding number – due to the missing animation data, the number of SSBOs differs between the shaders for animated and non-animated instances: The shader for animated models binds the instance selection data on binding point 3 since binding point 2 is already used by the world position matrices:

```
mShaderBoneMatrixBuffer.bind();
mShaderModelRootMatrixBuffer.uploadSsboData(
mWorldPosMatrices, 2);
mSelectedInstanceBuffer.uploadSsboData(
mSelectedInstance, 3);
```

In contrast, the shader for non-animated models binds only two buffers:

```
mShaderModelRootMatrixBuffer.uploadSsboData(
mWorldPosMatrices, 1);
mSelectedInstanceBuffer.uploadSsboData(
mSelectedInstance, 2);
```

To adjust the color of the selected instance, we can now use the internal variable gl_InstanceID of the shader, retrieving the value of the data at the location of the instance in the selected buffer:

```
color = aColor * selected[gl_InstanceID];
```

As an optional change, we could also decrement the depth value of the selected instance by 1.0f:

```
if (selected[gl_InstanceID] != 1.0f) {
	gl_Position.z -= 1.0f;
}
```