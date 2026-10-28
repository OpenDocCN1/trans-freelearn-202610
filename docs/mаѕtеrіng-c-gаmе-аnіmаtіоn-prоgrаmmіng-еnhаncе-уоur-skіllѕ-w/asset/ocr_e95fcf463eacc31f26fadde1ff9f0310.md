We could overcome the size problem with one of the following three methods:

+   1. Keep a large triangle in the octant big enough to contain the entire triangle. This solution would store additional objects in a parent node, not only in a leaf.
+   2. Add the triangle to all affected subdivided octants. We would only have data in the leaves but would duplicate the triangle data in the worst case 8 times.
+   3. Split the triangle on the octant boundaries and add only the sub-triangles to each octant. Again, we would have an additional triangle per affected octant, plus possible problems with rounding errors in the split lines.

To keep the code simple, we will use the first method and add any triangle exceeding the dimensions of a subdivided octant only in the parent octant.

We can achieve the storage process for oversized triangles with a two-step check in the add() and split() methods. First, we iterate over all child octants to find possible intersections of the triangle with the boundaries of the child octant:

```
int intersectingChildren = 0;
for (int i = 0; i < node->childs.size(); ++i) {
    BoundingBox3D childBox = getChildOctant(box, i);
    if (childBox.intersects(triangle.boundingBox)) {
        intersectingChildren++;
    }
}
```

If we find an intersection with a child octant, we increment the intersectingChildren variable. Then, for the add() method, we check how many child octants the triangle would intersect. In case of more than one octant, the triangle is kept in the current octant:

```
if (intersectingChildren > 1) {
    node->triangles.emplace_back(triangle);
} else {
    int i = getOctantId(box, triangle.boundingBox);
    if (i != -1) {
        add(node->childs.at(i), depth + 1,
            getChildOctant(box, i), triangle);
    }
}
```