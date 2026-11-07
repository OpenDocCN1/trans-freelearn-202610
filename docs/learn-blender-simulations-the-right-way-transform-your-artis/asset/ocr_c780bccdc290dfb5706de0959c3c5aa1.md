There are two types of effectors in Mantaflow: **collisions** and **guides**. We learned about guides earlier, so let's select **Collision**. Let's go over the four settings:

+   - **Sampling Substeps**: This is very similar to **Substeps** for the flow object. If you have a fast-moving collision, it's recommended to turn this value up; otherwise, you might get some inaccuracies.
+   - **Surface Thickness**: This is the area around the object that will be considered a collision. Higher values will result in fluid colliding further from the surface.
+   - **Use Effector**: This enables or disables the effector. This can be useful for animating the collisions and turning them off and on.
+   - **Is Planar**: Again, if your effector is non-manifold, then make sure **Is Planar** is checked. This will help Blender get more accurate collisions:

![](bbox=[0.225, 0.34, 0.745, 0.71])

Figure 2.60 – Collision example

If you have multiple objects you want to add an effector collision to, the easy way to do that is to select all of them. Then, with an active object selected, click **Fluid | Type | Effector**. Then, press *Ctrl + L* or *Cmd + L* and select **Copy Modifiers**. That will copy the settings you have and apply them to all the objects selected.

And there you have it; those are all the settings in the effecter collision in Mantaflow!