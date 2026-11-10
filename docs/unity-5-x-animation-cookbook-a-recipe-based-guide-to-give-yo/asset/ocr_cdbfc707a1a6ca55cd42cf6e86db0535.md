# Getting ready

Use the same character as in the previous recipe. Add a 180-degree turn animation, starting with the Idle pose and ending with the Idle pose rotated by 180 degrees in the vertical axis. Name the animation `180Turn` for clarity. You can open the provided example Unity project and go to the `Chapter 04 Character movement\Recipe 05 Using root motion for a 180 degrees turn` directory. You will find an `Example.unity` scene there. Play the game and try to walk in the direction opposite to the one the character is facing (press the down arrow). You can find the `Humanoid.fbx` asset in the `Rigs` directory with all the required animations.

# How to do it...

To make a 180-degree turn, follow these steps:

+   1. Create four transitions (see the following screenshot):
+   2. Drag and drop your **180Turn** animation into the Animator Controller to create a new state.
+   3. Open the Animator Controller (this should be the same controller as in the previous recipe), add two float parameters, and call them `DirectionRaw` and `SpeedRaw`.
+   4. Check the **Bake Into Pose** option for the **Root Transform Position (Y)** in the **180Turn** animation.
+   5. In the **Animation Import Settings** of the character, find your additional **180Turn** animation. You may need to add it as a new clip by clicking on the plus button in the **Clips** section (**Animations** tab) and choosing the **180Turn** animation as the source.
+   6. Import the character and repeat all the steps from the *Using root motion to steer a character* and *Using animations for better looking transitions* recipes to have a character that is able to walk.
+      - **Idle | 180Turn** with two conditions: The **SpeedRaw** parameter should be greater than 0.5 and the **DirectionRaw** parameter should be greater than 160. **Has Exit Time** should be set to false and **Transition Duration** should be set to around 0.1 seconds.
+      - **Idle | 180Turn** (a second one) with two conditions: The **SpeedRaw** parameter should be greater than 0.5 and the **DirectionRaw** parameter should be less than -160. **Has Exit Time** should be set to false and **Transition Duration** set to around 0.1 seconds. We have two transitions between the same states because we want them to work as a