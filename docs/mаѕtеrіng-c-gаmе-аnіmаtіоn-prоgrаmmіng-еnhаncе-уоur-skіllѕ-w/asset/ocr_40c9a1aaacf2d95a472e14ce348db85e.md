Once we have the transformation we want to see, we can get the lines from the octree and the instances.

# Collecting the lines

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