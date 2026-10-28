
Right after creating the **Instance Positions** window in the createPositionsWindow() method of the UserInterface class, we add a check to see if the current window has been hovered over:

```
if (ImGui::IsWindowHovered(
    ImGuiHoveredFlags_RootAndChildWindows)) {
```

Without the check, the mouse buttons and motions would trigger the octree view changes in all windows, leading to unwanted results.

Then, we get a reference to the internal io structure of ImGui:

```
ImGuiIO& io = ImGui::GetIO();
mOctreeZoomFactor += 0.025f * io.MouseWheel;
mOctreeZoomFactor = std::clamp(mOctreeZoomFactor,
    0.1f, 5.0f);
```

In ImGui’s io structure, many internal states are available, like changes made to the mouse wheel or the mouse position.

We use the mouse wheel here to adjust the zoom factor of the octree view. Setting upper and lower bounds for the zoom factor helps to avoid losing the overview.

Next, we check if the right mouse button has been pressed and rotate the view according to the mouse movement while the right mouse button is still pressed:

```
if (ImGui::IsMouseDown(ImGuiMouseButton_Right)) {
    mOctreeRotation.y += io.MouseDelta.x;
    mOctreeRotation.x += io.MouseDelta.y;
}
```

Using the right mouse button to change the rotation is also used for the camera, so this kind of view change should be known from working with the application.

Finally, we check for the middle mouse button, moving the octree around with the mouse motion:

```
if (ImGui::IsMouseDown(ImGuiMouseButton_Middle)) {
    mOctreeTranslation.x += io.MouseDelta.x;
    mOctreeTranslation.y += io.MouseDelta.y;
}
}
```

If you wonder why we don’t use the left mouse button here: Pressing the left mouse button above an ImGui window activates the internal window movement. We have only the right and middle mouse buttons left to achieve two different view changes.

Once we have the transformation we want to see, we can get the lines from the octree and the instances.

## Collecting the lines

To store the lines before drawing, we add a private variable named mOctreeLines to the UserInterface class:

```
OGLLineMesh mOctreeLines{};
```

After clearing the vertices vector inside mOctreeLines, we get the octree lines:

```
mOctreeLines.vertices.clear();
const auto treeBoxes =
    modInstCamData.micOctreeGetBoxesCallback();
```

For every octree quadrant, we get a BoundingBox3D object, containing the minimum and maximum point positions. A conversion to an AABB is simple:

```
AABB boxAABB{};
boxAABB.create(box.getFrontTopLeft());
boxAABB.addPoint(box.getFrontTopLeft() +
    box.getSize());
```

Then, we use the getAABBLines() method to create all lines for the AABB as an OGLLineMesh and add the lines to the mOctreeLines mesh:

```
std::shared_ptr<OGLLineMesh> instanceLines =
    boxAABB.getAABBLines(white);
mOctreeLines.vertices.insert(
    mOctreeLines.vertices.end(),
    instanceLines->vertices.begin(),
    instanceLines->vertices.end());
```

Next, we get AABB lines for every instance, coloring colliding instances red and all other instances yellow. The currently selected instance also gets an additional green border to be able to stop the instance easily.

At the end of the loops over both the octants and the instances, mOctreeLines contains vertices of all lines that should be drawn to the ImGui window. We now need to transform the vertices to match the selected scaling, rotation, and translation.

## Calculating the view and drawing the lines

Since storing intermediate results is faster than allocating and calculating a new transformation matrix for the octree lines in every frame, we add three more private member variables named mScaleMat, mRotationMat, and mOctreeViewMat:

```
glm::mat3 mOctreeViewMat = glm::mat3(1.0f);
glm::mat4 mRotationMat = glm::mat4(1.0f);
glm::mat4 mScaleMat = glm::mat4(1.0f);
```

Then, scaling and the two rotations are applied to create the final view matrix:

