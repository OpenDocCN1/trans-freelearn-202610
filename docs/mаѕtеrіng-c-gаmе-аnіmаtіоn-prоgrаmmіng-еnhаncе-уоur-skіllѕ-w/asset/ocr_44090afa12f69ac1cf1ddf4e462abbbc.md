Now we can use the octree to detect collisions in three dimensions. Sadly, the Instance Positions window still shows a top-down view of the instances, and we are unable to see if any instances are placed at different heights. We must adjust the rendering of the mini window containing the instances so that we can see any instances placed at different heights.

# Creating an interactive octree view

The new octree view is created in three steps:

+   1. For a better overview of the octree and the instances, the new view can be scaled, rotated, and translated by using the mouse. It may still take some extra rotations to align the octree with the camera view, but that is hard to do due to the orthogonal display and the symmetry of the octree. Fading out far away parts of the octree or highlighting the octant containing the camera may help to focus on the current area of interest. On the other hand, using a perspective projected octree may make it easier to find the right alignment, but the perspective will distort the distances between the instances, and we just create a copy of the main level rendering.
+   2. The lines of the octree quadrants and the instances are collected into a temporary data structure. We will use an OGLLineMesh here as this data type contains just the bare minimum; we need to draw ImGui lines.
+   3. To achieve the desired view, all points of the octree quadrants and the instance bounding boxes must be transformed by the scaling, rotation, and translation from the first step. This transformation is done in the same way as we did for the cameras and instances: by creating a matrix and a matrix-vector multiplication for each point. There should be no surprises in the matrix operations; only the translation may need an explanation.

Let’s step through the interactive part first.

# Adding interactivity

To be able to change the octree view with the mouse button and mouse movements, we need three new private variables named mOctreeZoomFactor, mOctreeRotation, and mOctreeTranslation in the UserInterface class:

```
float mOctreeZoomFactor = 1.0f;
glm::vec3 mOctreeRotation = glm::vec3(0.0f);
glm::vec3 mOctreeTranslation = glm::vec3(0.0f);
```

The names of the variables are self-explanatory, so we do not need to dive into the details here.