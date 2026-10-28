Right now, the return value of the callback only adjusts the position in the list of models. But it could be used to give feedback to the user – an error message popup can be presented if the model file could not be opened, or the same model file was already loaded.

# Drawing the model to the screen

The process of drawing the model in the renderer is simple:

+   - Get the smart pointer that points to the Assimp model.
+   - If the model is animated, update the animation with the delta time between the last frame and this frame, and collect the new bone matrices.
+   - If the model is not animated, simply get the node matrices of the instances.
+   - Upload the matrix data to the shader storage buffer on the shader.
+   - Issue a draw call to the graphics API.

The custom classes handle all other steps required to draw the model, like uploading the vertex data or binding the correct texture for the mesh to draw next.

Loading and drawing a single file is already cool. But to use the full power of Assimp, we will allow adding and deleting different models and model instances at program runtime.

# Adding and removing model instances dynamically

Supporting multiple instances from multiple models was done by creating the class AssimpInstance in the model folder. Every AssimpInstance contains a smart pointer to access its base model, including the nodes and bones. Adding multiple instances of the same model requires one of two options to handle nodes and bones: using a copy of the node data structures in every instance or sharing the model's nodes across all instances.

To avoid the duplication of all nodes in every instance, we will reuse the nodes of the model during the calculation of each final node position for the frame.

# Reusing the bones for the sake of simplicity

To calculate the nodes during animation in the updateAnimation() method, we iterate the channels of the clip and use the model's corresponding node:

```
for (const auto& channel : animChannels) {
    std::string nodeNameToAnimate =
        channel->getTargetNodeName();
```