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

```
for (const auto& level : mModelInstCamData.miLevels) {
  if (level->getTriangleCount() == 0) {
    continue;
  }
}
```