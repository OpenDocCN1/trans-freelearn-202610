Checking a convex hull for collisions needs a lot of tests, possibly lowering the effect we wanted to achieve with our model simplification. As long as you don't have models with lots of triangles, a convex hull may be out of scope.

On the other hand, if small collision errors can be tolerated, the convex hull can be used as a replacement for a high-poly model, and all collision checks can be done only against the convex hull.

As the last method, the bounding volume hierarchy can be used. The hierarchy model is not just suitable for space partitioning but also for model simplification.

# **Bounding volume hierarchy**

The general idea of the bounding volume hierarchy for space partitioning also holds true to simplify the instances. But here, the model parts are enclosed in larger and larger volumes. Figure 8.12 shows a simplified example of the bounding volume hierarchy around a character model:

![](bbox=[0.22, 0.62, 0.78, 0.78])

A failing check on one of the larger volumes allows us to discard the body part(s) inside the volume completely. By using a carefully crafted configuration, only a small number of tests are needed to decide if the instance should stay on the list for deeper checks, or if it can be ignored.

For details about collision checking between convex objects, you can look at the Gilbert–Johnson–Keerthi (GJK) distance algorithm. The GJK algorithm reduces the collision detection complexity by using the so-called Minkowski difference. A link to a web page explaining the principle of the GJK algorithm is available in the Additional resources section.

For the sake of simplicity, because of the fast updates, and also based on setting a limit to the number of instances to check, we will implement a quadtree into the application and bounding spheres for the instances. The quadtree will allow us to do the first step of collision detection