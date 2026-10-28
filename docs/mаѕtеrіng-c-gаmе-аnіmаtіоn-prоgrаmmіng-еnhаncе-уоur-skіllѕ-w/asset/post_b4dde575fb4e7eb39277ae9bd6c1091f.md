
## Quadtree

The basic element of a **quadtree** is a single cell, being either a square or a rectangle. We will only use square cells for the description, but all holds true for rectangle-shaped cells, too. Objects are inserted with their position and sizes into the root cell, usually by using a two-dimensional bounding box covering the extents of the object.

The magic part of a quadtree starts when a configurable threshold of objects per cell has been hit. The affected cell is subdivided into four equally sized child cells. Any object overlapping one or more children could be either kept in the parent cell or added to all affected child cells, depending on the implementation. All other objects are moved into the respective child cell. See Figure 8.2 for an example of a quadtree:

![](img/3cfb9832c943f5f7366fd104b5255971_258_0.png)

*Figure 8.2: A quadtree with different subdivisions*

Dividing the squares into four child cells and moving the objects from the parent cell into the child cells reduces the number of objects per cell to be less than the configured threshold, minimizing the number of objects to test for collisions.

If the sum of all objects inside the four child cells falls below the threshold, all objects are moved to the parent cell again, and the now empty child cells are deleted. This dynamic behavior helps to keep the number of objects in each cell between zero and the threshold, independent of the size, number of parents, or location of a cell.

A quadtree can hold only two-dimensional information about the object’s position and size. To extend the same logic into three dimensions, an octree can be used.

## Octree

The basic functionality of a quadtree and an **octree** is identical. The only difference is the number of dimensions used for the element of the tree itself. While a quadtree uses squares or rectangles as cells, an octree is made of cubes or cuboids. In Figure 8.3, a simple octree is shown:

![](img/3cfb9832c943f5f7366fd104b5255971_259_0.png)

The inserted objects are internally maintained as three-dimensional axis-aligned bounding boxes, representing the extents of the object. A split operation when reaching the threshold results in creating eight sub-cubes as child cells (or eight sub-cuboids).

An octree is an effective way to remove large parts of the three-dimensional space when checking for possible collisions. Another data structure to handle two- and three-dimensional space partitioning is binary space partitioning (BSP).

You may have heard of the three letters BSP from older games. One of the first games using BSP trees to maintain level data was the 1993 game Doom from Id Software.

Although the level data in Doom was only two-dimensional, the game engine created the illusion of a fully three-dimensional game.

A BSP tree is created by recursively dividing the world space by using lines (2D) or planes (3D) as hyperplanes, creating a front side and a back side. The division into front and back sides continues until the remaining partition fulfills some exit condition; for games, this condition is usually when the partition is either completely filled or empty.

If any other lines or planes are hit by the subdivision, these lines or planes are split into two parts, one residing on the front side and the other on the back side.

Figure 8.4 shows the subdivision of a space and the resulting BSP tree:

![](img/3cfb9832c943f5f7366fd104b5255971_260_0.png)

Figure 8.4: An example object and the resulting BSP tree

In Figure 8.4, line A is used as the starting point, with the front side pointing downward. Splitting the space into two halves also splits lines B and C, resulting in lines B1, B2, C1, and C2. Line B1 lies on the back side of A and will be added as the left child to A, and B2, on the back of A, as the right child of A. Both lines C1 and C3 are on the back sides of B1 and B2, so they are added as the left children to B1 respective to B2.

While parsing a BSP tree to find a partition is really fast, generating the same BSP tree is a time-consuming task. The tree generation is done offline in most cases, and the precalculated tree is shipped with the game or application. Checking all lines or planes against all other lines or planes is the same type of problem we have with collision detection.

The inability to quickly change or update the elements of a BSP tree makes such a tree only suitable for static data (i.e., for level data of a game). Dynamic game elements, like doors or players, require using a different data structure, for instance, an octree.

Similar to BSP trees, a k-d tree is fast for searching elements but slow for creation or updates.

## K-d tree

A k-d tree stores information about objects in a k-dimensional space. The algorithm is a bit more complex compared to the previous trees. At every data point insertion, the remaining space gets split into two parts, and after the split, the affected dimension is changed.

