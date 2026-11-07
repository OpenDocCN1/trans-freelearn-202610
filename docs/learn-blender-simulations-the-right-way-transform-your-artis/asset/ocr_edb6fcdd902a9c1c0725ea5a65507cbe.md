Now, the texture will move around as the animation plays.

9. Now that we’ve created the first inflow, let’s copy all of those settings and apply them to the rest of the objects. Make sure the objects you want to assign are selected and that the object with the inflow settings is selected last. If done correctly, the last object will be highlighted alone in a different yellow/orange color. This will make sure that it’s the *active object*.

> **What’s an active object?**
> When you have multiple objects selected in Blender, there will be one that has a yellow outline.
> This is your active object. The rest of the objects will have an orange outline.

Then, press *Ctrl + L* or *Cmd + L* and click **Copy Modifiers**. Now, all of those objects will have the same settings we just created! The other way to do this is by going to **Object | Link/Transfer Data | Copy Modifiers**.

10. The preceding step only copied the settings for the inflow – it didn’t copy the driver we created in *step 8* for the texture offset. What we need to do is copy the animation data of the texture. Press *Ctrl + L* or *Cmd + L* and select **Link Animation Data**. Now, all of the objects will have the same driver we created earlier.

And there we go; we’ve successfully created all of the inflow objects. The next thing we need to do is assign some effector objects. We don’t want the fire to go through the logs, so let’s assign some collisions!

# Adding effectors

This part is pretty simple – all we need to do is assign an effector collision to all the logs that aren’t inflows, as follows:

+   1. Select **Cylinder.007** through **Cylinder.014** in the **Outliner** area (at the top right of the screen), as follows:

![](bbox=[0.21, 0.67, 0.74, 0.88])

Figure 3.8 – Effector selections