```
mScaleMat = glm::scale(glm::mat4(1.0f),
glm::vec3(mOctreeZoomFactor));
mRotationMat = glm::rotate(mScaleMat,
glm::radians(mOctreeRotation.x),
glm::vec3(1.0f, 0.0f, 0.0f));
mOctreeViewMat = glm::rotate(mRotationMat,
glm::radians(mOctreeRotation.y),
glm::vec3(0.0f, 1.0f, 0.0f));
```

Now we loop over the vertices in mOctreeLines in groups of two since we need a starting point and ending point for each line:

```
for (int i = 0; i < mOctreeLines.vertices.size(); i += 2) {
```

Inside the loop, we extract the vertex pair for each line and transform the vertex positions by multiplication with the mOctreeViewMat:

```
OGLLineVertex startVert = mOctreeLines.vertices.at(i);
OGLLineVertex endVert = mOctreeLines.vertices.at(i+1);
glm::vec3 startPos = mOctreeViewMat *
    startVert.position;
glm::vec3 endPos = mOctreeViewMat * endVert.position;
```

Next, we add the point coordinates to the center of the drawing and add the translational part:

```
ImVec2 pointStart =
    ImVec2(drawAreaCenter.x + startPos.x +
        mOctreeTranslation.x,
        drawAreaCenter.y + startPos.z +
        mOctreeTranslation.y);
ImVec2 pointEnd =
```

```
ImVec2(drawAreaCenter.x + endPos.x +
    mOctreeTranslation.x,
    drawAreaCenter.y + endPos.z +
    mOctreeTranslation.y)
```

We don’t need a separate translation matrix here because the movement of the octree lines is only related to the ImGui window, not the position of the vertices in 3D.

At the end of the loop, we draw an ImGui line from the start point to the end point using the colors of the line and a fixed alpha value:

```
drawList->AddLine(pointStart, pointEnd,
    ImColor(startVert.color.r, startVert.color.g,
        startVert.color.b, 0.6f));
```

If we start the application after these changes, the octree view can be rotated by pressing the right mouse button, moved around by pressing the middle mouse button, and zoomed in and out by using the mouse wheel.

Figure 11.4 shows a view of the octree in the Instance Positions window:

![](img/3cfb9832c943f5f7366fd104b5255971_388_0.png)

Figure 11.4: A rotated octree with instances and two subdivision levels

You can see the bounding boxes for all instances in three dimensions. Colliding instances are drawn in red, all other instances are yellow (the same colors as for the AABB debug lines), and the currently selected instance has an extra outline in green.

Another important fact to see is the subdivision level of the octree. At the root level of the octree, one subdivision is divided into eight octants – this has happened because of the number of instances. In the far lower right octant, another subdivision was needed to limit the instance count per octant.

When the application is running, the movement of the instances and changes in the octree subdivision can be seen in real time. And the view can be changed to focus on any interesting area of the octree.

One drawback of the default octree implementation is the growing overhead. A subdivision may produce a lot of empty nodes, especially if the threshold per node is small and the tree depth is high. For our implementation, each subdivision into eight octants adds a memory overhead of roughly one kilobyte in memory usage, even if we need to store only one or two instance AABBs in one of the child octants.

Also, each subdivision adds another step to the traversal costs. Even though the traversal complexity grows only on a logarithmic basis, the overhead in memory and traversal time may become significant in larger levels. In this case, a data structure such as a sparse voxel octree could be used instead. A link to a paper describing the principles of sparse voxel octrees is available in the Additional resources section.

A shortcoming of the current level loading is a missing “feeling” about the dimensions of the loaded level data. If the level is not flat but contains areas with different heights, it is hard to imagine the edges of the level data.

To get a better orientation, we will also add an AABB for the level data.

## Building an AABB for the level data

Axis-aligned bounding boxes are a great tool to avoid estimating or guessing the dimensions of an object. We are using model instance AABBs as a solution to detect collisions by allowing the application to compare the maximum extents of two instances.

While unused areas between the vertices of the instances and the bounding box are a trade-off between speed and complexity for collision detection, the situation is different for level data. The AABB lines will help us to see the maximum extent of the level in all three dimensions, especially when the level data has large unused areas.