For a two-dimensional k-d tree, the split dimensions alternate between the X and Y axes; for a three-dimensional k-d tree, it alternates in the order X, Y, and Z; and so on. Figure 8.5 shows what a k-d tree in two and three dimensions looks like:

![](img/3cfb9832c943f5f7366fd104b5255971_261_0.png)

The red lines and the respective blue lines are for the split dimension in 2D: red stands for an X split and blue for a Y split. In three dimensions, the same pattern applies, and we use the “next” dimension upon consecutive splits. Due to the removal of parts of the remaining space, searching in a k-d tree for an element is fast. The main uses for k-d trees are point clouds and searching for the nearest neighbors of a given point.

A slightly different method for space partitioning is by using a bounding volume hierarchy.

## Bounding volume hierarchy

In contrast to the previous tree variants, a bounding volume hierarchy can be achieved by different types of geometrical representations. As an example, we are using two-dimensional bounding circles, as shown in Figure 8.6:

![](img/3cfb9832c943f5f7366fd104b5255971_261_1.png)

By enclosing two or more bounding circles into a larger bounding circle, the number of collision tests can be lowered. If the possible colliding object does not hit the outer circle, there is no need to check any of the inner circles for possible collisions. The inner circles are impossible to reach for the colliding object.

Only if we hit the outer circle is a deeper check required. Similar to the other trees, a bounding volume hierarchy can remove larger parts of the world space from further collision checks.

Before we start the implementation of a spatial partitioning algorithm, we need to explore the second way of accelerating collision detection: using simplified representations of the instances for faster checks.

## Simplifying the instances for faster collision checks

Instead of checking every triangle of the instances after reducing the overall number, we can greatly improve the performance of collision checks if we use model abstractions. These abstractions are made of only a few geometrical elements, like boxes, cuboids, or circles, enclosing the instance. If these abstractions don’t intersect, the instances cannot have a collision, and we can remove the instance already from our list of candidates.

One of the fastest abstractions for an object is the axis-aligned bounding box.

## Axis-aligned bounding box

An axis-aligned bounding box (AABB) is a rectangle or cuboid, just large enough to contain the object, and all lines of the rectangle or planes of the cuboid are aligned with the axes of the Cartesian coordinate system. We will use the two-dimensional variant of an AABB during the implementation of the quadtree, the bounding box. Figure 8.7 shows a two-dimensional bounding box:

![](img/3cfb9832c943f5f7366fd104b5255971_262_0.png)

Figure 8.7: A two-dimensional bounding box

AABBs are best suited for objects shaped close to squares, rectangles, cubes, or cuboids. For round objects, AABBs will produce a lot of space between the object and the box extents, leading to more “false positives” when checking for a possible collision.

Checking intersections between AABBs is a quick and easy task. Since all four lines or six sides are aligned to axes, only one dimension per line or plane needs to be checked against the opposite line or plane of the second instance.

For instance, the bottom plane of instance one and the top plane of instance two can be checked by just comparing the Y values (if the Y axis points upward). If this check signals that the instances are not colliding (namely, the top plane of the second instance is not above the bottom plane of instance one), the collision check can be ended immediately – these two instances cannot intersect at all. So, we can do a fast preflight check by using AABBs.

When we rotate an AABB with the object, an **oriented bounding box (OBB)** will be created.

## Oriented bounding box

Just rotating the AABB with the object may be tempting, since we still have a good ratio between the object data and the extra space of the box. See Figure 8.8 for an example:

![](img/3cfb9832c943f5f7366fd104b5255971_263_0.png)

*Figure 8.8: An oriented bounding box*

But be aware that OBBs are not just AABBs in disguise! Rotating the planes will no longer allow us to do simple coordinate checks, and we now need to solve a lot of plane equations or use advanced methods like the so-called **Separating Axis Theorem** to check if the side planes of the two OBBs intersect. Plus, the creation and rotation of OOBs can be complex and ambiguous. So, the tip here is to stay away from OBBs; in most cases, they are not worth the additional complexity.

For round(ish) objects, bounding circles and spheres are a good simplification type.
