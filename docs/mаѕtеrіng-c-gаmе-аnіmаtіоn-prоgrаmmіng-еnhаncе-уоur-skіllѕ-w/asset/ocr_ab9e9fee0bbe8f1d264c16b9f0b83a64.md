Then, add two new SSBOs named AnimMorphBuffer and AnimMorphData on binding points four and five:

```
layout (std430, binding = 4) readonly restrict
    buffer AnimMorphBuffer {
    MorphVertex morphVertices[];
};
layout (std430, binding = 5) readonly restrict
    buffer AnimMorphData {
    vec4 vertsPerMorphAnim[];
};
```

The first buffer, AnimMorphBuffer, contains the vertices of all morph available animations. In the second buffer, AnimMorphData, all instance settings are handed over from the CPU to the GPU.

Inside the main() method of the vertex shader, we calculate the offset to the vertices for the desired face animation by multiplying the vertex count and the index of the face animation clip for every instance:

```
int morphAnimIndex =
    int(vertsPerMorphAnim[gl_InstanceID].y *
        vertsPerMorphAnim[gl_InstanceID].z);
```

It is no problem to cast the float values to int here to avoid another struct with separate integer and floating-point values and to use glm::vec4 as a “transportation object.” The first inaccurate integer represented by a float would be 2^24+1, and that value is large enough even for bigger meshes with many face animations (2^24+1 would be ~16 MiB of vertex data).

Then we extract the vertices from the morph mesh by using the gl_VertexID internal shader variable (gl_VertexIndex for Vulkan):

```
vec4 origVertex = vec4(aPos.x, aPos.y, aPos.z, 1.0);
vec4 morphVertex = vec4(morphVertices[gl_VertexID +
    morphAnimOffset].position.xyz, 1.0)
```

Now we can mix the positions of the original vertex and the replacement vertex from the morph mesh according to the weight factor:

```
gl_Position = projection * view * worldPosSkinMat *
    mix(origVertex, morphVertex,
    vertsPerMorphAnim[gl_InstanceID].x);
```