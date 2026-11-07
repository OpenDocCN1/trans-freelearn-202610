4. For now, uncheck **Goal** so the object doesn't float. Later in this chapter, we will re-enable it and then animate some of the settings and values to control how **Goal** operates in the simulation.

5. On the **Edges** tab, let's change a few settings. To give the sphere some structure, set the **Push** and **Pull** values to 0.6. When the sphere collides with something, it's going to have a lot of movement and jitteriness. To calm it down a bit, let's set **Damp** to 40. To prevent the sphere from collapsing and losing its structure, set **Bending** to 2.5.

![](bbox=[0.295, 0.21, 0.67, 0.38])

Figure 7.2 – Edges settings

With that done, let's move on to collision objects!

# Adding collisions

If we play the animation, the sphere will pass through all the objects. This is not what we want, so let's add collisions to everything in the course:

+   1. Select **Cylinder 1**, which is the top cylinder that the sphere is inside of, and give it collision by selecting **Collision** in the **Physics** panel.
+   2. Next, head over to the **Modifier** panel and make sure the **Collision** modifier is above the **Solidify** modifier or you will get some clipping issues. You can move modifiers up or down by dragging the dots on the right side. You will have to do this for most objects in this obstacle course:

![](bbox=[0.25, 0.64, 0.715, 0.785])

Figure 7.3 – Modifier stack