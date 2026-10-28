
3. Next, we create a new private method called checkForLevelCollisions() in the renderer. We start by getting the instance settings and skipping the null instance:

```
void OGLRenderer::checkForLevelCollisions() {
    for (const auto& instance :
        mModelInstCamData.micAssimpInstances) {
        InstanceSettings instSettings =
            instance->getInstanceSettings();
        if (instSettings.isInstanceIndexPosition == 0)
            continue;
    }
```

4. Then, we simply query the triangle octree with the bounding box of the instances:

```
std::vector<MeshTriangle> collidingTriangles =
    mTriangleOctree->query(instance->getBoundingBox());
```

5. Just knowing that we have collisions is nice, but we want to store the collision data for further actions, such as reacting to collisions or drawing debug lines. So, we store the vector of MeshTriangles in the instance settings:

```
instances.at(i)->setCollidingTriangles(
    collidingTriangles);
}
}
```

The method checkForLevelCollisions() is called to the draw() method of the renderer, after the instances have been rendered and before the UI is drawn. Now all instances are checked in every frame for collisions with the level geometry, and if colliding triangles were found, we store them in the InstanceSettings of the instance.

Although you could have trust in the general functionality of collision detection to work, seeing is believing. We add extra debug lines for the various level data now, allowing us to draw viable information as overlaying lines on the screen.

## Drawing debug lines

If we would like to see the collisions in real time on the screen, we can highlight the affected triangles in the level geometry. To achieve this highlighting, we will follow these steps:

- 1. We add a new private member, mLevelCollidingTriangleMesh, to store the colliding triangle meshes to the renderer:

```
std::shared_ptr<OGLLineMesh>
mLevelCollidingTriangleMesh = nullptr;
```

- 2. Then, in checkForLevelCollisions(), we clear the mesh:

```
mLevelCollidingTriangleMesh->vertices.clear();
```

- 3. Next, we iterate over all colliding triangles, adding the vertices pairwise to the mesh storage to create a line for every side of the triangle:

```
for (const auto& tri : collidingTriangles) {
OGLLineVertex vert;
vert.color = glm::vec3(1.0f, 0.0f, 0.0f);
vert.position = glm::vec4(tri.points.at(0), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(1), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(1), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(2), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(2), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
vert.position = glm::vec4(tri.points.at(0), 1.0f);
mLevelCollidingTriangleMesh->vertices.push_back(vert);
}
```

- 4. Now we have saved the outlines of the detected triangles for all instances. Rendering the triangles to the screen can be done with a simple line draw call:

```
mLineVertexBuffer.uploadData(*mLevelCollidingTriangleMesh);
if (mLevelCollidingTriangleMesh->vertices.size() > 0) {
```

```
mLineShader.use();
mLineVertexBuffer.bindAndDraw(GL_LINES, 0,
    mLevelCollidingTriangleMesh->vertices.size());
}
```

Running the application with instance/level collision and debug drawing results in a picture similar to Figure 12.1:

![](img/3cfb9832c943f5f7366fd104b5255971_411_0.png)

Figure 12.1: Collision between an instance and the level geometry

In Figure 12.1, the instance collides with the ground and several walls. All the triangles were detected by using their bounding boxes, so don’t be alarmed if a triangle is highlighted while the instance is still a bit away from the triangle itself. In this case, the bounding box of the triangle was hit, spanning a larger area than the triangle itself. But the number of false positives among the detected collisions is low and may only affect the performance of the collision detection by a small amount.

To add a proper reaction of the instance to a collision with the level geometry, we will enhance the node tree next.

## Extending the node tree to support level geometry collisions

Thanks to the good foundation built in the last chapters, adding a new collision type takes a matter of minutes by following these steps:

- 1. First, we add the new event type to the nodeEvent enum in the Enums.h file:
```
enum class nodeEvent : uint8_t {
...
instanceToLevelCollision,
NUM
};
```
- 2. We also add some text to the micNodeUpdateMap during the init() method of the renderer to add a name in the node tree:
```
mModelInstCamData.micNodeUpdateMap[
nodeEvent::instanceToLevelCollision] =
"Inst to Level collision";
```
- 3. Then, in checkForLevelCollisions(), trigger the new event if we have at least one colliding triangle:
```
if (collidingTriangles.size() > 0) {
 mModelInstCamData.micNodeEventCallbackFunction(
 instSettings.isInstanceIndexPosition,
 nodeEvent::instanceToLevelCollision);
}
```
- 4. The event informs the node tree of the instance that a collision with level geometry occurred. As an example, we could just let the instance turn around by 180 degrees, as shown in Figure 12.2:

![](img/3cfb9832c943f5f7366fd104b5255971_413_0.png)

Figure 12.2: An Event node reacting to instance/level collisions

In the Event node in Figure 12.2, the instance would instantly turn around when running into a collision with the level. Also, a cooldown of 250 milliseconds has been added. Using a cooldown should give the instance a bit of time to walk far enough away from the affected level geometry to avoid immediately re-triggering a level collision.

But right now, collision detection has a huge drawback: the instances keep walking at the same height level, thus running through hills or walking in the air when the elevation changes.

To keep the instances on the ground, let’s add a simple form of gravity to the application.

## Using gravity to keep the instances on the floor

Gravity is literally always around us, accelerating objects downwards until they hit some other, immovable object, such as the ground. We mostly don’t realize the effects of gravity while walking around or standing still; we only realize its power when something falls down and most probably breaks when it hits the ground.

Before we can add gravity, we need to find a way to detect if a triangle in the level geometry belongs to the ground or not.

## Finding ground triangles in level data

For real-world levels, extra attributes could be used to mark areas of a level as walkable or not. Since we want to keep the ground-level technique as generic as possible, we will use a different approach and use the triangle normal to check if the area around us belongs to the floor or a wall, and up to which angle a sloped area can be walked on.

We store the normal of each triangle as 3-element vector inside the MeshTriangle struct, along with the other triangle data:

```
struct MeshTriangle {
    ...
    glm::vec3 normal;
    BoundingBox3D boundingBox;
};
```

When generating the triangle octree in generateLevelOctree(), the normal from the first vertex of the triangle is taken:

```
tri.normal = glm::normalize(normalMat *
    glm::vec3(mesh.vertices.at(
    mesh.indices.at(i)).normal));
```

Due to the indexed structure of the level data, using one of the normals of the vertices could lead to artifacts caused by reusing the same vertex for different triangles. In this case, we could alter the calculation of the normal to be the cross product of two edges of every triangle:

```
glm::vec3 edge1 = glm::vec3(mesh.vertices.at(
    mesh.indices.at(i + 1)).position -
    mesh.vertices.at(mesh.indices.at(i)).position);
glm::vec3 edge2 = glm::vec3(mesh.vertices.at(
    mesh.indices.at(i + 2)).position -
    mesh.vertices.at(mesh.indices.at(i)).position);
tri.normal = glm::normalize(normalMat *
    glm::cross(edge1, edge2));
```

To be able to control the ground slope in the user interface, we add a new variable called rdMaxLevelGroundSlopeAngle to the OGLRenderData struct:

```
float rdMaxLevelGroundSlopeAngle = 0.0f;
```
