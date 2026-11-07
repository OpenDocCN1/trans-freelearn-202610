Now, let’s create the cloth simulation!

+   5. Select your flag and head over to the **Physics** panel.
+   6. Now, select **Cloth**. We are going to start at the top and work our way down the settings, changing them as needed. The first thing we’ll change is **Quality**. This will increase the accuracy of the simulation and help prevent any errors. Since the ropes and the flag will be interacting with each other, we need to set **Quality** to a high value so that we get a nice simulation. Let’s go with a value of 30.
+   7. Our cloth currently weighs way too much, and the wind is hardly going to affect it. To fix this, let’s set **Vertex Mass** to 0.03 kg.
+   8. Since the wind strength is so high, let’s set **Tension** in the **Stiffness** tab to 25 to make sure that the cloth doesn’t stretch.
+   9. As for **Bending** in the **Stiffness** tab, set this value to 0.4 so that we get slightly smaller wrinkles and bends.

![](bbox=[0.25, 0.39, 0.71, 0.75])

Figure 9.19 – Stiffness values

10. Now, let’s create the pin group so that the flag stays in place and doesn’t just fall straight down. For this, we need to create a vertex group and assign some of the vertices to it.