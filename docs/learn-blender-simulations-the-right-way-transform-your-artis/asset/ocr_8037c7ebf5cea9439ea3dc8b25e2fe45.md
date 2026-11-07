Let's take a look at these in more detail:

+   - **Mass**: This is where you set how much the rigid body should weigh in the simulation. As you can probably guess, the weight of the object will affect how it interacts with other rigid bodies and forces. You can type this number in manually or you can let Blender calculate it automatically. In the **Rigid Body Object Menu** area, you can select **Calculate Mass**. This will bring up a large menu containing all the different types of real-world materials:

![](bbox=[0.385, 0.22, 0.58, 0.38])

Figure 10.7 – The Calculate Mass menu

Mass is also calculated based on the size of the mesh. For example, if a cube is 2 x 2 meters and you select **Gold** for the calculation, Blender will treat the cube as a completely solid block of gold and **Mass** is going to be set at 154256 kg or 340076 lb. This is quite heavy, so make sure you choose the right mass for your simulation.

+   - **Dynamic**: This allows you to turn the rigid body object off and on. This setting can also be animated, which can be useful if you want the rigid body to stop interacting with the simulation at a certain point in the timeline.

+   - **Animated**: This setting allows you to move the rigid body object around using animation data and keyframes. And it will still interact with the simulation! This is extremely useful and can be applied to many simulations. The other great thing is that the **Animated** checkbox in *Figure 10.6* can also be toggled off and on using animation keyframes. For example, you can have a cube fly into other cubes using location keyframes:

![](bbox=[0.285, 0.66, 0.68, 0.83])

Figure 10.8 – Cube Animated on