18. Make sure the record button is pressed and select the **BlackScreen** game object. Set the **Right** and **Left** positions to 0. Enable the **BlackScreen** game object (make sure a **Game Object.Is Active** property shows in the **Animation** View, and, if not, click on the game object's **enable** checkbox twice). This Animation Clip should also have only one key frame on the frame 0; remove any other key frames. The whole **Game** View should show a black screen.

19. Select the **FadeAnimator** game object again and create another animation. Name it **FadeIn**. With the record button pressed, select the **BlackScreen** game object and create two key frames: one on frame 0 and one on 1-second mark. The **BlackScreen** game object's position on frame 0 should be set to the same values as in the **Alpha_0** animation (**Right** to −20 and **Left** to a number bigger than the **Game** View screen resolution). On the 1-second frame, the position should be set to **Left** 0, **Right** 0. Also enable the game object in both frames. When you play the animation in the **Animation** View, you should see the **BlackScreen** game object swipe from right to left (from a clear game screen to a complete black screen).

20. Select the **FadeAnimator** game object again and create another animation. Name it `FadeOut`. With the record button pressed, select the **BlackScreen** game object and create two key frames: one on frame 0 and one on 1-second mark. The position of the **BlackScreen** game object in frame 0 should be set to Left 0 and Right 0. In the 1-second mark frame, the **Left** position should be set to −20 and the **Right** position to a number higher than the **Game** View's screen resolution. Also make sure to enable the **BlackScreen** game object in both key frames. When you play the animation in the **Animation** View, you should see the **BlackScreen** game object swipe from right to left (from a full black screen to a clear game screen).

21. Open the Animator Controller assigned to the **FadeAnimator** game object.

22. Create a `Trigger` parameter and call it **Fade**.

23. Create a loop of transitions:

+   - **Alpha_0** | **FadeIn** with the trigger **Fade** set as condition and **Has Exit Time** set to `false`
+   - **FadeIn** | **Alpha_100** with no conditions and **Has Exit Time** set to `true`
+   - **Alpha_100** | **FadeOut** with the trigger **Fade** set as condition and **Has Exit Time** set to `false`
+   - **FadeOut** | **Alpha_0** with no conditions and **Has Exit Time** set to `true`

24. Set every **Transition Duration** to 0.