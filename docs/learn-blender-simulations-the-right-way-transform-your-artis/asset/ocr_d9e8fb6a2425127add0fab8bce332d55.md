# Settings

When you assign a mesh to be a domain object, here are the settings you will see:

![](bbox=[0.115, 0.135, 0.85, 0.295])

Figure 2.2 – Domain settings

Let’s go through each one; I will describe exactly what it does:

+   - **Domain Type**: This allows you to choose between **Gas** and **Liquid**.
+   - **Resolution Divisions**: This controls how good the simulation will look. We mentioned this setting in *Chapter 1*, but let’s review it again. Higher values will result in smaller voxels, making the simulation look better but run slower. The bigger the domain, the higher you need to set this value. If you double the size of your domain, you will also need to double the size of the resolution to get the same look.
+   - **Time Scale**: This controls the speed of the simulation. Higher values will result in faster simulations and lower values will give the look of slow motion. If you set this setting to 0, the simulation won’t stop completely; there will be a tiny amount of motion.
+   - **CFL Number**: This stands for Courant–Friedrichs–Lewy; it is a little bit technical. When baking a simulation, there is a certain number of *time steps per frame*. Time steps are how many times the solver will calculate the simulation per frame. The more velocity an object has, the more time steps it will need to be physically accurate. Higher values will result in fewer time steps and faster bake times, which will give less accurate simulations. Lower numbers will give the simulation more time steps, thus increasing the bake time but giving better results. Leaving it at the default value of 2 will work for most simulations.
+   - **Use Adaptive Time Steps**: This checkbox will have Blender automatically decide when to give more steps per frame. It’s usually a good idea to have this checked; that way, when there are fast-moving fluids, Blender will add more time steps, and when there are slow-moving fluids, you won’t need as many time steps. This will help decrease the bake time.