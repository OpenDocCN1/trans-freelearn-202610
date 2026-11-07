You should already know how all this works because we went over it in Chapter 6, but here are some quick notes just in case you wish to recapitulate:

+   - **Simulation Start** and **End** control the frame range of the simulation.
+   - To bake in the cloth, all you have to do is click on **Bake**. This will gray out all the settings, and you won’t be able to change them unless you click **Free Bake**.
+   - If you have multiple clothes, soft bodies, or even particle systems and you want to bake everything at once, just click **Bake All Dynamics**. This will then bake every simulation in the scene at once.
+   - The great thing about baking in the simulation is that you can skip to any frame in the Timeline and preview what the cloth will look like in that frame. Without baking the cloth simulation, you won’t be able to skip any frames; rather, you will have to play the simulation from the start.

As for the rest of the settings, since we have already discussed them in Chapter 6, let’s go ahead and move on to the Shape tab!

# Shape

The **Shape** tab allows you to pin vertices in place. This can be very useful for creating things such as flags, a towel hanging from a hook, a hammock, or anything where cloth is pinned to something.

To get the cloth to be pinned in a certain place, first go into **Edit Mode** and select the parts of the mesh you want to pin, as illustrated in the following screenshot:

![](bbox=[0.17, 0.52, 0.83, 0.84])

Figure 8.23 – Selected vertices