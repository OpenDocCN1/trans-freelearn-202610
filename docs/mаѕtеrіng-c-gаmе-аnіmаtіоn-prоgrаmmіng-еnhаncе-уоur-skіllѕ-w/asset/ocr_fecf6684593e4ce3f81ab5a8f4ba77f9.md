AABBs are best suited for objects shaped close to squares, rectangles, cubes, or cuboids. For round objects, AABBs will produce a lot of space between the object and the box extents, leading to more “false positives” when checking for a possible collision.

Checking intersections between AABBs is a quick and easy task. Since all four lines or six sides are aligned to axes, only one dimension per line or plane needs to be checked against the opposite line or plane of the second instance.

For instance, the bottom plane of instance one and the top plane of instance two can be checked by just comparing the Y values (if the Y axis points upward). If this check signals that the instances are not colliding (namely, the top plane of the second instance is not above the bottom plane of instance one), the collision check can be ended immediately – these two instances cannot intersect at all. So, we can do a fast preflight check by using AABBs.

When we rotate an AABB with the object, an **oriented bounding box (OBB)** will be created.

# Oriented bounding box

Just rotating the AABB with the object may be tempting, since we still have a good ratio between the object data and the extra space of the box. See Figure 8.8 for an example:

![](bbox=[0.38, 0.55, 0.6, 0.75])

*Figure 8.8: An oriented bounding box*

But be aware that OBBs are not just AABBs in disguise! Rotating the planes will no longer allow us to do simple coordinate checks, and we now need to solve a lot of plane equations or use advanced methods like the so-called **Separating Axis Theorem** to check if the side planes of the two OBBs intersect. Plus, the creation and rotation of OOBs can be complex and ambiguous. So, the tip here is to stay away from OBBs; in most cases, they are not worth the additional complexity.

For round(ish) objects, bounding circles and spheres are a good simplification type.