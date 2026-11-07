+   4. Click **New** to create a new material. Name the material `Glass` so it's easy to find later.
+   5. In the **Material** panel, scroll down to the **Settings** tab, check **Raytraced Transmission**, and set **Thickness** to **Slab**. This will help the glass look much better.
+   6. In the **Node Editor** workspace, let's delete the **Principled BSDF** shader and add a **Glass** shader. To do this, press *Shift + A* and then go to **Shader | Glass BSDF**.

![](bbox=[0.22, 0.16, 0.76, 0.45])

Figure 7.26 – Material settings

We are starting to get reflections, but we still can't see through the glass so let's fix that!

![](bbox=[0.2, 0.57, 0.8, 0.83])

Figure 7.27 – Basic glass material