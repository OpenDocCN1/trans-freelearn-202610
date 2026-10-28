# Performance considerations for asset AABBs

Currently, the AABB for non-animated instances is calculated for every instance in every frame, like the AABB for the animated instances. If the calculation becomes a bottleneck, for example, if a lot of static assets are placed in the virtual world, changing the AABB computation only on property changes in the UI can be done easily.

Once the level data has been loaded and processed and the AABBs are calculated, we are ready to draw the level triangles to the screen.

Let’s extend the renderer to add the level data to the rendering process.

# Sending the level data to the GPU

With every addition of new features, it becomes simpler to reuse code in the application. In the case of level data, mesh data is encapsulated in the AssimpLevel class, and drawing the level’s triangles can be done in a small loop over all loaded levels.

As the first step, we create a new shader to draw the level data.

# Creating a new shader

Since we need to draw the level triangles only once, using a separate shader for the level data is a clever idea. Add the private shader named mAssimpLevelShader to the header file of the renderer class:

```
Shader mAssimpLevelShader{};

```

The shader will use the two new files, assimplevel.vert for the vertex shader and assimplevel.frag for the fragment shader. We load the files along with the other shaders in the init() method of the renderer:

```
if (!mAssimpLevelShader.loadShaders(
"shader/assimp_level.vert",
"shader/assimp_level.frag")) {
  return false;
}

```