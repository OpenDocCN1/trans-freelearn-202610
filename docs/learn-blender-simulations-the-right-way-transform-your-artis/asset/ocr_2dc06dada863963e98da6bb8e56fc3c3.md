# Dynamic weight painting

Let's begin by opening the `Burning Effect Setup.blend` file, whose link you can find in the *Technical requirements* section. This file includes a plane, a couple of area lights, and some render settings already in place. This plane object is going to be our canvas, cloth, and inflow object for this simulation.

We'll start out by creating a new brush object to paint weight on the canvas. Then, from there, we'll use a new vertex group to mask out the canvas and determine where the fire should be. That will create the look of the **Plane** object burning up!

Let's go ahead and get started by creating the first simulation for this project, dynamic paint!

+   1. We already have the canvas for our dynamic paint simulation so now we need to add a brush object. Press *Shift + A* and then click on **Mesh | Icosphere**.
+   2. Before you do anything else, make sure to open the menu at the bottom left and set **Subdivisions** to 6, which will increase the amount of geometry on the icosphere, and set **Radius** to 1.5.

![](bbox=[0.25, 0.41, 0.75, 0.68])

Figure 15.1 – Add Ico Sphere menu

The next step is to have this icosphere paint weight on the canvas, which we will use later to determine where the fire appears on the plane. To do this, we need to animate the icosphere moving up the plane. Here are the steps!

+   I. Position the icosphere beneath the plane object.
+   II. On **Frame 1** press *K* and, add a **Location** keyframe to the brush.