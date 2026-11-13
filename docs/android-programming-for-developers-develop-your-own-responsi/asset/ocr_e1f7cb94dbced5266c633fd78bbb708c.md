# Optimization

Checking for collisions is an algorithm of quadratic complexity. If the number of objects to be checked grows, it may quickly become a bottleneck.

One option is to keep a record of the previous state of the comparison between the objects, and then either return the old value if both objects have not changed since the last check or use the old state as a parameter to the calculation of the new one. This is particularly handy for 3D games, where collision detection requires much more complex algorithms.

There is another optimization called spatial partitioning that exploits the proximity of the objects. This technique is based on the idea that objects can only collide with other elements that are close to them on the screen. Pretty obvious, right?

To implement spatial partitioning, we are going to use a data structure known as QuadTree.