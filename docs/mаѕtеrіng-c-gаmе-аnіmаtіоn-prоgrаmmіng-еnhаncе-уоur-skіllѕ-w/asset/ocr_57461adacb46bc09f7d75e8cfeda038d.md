```
mModelInstData.miAssimpInstances.at(
mModelInstData.miSelectedInstance);
```

In addition to storing the instance, we alter a float value inside the OGLRenderData respective VkRenderData struct by adding a scaled deltaTime, and resetting the value once it reaches 2.0f:

```
mRenderData.rdSelectedInstanceHighlightValue +=
deltaTime * 4.0f;
if (mRenderData.rdSelectedInstanceHighlightValue > 2.0f) {
  mRenderData.rdSelectedInstanceHighlightValue = 0.1f
}
```

The value of the variable rdSelectedInstanceHighlightValue will be used in the shader to scale up or down the color of the selected instance. By adding the value of deltaTime in every draw() call and resetting the highlight variable to 0.1f when we reach 2.0f, the selected install will blink from very dark to very bright. The blinking instance will be easier to spot on the screen, compared to just the coordinate arrows.

Inside the instance loop, we compare the smart pointer of the instance we are working on and the saved smart pointer of the selected instance. If they are identical, the alternating value of the variable rdSelectedInstanceHighlightValue will be set at the index of the current instance in the mSelectedInstance vector:

```
if (currentSelectedInstance ==
modelType.second.at(i)) {
  mSelectedInstance.at(i) =
  mRenderData.rdSelectedInstanceHighlightValue;
} else {
  mSelectedInstance.at(i) = 1.0f;
}
```

If we are working on any other instance in the loop, we simply set the x value to 1.0f, resulting in an unchanged color of the instance in the shader.

The collected data of the mSelectedInstance vector is then uploaded to the SSBO. For example, the OpenGL renderer uses the uploadSsboData() method of the ShaderStorageBuffer class to upload the vector data to the GPU:

```
mSelectedInstanceBuffer.uploadSsboData(
mSelectedInstance, 3);
```