# Setting up the render

This tutorial is almost done! The final steps we need to do are setting up the render, animating the camera to follow the paintbrush, and rendering the animation. Let's start by taking a look at the EEVEE settings and changing them to make the whole scene look much better:

+   1. In the **Render** panel, let's turn on **Raytracing** to get nice reflections in the materials. Set **Resolution** to **1:1**.
+   2. In the **Color Management** tab, let's set **Look** to **High Contrast**. This will really make the colors in our scene pop!
+   3. For the **Camera** animation, we recommend jumping to **Frame 1**, positioning **Camera** to the view you want, and then adding a keyframe. You can do this by selecting **Camera**, hitting *I*, and selecting **Location & Rotation**.
+   4. Around **Frame 100**, rotate and move **Camera** so that it's facing the paintbrush, then add another keyframe. Repeat this step at around **Frame 160** and then **250**. Remember, you can always move the keyframes by selecting them in **Timeline** and pressing G.
+   5. If you want to delete a keyframe, select it and press X, and choose **Delete Keyframes**.

![](bbox=[0.27, 0.25, 0.73, 0.45])

Figure 13.40 – EEVEE render settings

![](bbox=[0.27, 0.7, 0.73, 0.87])

Figure 13.41 – Deleting keyframes