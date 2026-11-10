logical OR operator; if any of them is met, the transition will be triggered.

+   - **180Turn | Idle** with one condition: **Speed** parameter less than 0.5. **Has Exit Time** should be set to true and **Transition Duration** set to around 0.1 seconds.
+   - **180Turn | Steering** with one condition: **Speed** parameter greater than 0.5. **Has Exit Time** should be set to true and **Transition Duration** set to around 0.1 seconds.

![](bbox=[0.1, 0.29, 0.9, 0.53])

Animator Controller with additional 180Turn transition animation

+   7. Edit the **Idle | Steering** transition by adding two more conditions: **DirectionRaw** parameter greater than -160 and **DirectionRaw** parameter less than 160. Multiple conditions in the same transition work as an AND logical operator; all of them have to be met to trigger the transition.
+   8. The Animator Controller is set up, but we need to set the **SpeedRaw** and **DirectionRaw** parameters from scripts. We've created those two additional parameters because we need raw values here (not damped in time). First, open the script in which we calculate and set the `direction` and `speed` parameters. In this example, it is called `RootMotionSteering.cs`; you can find it in the `Chapter 04 Character movement\Recipe 03 Using root motion to steer a character\Scripts` directory.
+   9. We have two `float` variables there: `direction` and `speed`. They are being calculated every frame. We just need to make them `public` to be able to read them in another script (they are already public in the example script).