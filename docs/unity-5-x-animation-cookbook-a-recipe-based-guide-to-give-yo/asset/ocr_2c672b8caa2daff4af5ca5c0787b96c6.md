To *offset* an animation, you need to go through the following steps:

+   1. Select the animated asset and go to the **Animations** tab.
+   2. Select your animation and make it loop (**Loop Time** checkbox).
+   3. Enter a value in the **Cycle Offset** field, below the **Loop Pose** checkbox.
+   4. Click on the **Apply** button.

To *mirror* an animation, you need to go through the following steps:

+   1. Select the animated **Humanoid** asset and go to the **Animations** tab.
+   2. Find the **Mirror** checkbox on the bottom of the animation settings.
+   3. Check the **Mirror** checkbox and click on the **Apply** button. The animation is mirrored.
+   4. Mirroring animations works only for **Humanoid** rigs.

# How it works...

+   - **Looping animations**: This is a common technique used for all cyclic movements (walk and run cycles, idle animations, and so on). If you don't set an animation to loop, it will play once and freeze on the last frame.
+   - **Offsetting animations**: Sometimes it is convenient to offset the cycle of a looped animation. It is often used with the **Mirror** option for steering animations (clips used to turn the character while moving). We will be showing that in the *Using root motion to steer a character* recipe in `Chapter 4, Character Movement`.
+   - **Mirroring animations**: This option works only with **Humanoid** rigs. It is used to flip the animation left to right and can save up to 50 percent of steering animations when combined with the **Offset Cycle** option.

# There's more...

You can also mirror and offset animation states in the **Animator Controller**. If you select an animation state and go to the **Inspector** tab, you can find the **Mirror** and **Cycle Offset** options. There is also an option to use **Animator Controller** parameters to switch the **Mirror** option on and off and set the **Cycle Offset**. You need to have a Boolean parameter defined for the **Mirror** option and a float parameter for the **Cycle Offset**. Those settings will be automatically synchronized with the parameters. Whenever you change a parameter value, the setting will also be changed.