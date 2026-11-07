
## Settings

When you assign a mesh to be a domain object, here are the settings you will see:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_43_0.png)

Figure 2.2 – Domain settings

Let’s go through each one; I will describe exactly what it does:

- **Domain Type**: This allows you to choose between **Gas** and **Liquid**.
- **Resolution Divisions**: This controls how good the simulation will look. We mentioned this setting in *Chapter 1*, but let’s review it again. Higher values will result in smaller voxels, making the simulation look better but run slower. The bigger the domain, the higher you need to set this value. If you double the size of your domain, you will also need to double the size of the resolution to get the same look.
- **Time Scale**: This controls the speed of the simulation. Higher values will result in faster simulations and lower values will give the look of slow motion. If you set this setting to 0, the simulation won’t stop completely; there will be a tiny amount of motion.
- **CFL Number**: This stands for Courant–Friedrichs–Lewy; it is a little bit technical. When baking a simulation, there is a certain number of *time steps per frame*. Time steps are how many times the solver will calculate the simulation per frame. The more velocity an object has, the more time steps it will need to be physically accurate. Higher values will result in fewer time steps and faster bake times, which will give less accurate simulations. Lower numbers will give the simulation more time steps, thus increasing the bake time but giving better results. Leaving it at the default value of 2 will work for most simulations.
- **Use Adaptive Time Steps**: This checkbox will have Blender automatically decide when to give more steps per frame. It’s usually a good idea to have this checked; that way, when there are fast-moving fluids, Blender will add more time steps, and when there are slow-moving fluids, you won’t need as many time steps. This will help decrease the bake time.

- **Timesteps Maximum and Minimum**: These values control how many steps there will be per frame of the simulation. Usually, if you have collisions that aren't working properly or smoke/ fluid that is moving too fast, you will want to turn these numbers up.
- **Using Scene Gravity**: This, just like in real life, gives the scene some physics and allows the fluid to fall. You can control the values here so that instead of falling, the fluid might fall to the side or even upside down if you set Z to a positive value.

You will notice that this is currently grayed out in Figure 2.2. This is because the scene is using Blender's default gravity. You can turn this off by heading to the Scene panel and unchecking **Gravity**:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_44_0.png)

Figure 2.3 – Gravity

Now, you can change the gravity in the domain settings. This can be useful if you want multiple domains with different gravity settings. Keep in mind that if you uncheck **Gravity** in the Scene panel, other physics, such as particle systems and rigid bodies, won't have gravity.

- **Empty Space**: This is for gas domains only. It is used to get rid of smoke as it gets very thin. This will help with render times and baking times:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_44_1.png)

Figure 2.4 – Empty Space example

You will notice that with **Empty Space** set to 0.7, the top and sides of the simulation have been clipped off. Make sure that this is set very low; otherwise, you might delete some smoke that you don’t mean to.

- **Delete in Obstacle**: This will delete any fluid/smoke inside an obstacle if this is checked.
- **Border Collisions**: This is a way to have the smoke/fluid collide with the edges of the domain. The options in this tab that can be checked and unchecked are **Front**, **Back**, **Right**, **Left**, **Top**, and **Bottom**. If left unchecked, the smoke will pass out of the domain without colliding and disappear.
- **Adaptive Domain**: This is used for gas domains only. This option is great and should almost always be checked. It allows the size of the domain to change dynamically to fit the simulation:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_45_0.png)

Figure 2.5 – Adaptive Domain example

There are three settings that you can change here:

- **Add Resolution**: This will expand the domain past its original size if the smoke reaches the edge.
- **Margin**: This is the amount of space between the domain and the smoke. Lower values will bring the edge of the domain closer, while higher values will push it away.
- **Threshold**: This is another way to clip the smoke. If the density of the smoke is smaller than the threshold, **Adaptive Domain** will cut it off, deleting that smoke from the simulation.

**Adaptive Domain** is very useful and is a huge time-saver when baking a simulation! In some rare cases, this option can cause problems with smoke clipping or importing OpenVDBs, but most of the time, you will want to have this turned on.

That is all the settings for the first panel of the domain. Now, before we jump into the smoke and liquid options, I want to mention the Cache panel!

## Cache

The Cache panel is the directory that's used to set where all the smoke/fluid data will be stored when baked in. The default directory is a temporary folder. This means that the simulation data will be deleted after you close your project. In some cases, this might be useful because simulations can take up a lot of hard drive space. In other cases, it might be annoying having to rebake the simulation every time you reopen the project.

To save your data so that it doesn't get deleted, you need to click on the little folder icon and select a different folder. Usually, it's a good option to put the cache in the same folder as your project blend file:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_46_0.png)

Figure 2.6 – The Cache panel

Below the directory, we have Frame Start and End, which control when the simulation will start and when it will end, respectively. The Offset parameter allows you to delay the start of the simulation. For example, setting Offset to 50 will allow the simulation to start at frame 50 instead of 1. This can also be changed after the simulation has finished baking.

In the Type dropdown menu, there are three ways to bake the simulation:

- **Replay**: This bakes the simulation in real time as you are playing it in the viewport. This can be useful when testing out settings at a low resolution.
- **Modular**: This splits up the domain into different modules that need to be baked separately. If you select this type, the noise, guide, mesh, and particle panels will all need to be baked individually. This is also useful when you want to change something in one of these panels without rebaking the entire simulation.
- **Final**: This, unlike Modular, will bake everything all at once.

With the **Modular** or **Final** type selected, the **Bake** button will appear. Once you click **Bake**, the simulation will start the baking process and you will be able to see the progress by looking at the percentage at the bottom of the screen:

![](img/2bdfac576180ebfe5ee8960a6eec7f99_47_0.png)

Figure 2.7 – Bake progress

When the simulation has finished baking, the settings will be grayed out and you won’t be able to change anything. However, you can click **Free Data**, which will delete the bake, allowing you to change the settings.

The **Is Resumable** checkbox allows you to cancel the bake halfway through and resume it later. I almost always keep this option on just in case I need to stop the bake. One thing to keep in mind is that enabling this will cause more simulation data to be written, thus increasing the bake size.

Finally, the two **Format Volumes** options that store all the data are **Uni Cache** and **OpenVDB**:

- **Uni Cache**: This is Blender’s way of storing the simulation data. Each object in the simulation will have its own Uni Cache file (`.uni`).
- **OpenVDB**: This will store all the objects in the simulation in one OpenVDB file per frame, instead of having every object have its own `.uni` file. This is more efficient, and most of the time, I will be using this format.

Now that we understand what baking a simulation in Blender looks like, let’s discuss all the settings for the gas and liquid domains!

## Gas domains

When creating a gas domain, there are a lot of options that differ from liquid domains, such as the density of the smoke, when the smoke will dissolve, and the height of the flames. In the upcoming chapters, we will learn how to use gas domains to create realistic flames and explosions.

![](img/2bdfac576180ebfe5ee8960a6eec7f99_47_1.png)

Figure 2.8 – Gas settings
