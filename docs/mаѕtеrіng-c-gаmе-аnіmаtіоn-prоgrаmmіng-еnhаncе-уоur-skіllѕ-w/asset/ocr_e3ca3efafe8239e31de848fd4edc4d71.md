change in almost every frame. In such an environment, instance removal and addition will be more expensive than clearing the quadtree and adding all instances with fresh data.

Starting over with a fresh quadtree in the draw() call of the renderer is easy. Right before going into the instance loop, clear the contents of the quadtree:

```
mQuadtree->clear();
```

After the quadtree is ready, we have to solve the question of how to generate the bounding boxes for the instances. The answer may be surprising: We need to split the work between the GPU and the CPU.

# Calculating the instance bounding boxes

The first idea when it comes to a large number of calculations for all instances will most probably be “I will use a compute shader!”, especially after we have solved some compute-intensive problems by offloading the work to the GPU. But in this case, the CPU must do a part of the job.

While compute shaders are great for calculating independent results in a massively parallel way, they aren’t well suited for simple tasks like creating a bounding box. To calculate the bounding box of an instance, we need to store the minimum and maximum coordinates of all nodes for every instance in the virtual world, combining the intermediate results to create the final bounding box. On the CPU, we can easily grab the vertex positions from the data structures one by one, do the required calculations, and store the final AABB back in memory.

But if we would like to compute the AABB coordinates inside a shader, we may end up overwriting the results of other shader invocations if the AABB results use the same buffer positions. Or we will have a huge list of coordinates that must be sorted afterward. Sorting the coordinates could be done in another computer shader too, but parallel sorting algorithms tend to be complex because they face the same constraints of not being able to write data to the same buffer positions.

So, instead of worrying about data synchronization issues in compute shaders, we use them for what they can do very fast: creating lookup tables.

# Adding a three-dimensional bounding cube class

To store the generated lookup data, we will add a C++ class for a three-dimensional AABB. Using AABBs here has the advantage of having data to draw debug lines in the renderer, but the third set of coordinates adds only a little overhead to storage and generation time. Also, a possible upgrade to an octree for a fast collision check in all three dimensions will be simple.