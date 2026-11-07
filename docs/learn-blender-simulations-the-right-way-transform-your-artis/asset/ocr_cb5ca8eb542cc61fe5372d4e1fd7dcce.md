6. Now, let's add physics to the rest of the objects! Select **Platform 1** and **Platform 2**. Give them both a **Passive** rigid body type, with **Shape** set to **Mesh** and **Margin** to 0 m.

![](bbox=[0.22, 0.14, 0.74, 0.44])

Figure 11.21 – The platform settings

7. If the **Sphere** object does not make it all the way around the circle, you can set **Friction** in the surface responses of **Platform 1** to around 0.1. This will make the sphere slide a bit more.

With that done, let's move on to creating the **Hinge** constraints!

# Creating the hinges

The last obstacle for the sphere to go through, before it crashes into the tower of cubes, is a series of walls that rotate using the **Hinge** constraint. To add this, do the following:

+   1. Select the **Wall 1** object and give it an **Active** rigid body.
+   2. We want the **Sphere** to easily push the wall up, so let's set **Mass** of **Wall 1** to 0.001 kg.

![](bbox=[0.22, 0.71, 0.74, 0.87])

Figure 11.22 – The wall settings