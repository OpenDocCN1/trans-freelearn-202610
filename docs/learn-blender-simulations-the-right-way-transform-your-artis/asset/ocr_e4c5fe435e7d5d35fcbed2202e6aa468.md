# Technical requirements

This chapter requires that you have Blender version 4.2 or above installed. To download Blender, visit www.blender.org.

You can find the assets for this chapter in this book's GitHub repository: https://github.com/PacktPublishing/Learn-Blender-4-Simulations-the-Right-Way/tree/main/Chapter%201.

# What is Mantaflow?

Mantaflow is the framework in Blender for simulating gas and liquid. Mantaflow was introduced in Blender 2.82 and has seen many updates since then. It was first developed back in 2009 at the ETH Computer Graphics Laboratory. Now, it's being maintained and developed by the Thurey group at the Technical University of Munich (TUM).

In Blender version 2.81 and below, the smoke and fluid simulations were two completely different things, and they weren't very compatible with each other. When 2.82 came out, the Blender developers removed these two simulations and introduced Mantaflow. This was a much better system because it combined both the fluid and smoke simulations into one.

To enable Mantaflow on an object, you need to head over to the Physics properties area (it will look like a circle with a dot in the middle) and select Fluid:

![](bbox=[0.32, 0.55, 0.65, 0.62])

Figure 1.1 – Blender's Physics properties area

Don't get confused when you only see Fluid in Blender's Physics panel. Fluid is just the name used for the overall simulation. Once you select it, you'll be able to choose between Gas or Liquid.

Now that we've covered what Mantaflow is, let's take a look at the simulations you can create with it in Blender!

# Gas and liquid simulations

You've seen me throw around the terms gas, liquid, and fluid, but what are the differences? As mentioned previously, Fluid is used to describe the entire simulation. Inside the simulation, there are two options to choose from:

+   - Gas: Used to create things such as smoke, fire, or airborne solids
+   - Liquid: Used to simulate things such as water or honey