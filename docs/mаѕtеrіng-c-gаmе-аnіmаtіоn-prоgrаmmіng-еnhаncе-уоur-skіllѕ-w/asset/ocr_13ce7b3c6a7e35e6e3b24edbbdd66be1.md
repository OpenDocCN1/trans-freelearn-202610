In this new struct, we will save the transformation values on a per-node basis. We are using a glm::vec4, that's a vector type with four float elements for translation and scaling to avoid additional padding values for proper alignment and simply ignoring the last element in the shader.

# GPU/CPU memory alignment may differ

Since GPUs are optimized for fast memory access, data in the buffers must be aligned in memory, in most cases to multiples of 16 bytes. This alignment will be automatically created when uploading data to the GPU. On the CPU side, a different alignment may be used, for instance for 3-element vector types like a glm::vec3, which is 12 bytes long. To use a glm::vec3 vector, an additional float is needed as padding to match the 16-byte alignment because uploading misaligned data will end up in distorted images and incorrect results.

We also use a glm::vec4 vector for the rotation, which is a glm::quat quaternion in the AssimpChannel class. The reason for this decision is simple: GLSL, the OpenGL Shading Language, does not know what a quaternion is, or how to handle a quaternion. We will have to implement the quaternion functions by ourselves in the compute shader. So, we utilize the normal 4-element vector to transport the four elements of the rotation quaternion to the shader.

Now, we can simplify the animation update. First, we add a local std::vector of our new type NodeTransformData to the class:

```
std::vector<NodeTransformData> mNodeTransformData{};
```

We iterate again over all channels, but instead of modifying the nodes of the model, we fill a local NodeTransformData variable with the transformation data:

```
for (const auto& channel : animChannels) {
    NodeTransformData nodeTransform;

    nodeTransform.translation =
        channel->getTranslation(
            mInstanceSettings.isAnimPlayTimePos);
    nodeTransform.rotation =
        channel->getRotation(
            mInstanceSettings.isAnimPlayTimePos);
    nodeTransform.scale =
        channel->getScaling(
            mInstanceSettings.isAnimPlayTimePos);
```