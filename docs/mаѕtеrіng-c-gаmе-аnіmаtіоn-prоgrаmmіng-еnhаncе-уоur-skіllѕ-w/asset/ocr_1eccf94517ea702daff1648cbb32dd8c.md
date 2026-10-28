```
glm::vec3 angles = glm::eulerAngles(destRoation);
rotateInstance(glm::degrees(angles.y) *
    deltaTime * 2.0f);
}
```

The multiplication with deltaTime makes the instance rotation smooth since the angle in every frame is small. The initial comparison of the angleDiff value makes sure we have a small “dead zone” left, leading to fewer rotations if the path is nearly straight and avoiding oscillations in case the rotation oversteers a bit.

If the target is moving around in the level, the dead zone also reduces the number of corrections when the path of the instance is recreated. By carefully tuning the amount of oversteering and the dead zone angle, a chasing instance will behave more naturally since both parameters could reduce the number of directional changes while following the target.

To make the calculated path visible, we should also add a visual output for the renderer.

# Adding debug lines for the path

Drawing the path to the screen is astonishingly easy. Next to adding a control Boolean named rdDrawInstancePaths to the OGLRenderData struct for OpenGL, respective to the VkRenderData struct for Vulkan, a new line mesh in the renderer and a checkbox in the UserInterface class, creating the lines between all path points is easy. The best place for the new code is after the code added in the Calculating the path to the target section.

First, we check if we should create the lines at all and if we have a valid target:

```
if (mRenderData.rdDrawInstancePaths &&
    pathTargetInstance > -1) {
```

Then, we set the desired color and a height offset for the path and create a vertex to draw the line:

```
glm::vec3 pathColor = glm::vec3(0.4f, 1.0f, 0.4f);
glm::vec3 pathYOffset = glm::vec3(0.0f, 1.0f, 0.0f);
OGLLineVertex vert;
vert.color = pathColor;
```

Next, we add the world position of the current instance as the starting point for the lines:

```
vert.position = instSettings.isWorldPosition +
    pathYOffset;
mInstancePathMesh->vertices.emplace_back(vert);
```