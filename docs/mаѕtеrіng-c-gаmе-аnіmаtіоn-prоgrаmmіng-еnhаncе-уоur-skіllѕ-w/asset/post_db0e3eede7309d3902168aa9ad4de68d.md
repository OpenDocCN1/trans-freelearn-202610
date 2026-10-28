
To get a nice interactive display in the user interface, we add a Boolean named settingChangedForAABB to the Levels section. On every slider or checkbox change, or when pressing the Reset Values to Zero button, we trigger a recalculation of the level’s AABB.

And what about other static level elements? These objects are called assets and can be added to a level. Assets also need collision checks to prevent the instances from running right through each other, but assets are mostly not animated; they remain in the place intended by the level designers.

We will use a shortcut here and utilize the non-animated models to mimic static level assets. For dynamic assets, such as buttons or doors, animated models can be used; see the Level data does not move around section for a brief explanation. In the Practical sessions section, a task to add dynamic assets to the virtual world is available.

So, let’s see what changes are needed for the non-animated models.

## Using non-animated models as assets

To avoid adding a new AssimpAsset class with mostly the same functionality as the non-animated models, we will extend the current AssimpModel class and the renderer a bit.

For the model class, we change the getAABB() method to return either a dynamic AABB for models with animations or a static AABB for models without animations:

```
AABB AssimpModel::getAABB(InstanceSettings instSettings) {
    if (mNumAnimatedMeshes > 0) {
        return getAnimatedAABB(instSettings);
    } else {
        return getNonAnimatedAABB(instSettings);
    }
}
```

The new method, getAnimatedAABB(), is just a new name for the old getAABB() method, and it calculates the AABB from the lookup data as before. The other new method, getNonAnimatedAABB(), is mostly the updateModelRootMatrix() method from the AssimpInstance class.

First, we calculate a separate matrix for scaling, rotation, axis swap, and translation:

```
glm::mat4 localScaleMatrix = glm::scale(glm::mat4(1.0f),
    glm::vec3(instSettings.iScale));
glm::mat4 localSwapAxisMatrix;
if (instSettings.iSwapYZAxis) {
    glm::mat4 flipMatrix = glm::rotate(glm::mat4(1.0f),
        glm::radians(-90.0f), glm::vec3(0.0f, 0.0f, 1.0f));
    localSwapAxisMatrix = glm::rotate(flipMatrix,
        glm::radians(-90.0f), glm::vec3(0.0f, 1.0f, 0.0f));
} else {
    localSwapAxisMatrix = glm::mat4(1.0f);
}
glm::mat4 localRotationMatrix = glm::mat4_cast(
    glm::quat(glm::radians(instSettings.iWorldRotation)));
glm::mat4 localTranslationMatrix =
    glm::translate(glm::mat4(1.0f),
        instSettings.iWorldPosition);
```

Then, the transformation matrices and the root transformation matrix from the model file are combined into a single transform matrix:

```
glm::mat4 localTransformMatrix =
    localTranslationMatrix * localRotationMatrix *
    localSwapAxisMatrix * localScaleMatrix *
    mRootTransformMatrix;
```

To create the bounding box data, all vertices of the model mesh are transformed to the new positions and the positions are added to a local variable named modelAABB:

```
AABB modelAABB{};
for (const auto& mesh : mModelMeshes) {
    for (const auto& vertex : mesh.vertices) {
        modelAABB.addPoint(localTransformMatrix *
            glm::vec4(glm::vec3(vertex.position), 1.0f));
    }
}
```

For the w element of the vertex position, the same adjustment as for the level data AABB is needed. By setting the last value to 1.0f, we guarantee a correct matrix multiplication when computing the final position.

With the resulting AABB, the extents of the non-animated instance vertices are able to detect AABB collisions with animated models and to draw the debug lines.

## Performance considerations for asset AABBs

Currently, the AABB for non-animated instances is calculated for every instance in every frame, like the AABB for the animated instances. If the calculation becomes a bottleneck, for example, if a lot of static assets are placed in the virtual world, changing the AABB computation only on property changes in the UI can be done easily.

Once the level data has been loaded and processed and the AABBs are calculated, we are ready to draw the level triangles to the screen.

Let’s extend the renderer to add the level data to the rendering process.

## Sending the level data to the GPU

With every addition of new features, it becomes simpler to reuse code in the application. In the case of level data, mesh data is encapsulated in the AssimpLevel class, and drawing the level’s triangles can be done in a small loop over all loaded levels.

As the first step, we create a new shader to draw the level data.

### Creating a new shader

Since we need to draw the level triangles only once, using a separate shader for the level data is a clever idea. Add the private shader named mAssimpLevelShader to the header file of the renderer class:

```
Shader mAssimpLevelShader{};

```

The shader will use the two new files, assimp_level.vert for the vertex shader and assimp_level.frag for the fragment shader. We load the files along with the other shaders in the init() method of the renderer:

```
if (!mAssimpLevelShader.loadShaders(
"shader/assimp_level.vert",
"shader/assimp_level.frag")) {
  return false;
}

```

The fragment shader file, assimp_level.frag, is just a copy of the assimp.frag file without any changes. For the vertex shader file, assimp_level.vert, we can copy the assimp.vert file and keep the in and out layout portion and the Matrices uniform buffer.

We don’t need the selection buffer since no visual selection has been implemented for the level data, and we can change the buffer at binding point 1 to contain only a single 4x4 matrix:

```
layout (std430, binding = 1) readonly restrict
  buffer WorldTransformMatrix {
    mat4 worldTransformMat;
};
```

Uploading only small data elements like a single matrix to a uniform buffer on the GPU is not the best idea as every upload may add a small delay to the frame. For a real game, or a game engine, the world transform matrix would be part of a bigger buffer, but for the sake of simplicity we just do the single upload here.

In the main() method of the vertex shader, we use the worldTransformMat to transform the position and normal vertices to the final position given by the matrix created from the level properties:

```
void main() {
  gl_Position = projection * view * worldTransformMat *
    vec4(aPos.x, aPos.y, aPos.z, 1.0);
  color = aColor;
  normal = transpose(inverse(worldTransformMat)) *
    vec4(aNormal.x, aNormal.y, aNormal.z, 1.0);
  texCoord = vec2(aPos.w, aNormal.w);
}
```

Then, in the draw() method of the renderer, we loop over all levels and skip the null level by checking for the existence of any triangles:

As the last step of the level display process, the vertex drawing is triggered:

```
for (const auto& level : mModelInstCamData.miLevels) {
  if (level->getTriangleCount() == 0) {
    continue;
  }

  mAssimpLevelShader.use();
  mShaderModelRootMatrixBuffer.uploadSsboData(
      level->getWorldTransformMatrix(), 1);
  level->draw();
}
```

Drawing the level AABB on the screen requires a few more methods and member variables in the renderer.

### Drawing the level AABB

The level AABB vertices are stored in a new private member variable named mAllLevelAABB in the renderer class:

```
AABB mAllLevelAABB{};
```

Also, two new private methods named generateLevelAABB() and drawLevelAABB() are added to the renderer class:

```
void generateLevelAABB();
void drawLevelAABB(glm::vec4 aabbColor);
```

We also split generating and drawing the AABB lines here to avoid costly calculations in every frame we draw.

Generating the level AABB in generateLevelAABB() is done in a simple loop. After clearing the level AABB data, we loop over all loaded levels:

```
mAllLevelAABB.clear();
for (const auto& level : mModelInstCamData.miLevels) {
    if (level->getTriangleCount() == 0) {
        continue;
    }
}
```
