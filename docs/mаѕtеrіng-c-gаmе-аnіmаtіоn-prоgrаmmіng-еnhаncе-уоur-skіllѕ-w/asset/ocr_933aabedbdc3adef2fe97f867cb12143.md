# Implementing light control

As the first step, we add two new variables called lightPos and lightColor to the Matrices uniform in all vertex and fragment shaders where Matrices exists:

```
layout (std140, binding = 0) uniform Matrices {
  ...
  vec4 lightPos;
  vec4 lightColor;
};
```

In lightPos, we will hand over the current position of the light source, allowing us to simulate the position of the Sun or Moon in the virtual world, and in lightColor, we transport the color of the light source to the GPU, enabling us to simulate sunrise, noon, sunset, and so on.

Since we defined the uniform buffer containing the view and projection matrix on the CPU to use glm::mat4 as the data type, we must add the two glm::vec4 vectors for the light position and color as the first two elements of a glm::mat4 that will be uploaded to the GPU before drawing the frame:

```
glm::mat4 lightMatrix = glm::mat4(lightPos, lightColor,
    glm::vec4(), glm::vec4());
matrixData.emplace_back(lightMatrix);
```

Now all fragment shaders containing the light source definitions can be adjusted with a better light control for the surfaces.

We start the new shader code in the main() method by defining the ambient light level:

```
float ambientStrength = 0.1;
vec3 ambient = ambientStrength * max(vec3(lightColor),
    vec3(0.05, 0.05, 0.05));
```

The ambient light level is used to simulate the light scattering from other surfaces, enabling some sort of minimal lighting in the virtual world. By limiting the minimal ambient light, we also prevent the resulting picture from becoming pitch black.

Next, we calculate the diffuse part by using the angle between the triangle normal and the direction of the light source:

```
vec3 norm = normalize(vec3(normal));
vec3 lightDir = normalize(vec3(lightPos));
```