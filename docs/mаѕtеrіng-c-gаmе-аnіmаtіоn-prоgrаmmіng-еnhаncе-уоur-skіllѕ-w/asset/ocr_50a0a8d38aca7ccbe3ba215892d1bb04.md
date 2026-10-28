In the AssimpInstance class, a new private member named mBoundingBox will be added, storing the bounding box of the instance:

```
BoundingBox2D mBoundingBox{};

```

We don't need to load or save the bounding box, so it can be stored directly in the AssimpClass, outside of the InstanceSettings struct.

We need two trivial public methods to store and retrieve the bounding box:

```
BoundingBox2D getBoundingBox();
void setBoundingBox(BoundingBox2D box);
```

As the last step for the bounding box implementation, we must add a new quadtree folder to the CMakeLists.txt file in the project root folder in two places, to be able to use the bounding box and the quadtree in other classes. First, add the folder with a wildcard for all .cpp files to the SOURCES file search:

```
file(GLOB SOURCES
...
quadtree/*.cpp
...
)
```

Then, append the quadtree folder to the include directory list:

```
target_include_directories(${PROJECT_NAME} ... quadtree)
```

After recreating the CMake files, which should be done automatically by Visual Studio and other IDEs after saving the changes in CMakeLists.txt, we can continue with the implementation of the quadtree.

# Rewriting the quadtree code to fit our needs

We will store the new Quadtree class in the Quadtree.h and Quadtree.cpp files in the quadtree folder. The transformation from the quadtree C++ template to a C++ class needs some extra steps compared to the bounding box code.

We will need to make the following adjustments while translating the C++ template code:

+   - Replace the custom Vector2 type with glm::vec2.
+   - Replace the custom Box type with the BoundingBox2D type and change the parameters to glm::vec2.