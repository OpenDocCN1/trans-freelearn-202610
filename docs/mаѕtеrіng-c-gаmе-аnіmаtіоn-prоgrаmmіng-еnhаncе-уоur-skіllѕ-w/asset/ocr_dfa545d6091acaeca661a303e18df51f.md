# 12

# Advanced Collision Detection

Welcome to *Chapter 12*! In the previous chapter, we added the ability to load static level data to the virtual world. Now the instances no longer have to run in thin air; they can get a virtual home. First, we explored the differences between model and level data, which file formats are mostly used for level data, and where to find game maps on the internet. Then, we added code to load level data and replaced the two-dimensional quadtree with a three-dimensional octree. Finally, we implemented the level data rendering, including debug data for wireframe, octree, and level AABB lines.

In this chapter, we will extend the level data from the previous chapter. First, we will add a specialized octree for the level data and update the code to also support collision detection between instances and level geometry. Then, we will add a simplified form of gravity to the virtual world to keep the instances at ground level instead of having them floating around. As the last step, we will introduce inverse kinematics for the instance legs to allow the instances to climb slopes and stairs with more natural leg motions and to prevent the feet from clipping into the ground or floating in the air.

In this chapter, we will cover the following topics:

+   - Enhancing collision detection for level data
+   - Using gravity to keep the instances at floor level
+   - Adding inverse kinematics