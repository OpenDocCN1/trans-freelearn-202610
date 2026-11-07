+   - **Timesteps Maximum and Minimum**: These values control how many steps there will be per frame of the simulation. Usually, if you have collisions that aren't working properly or smoke/ fluid that is moving too fast, you will want to turn these numbers up.
+   - **Using Scene Gravity**: This, just like in real life, gives the scene some physics and allows the fluid to fall. You can control the values here so that instead of falling, the fluid might fall to the side or even upside down if you set Z to a positive value.

You will notice that this is currently grayed out in Figure 2.2. This is because the scene is using Blender's default gravity. You can turn this off by heading to the Scene panel and unchecking **Gravity**:

![](bbox=[0.16, 0.27, 0.83, 0.41])

Figure 2.3 – Gravity

Now, you can change the gravity in the domain settings. This can be useful if you want multiple domains with different gravity settings. Keep in mind that if you uncheck **Gravity** in the Scene panel, other physics, such as particle systems and rigid bodies, won't have gravity.

+   - **Empty Space**: This is for gas domains only. It is used to get rid of smoke as it gets very thin. This will help with render times and baking times:

![](bbox=[0.22, 0.57, 0.78, 0.82])

Figure 2.4 – Empty Space example