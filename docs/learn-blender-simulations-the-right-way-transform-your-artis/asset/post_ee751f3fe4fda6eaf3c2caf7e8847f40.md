
- **Inner:** This is the collision area for the inside of the object. This is basically the padding distance, and if a soft body enters this area, it will try to force it out. You can see an illustration of this here:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_152_0.png)

Figure 6.8 – Plane collision area

An example of this would be to place a soft body in the middle of a plane. Since half of the soft body is inside the **Inner** zone, it will push it up and out. Keep in mind that this does not work for a cloth simulation, only for soft bodies. Here’s a visual example of this:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_152_1.png)

Figure 6.9 – Inner zone example

Another thing to note is that a collision is based on the direction of **normals**. The normals of an object are the direction in which the faces are pointed. If you notice that the collision object is not working properly, you may want to recalculate the normals and see which direction the normals are pointed in.

To recalculate the normals, go into **Edit Mode** with the object selected. Then, press A to select all vertices on the mesh, and from there, go up to **Mesh | Normals | Recalculate Outside** or press the *Shift + N* shortcut. You can also turn on **Face Orientation** in the **Overlays** menu to see which direction the faces are pointed in, as illustrated in the following screenshot:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_153_0.png)

Figure 6.10 – Overlays menu

The **Friction**, **Single Sided**, and **Override Normals** settings are used only for cloth simulations, which we will learn about in *Chapter 8*.

With that out of the way, let's jump into a soft body simulation and learn all about the settings and the nitty-gritty details!

## Understanding soft body settings and values

Now that you know how a soft body simulation and collision objects work in Blender, it's time to get into the technical details and learn about all the settings and values and how they can affect the simulation. While going through this chapter, I suggest you test out the settings for yourself so you can visualize how they work. You can download a basic .blend file that I have created for that exact purpose from https://github.com/PacktPublishing/Learn-Blender-4-Simulations-the-Right-Way/tree/main/Chapter%206. When you open this file, you will find just a basic scene with an icosphere and a collision object.

Now, let's talk about the settings!

### Object settings

When you first select Soft Body in the Physics panel, you will see a lot of settings on the right side, as we can see here:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_154_0.png)

Figure 6.11 – Object settings

Let’s start out from the top and work our way down, as follows:

- **Collision Collection**: This allows you to control which collections interact with the simulation. With it set to none, every collision in every collection will interact with the soft body.
- **Friction**: This works a little differently than you would expect. This is the overall friction of the entire area. If you increase this value, it will dampen the movement of the soft body, making it look like it’s moving in slow motion.
- **Mass**: This controls the weight of each vertex on the object. In the following screenshot, the left example has a Mass value of 1 kg, while the right has a Mass value of 10 kg. You will notice that it’s collapsing in on itself because the weight is too heavy to maintain the structure of the object:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_154_1.png)

Figure 6.12 – Mass example

- **Control Point**: This allows you to use a **vertex group** to control the **Mass** value of the object. If you have never used a vertex group before, it basically allows you to paint values from 0 to 1 on each vertex. In this example, I have set the left side of the cube to a value of 1, which is red, so it's using the full mass. The right side is set to blue, which is 0, so it weighs nothing. As you can see here, it is collapsing in on itself because the left side weighs more (we will talk about vertex groups in more detail later in this chapter):

![](img/2bdfac576180ebfe5ee8960a6eec7f99_155_0.png)

Figure 6.13 – Control Point example

- **Speed**: Finally, we have the option to control the speed of the simulation. This value is based on the **frames per second (FPS)** of the scene. For example, if you have the frame rate set to 60 and the **Speed** value set to 1, it's going to look like the simulation is playing at twice the speed, so you would need to slow the speed to match the frame rate. Consider what you are trying to make, the scale of your scene, and your frame rate, and adjust the speed accordingly.

With that done, let's move on to the **Cache** tab and learn how to bake in the simulation!

### Cache

Below the **Object** settings, we have the **Cache** tab. This is where you bake in the simulation. Some of these options should be familiar because they are the same as with a Mantaflow simulation.

One thing that is different, though, is the ability to have multiple caches. For example, you can click the + button to create another cache, as shown in the following screenshot. Double-click the cache to rename it. You can then select one of them, change the settings as you wish, and bake it in, and it won’t affect the other caches:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_156_0.png)

Figure 6.14 – Multiple caches example

This can be very useful for quickly previewing certain settings in the simulation. To switch between two caches, make sure to restart the Timeline, then select the one you want to preview and hit Play.

We should also discuss the difference between a cache and a bake. The cache is the result of the animation as it’s being played. In the following screenshot, you see an orange line at the bottom of the Timeline—this is the cache:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_156_1.png)

Figure 6.15 – Cache example

Whenever you change a setting in the simulation, the cache will disappear, and you will have to replay the animation to get it back. When you bake, then the result of the simulation is locked in, and you won't be able to change the settings unless you click Free Bake. The Timeline will also be filled with that orange line at the bottom. The following screenshot provides an example of this:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_157_0.png)

Figure 6.16 - Bake example

That is the difference—the cache is a temporary result, which will reset if you make any changes, and the bake is the locked-in result. Now, let's look at the rest of the settings for the Cache tab:

- **Simulation Start and End**: These just allow you to set the frame range of the simulation for baking.
- **Cache Step**: This allows you to skip frames of a simulation when baking. Instead of baking every frame, it will bake every other frame if this value is set to 2. This can save on storage, but you might end up with a less accurate simulation.
- **Bake**: This bakes in the simulation.
- **Calculate to Frame**: This allows you to select any frame in the Timeline, and Blender will calculate the animation up to where you selected.
- **Current Cache to Bake**: This allows you to take the temporary cache and use that as the bake.
- **Bake All Dynamics**: This will bake in every simulation in the scene. If you have multiple soft bodies, particle systems, or cloth simulations, all of them will be baked at once.
- **Delete All Bakes**: This is pretty self-explanatory. It deletes every bake in the scene.
- **Update All to Frame**: This will update all simulations to the current frame you have selected.

Sometimes, a simulation will not work properly if you try to render an animation without baking. So, it's usually a good idea to bake in the simulation before rendering; this way, everything works correctly and there are no problems.

### Soft Body Goal

You might notice that right when you create a soft body object and play the animation, the object just floats and bobs up and down. This is because **Goal** is checked in the settings. The Goal option is basically a way to pin vertices in place. This can be useful when trying to create blankets hanging from a line, corners of a flag pinned to the pole, or characters walking. You can see an example here:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_158_0.png)

Figure 6.17 – Goal example

Since **Goal** is trying to pin the vertices in place, this means any animation data will work for the soft body simulation. You could have an object fly across the scene but still have soft body physics! When **Goal** is enabled, the entire object is going to be *pinned*. If you only want some parts of the object to use **Goal**, you can create a vertex group and paint where you want the pinning to happen.

Let’s learn about vertex groups a bit more and how they work in Blender. As mentioned earlier, vertex groups are used to assign a value to a vertex from a range of 0 to 1. To create a new vertex group, you can manually paint vertices by pressing *Tab* and selecting **Weight Paint**. Now, you can select the value you want at the top and start painting on the mesh.
