# Chapter 14

The new vertex shader named skybox.vert outputs a vec3 with 3-dimensional texture coordinates. We need 3-dimensional coordinates here since we are inside a cube:

```
layout (location = 0) out vec3 texCoord;
```

In the main() method of the vertex shader, we invert the projection matrix and transpose the view matrix first:

```
mat4 inverseProjection = inverse(projection);
mat3 inverseView = transpose(mat3(view));
```

For the view matrix, we can use a cheaper transpose operation instead of taking the inverse matrix since we need to get rid of the translational part to stop the cube moving around with the camera.

By multiplying the inverse matrices with the incoming point positions of the cube, we calculate the texture coordinates in world space:

```
texCoord = inverseView * (inverseProjection * aPos).xyz;
```

Now, the texture coordinates to sample are from the inside of the cube, as shown in Figure 14.12.

As the last step for the vertex shader, we set the GLSL-internal variable gl_Position to the incoming vertex position:

```
gl_Position = aPos.xyww;
```

By setting the z component of gl_Position to the value of the w component, we make sure to draw the pixels of the cubemap on the far-Z plane, creating a background that will not be overwritten by other pixels.

The new fragment shader, skybox.frag, uses the incoming texture coordinates to look up the texture data in the cubemap:

```
layout (location = 0) in vec3 texCoord;
layout (location = 0) out vec4 FragColor;
uniform samplerCube tex;
void main() {
FragColor = texture(tex, texCoord);
}
```

Please note that the type of the texture tex is a samplerCube and not a sampler2D like on the other fragment shaders.