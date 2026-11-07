For most situations, you don’t need to change these settings, so just stick with **Average**.

+   - **Stiffness**: This controls how **Self Collision** works when a vertex enters another vertex collision ball. With it set at a high value, it will push the vertex out very quickly. When it’s set at a lower value, it will push the vertex out slowly.
+   - **Dampening**: This is used to slow down a vertex as it gets close to another vertex collision ball. High values will slow it down very quickly.

Most of the time, all you have to do is enable **Self Collision** and all the default settings will work just fine. Keep in mind the simulation will take longer to calculate, so only enable **Self Collision** when it’s needed.

# Solver

The **Solver** tab helps the simulation be more accurate. Sometimes, with fast-moving collisions, the simulation will have a hard time calculating, and you may get a couple of errors and issues with collisions. However, in this tab, you will be able to change the settings in order to get a more accurate simulation. You can see these settings in the following screenshot:

![](bbox=[0.13, 0.45, 0.87, 0.79])

Figure 6.32 – Solver settings