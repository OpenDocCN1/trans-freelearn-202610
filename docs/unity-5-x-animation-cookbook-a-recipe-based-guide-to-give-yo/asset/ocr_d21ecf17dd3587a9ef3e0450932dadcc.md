+   4. Create a new game object and name it `FogAnimator`.
+   5. Attach the `FogAnimator.cs` script to it.
+   6. Select the **FogAnimator** game object and add an Animator component to it.
+   7. Open the **Animation** View.
+   8. Create a new Animation Clip.
+   9. Make sure the record button is pressed.
+   10. Create an animation for public float `fogDensity` and public Color `fogColor` parameters by changing their values.
+   11. You can create any number of animations and connect them in the automatically created Animator Controller with transitions based on the **ChangeFog** Trigger (you need to add this parameter to the controller first).
+   12. The following is an example controller:

![](bbox=[0.22, 0.4, 0.78, 0.62])

Example controller for different fog animations

+   13. Remember that you don't need to create animations of the fog changing its color or density. You can rely on blending between animations in the controller. All you need to have is one key for the density and one for the color in each animation. In this example, all **Transition Durations** are set to 1 second and every transition's **Has Exit Time** parameter is set to false.
+   14. Make sure fog is enabled in the **Lighting** settings.
+   15. Play the game and press the space bar to see the effect.