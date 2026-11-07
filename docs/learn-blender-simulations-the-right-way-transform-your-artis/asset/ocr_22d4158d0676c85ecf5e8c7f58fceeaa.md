![](bbox=[0.115, 0.09, 0.85, 0.36])

Figure 13.10 – Hair Dynamics

5. In order for the hair to collide with the plane, we need to give the plane object a **Collision** modifier. Select the plane object, head over to the **Physics** panel, and select **Collision**. Also, make sure to set **Thickness Outer** to 0.001 so there is not a big gap between the hair and the plane.

![](bbox=[0.24, 0.47, 0.73, 0.72])

Figure 13.11 – Collision modifier

6. Next up, let's change a couple of settings in **Hair Dynamics** to get a nice paintbrush look:

+   I. Change **Quality Steps** to 10. This will give us a better overall result.
+   II. In the **Collisions** tab, let's set **Quality** to 4 and **Distance** to 0.001.