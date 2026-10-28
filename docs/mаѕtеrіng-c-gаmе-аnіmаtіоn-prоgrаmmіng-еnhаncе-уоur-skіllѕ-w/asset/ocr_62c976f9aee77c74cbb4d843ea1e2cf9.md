# Filling the per-instance buffer data in the renderer

As for all other shader-related data, we need an SSBO to hand the data over to the GPU. To fill the SSBO, a std::vector of some data type is needed. So, the renderer class header file OGLRenderer.h will get two new private variables called mFaceAnimPerInstanceData and mFaceAnimPerInstanceDataBuffer:

```
std::vector<glm::vec4> mFaceAnimPerInstanceData{};
ShaderStorageBuffer mFaceAnimPerInstanceDataBuffer{};
```

For the Vulkan renderer, the data type of the buffer differs. We need to add the following lines to the VkRenderer.h file:

```
std::vector<glm::vec4> mFaceAnimPerInstanceData{};
VkShaderStorageBufferData mFaceAnimPerInstanceDataBuffer{};
```

Even though we need only three values, we will use a glm::vec4 here to tackle possible data-alignment problems. You should try to avoid the three-element vector (glm::vec3) to transport data via an SSBO to the GPU since you may get a mismatch between the vector or struct on the CPU side and the buffer on the GPU side.

The per-instance face animation SSBO will be added to the draw() call of the renderer class file, OGLRenderer.cpp or VkRenderer.cpp, more specifically in the loop over the instances of the model:

```
for (const auto& model : mModelInstCamData.micModelList) {
    ...
    mFaceAnimPerInstanceData.resize(numberOfInstances);
    ...
    for (size_t i = 0; i < numberOfInstances; ++i) {
```

We also add the resizing of the vector containing the morphing data before the loop; see the highlighted part in the previous code snippet.

And since we have already extracted the InstanceSettings struct of the current instance in the loop, adding the face animation is done in just a couple of lines.

First, we add an empty glm::vec4 named morphData, plus we check whether the instance has a face animation set:

```
glm::vec4 morphData = glm::vec4(0.0f);
if (instSettings.isFaceAnimType !=
    faceAnimation::none) {
```