![](bbox=[0.12, 0.14, 0.2, 0.22])

If you are using a **Humanoid** rig, it is enough to create only forward and left animations. Then you can create a new animation clip in the character **Import Settings** in the **Animation** tab (you need to click on the plus button in the **Clips** section). When you choose the **WalkLeft** animation as the source, set it to **Mirror** and set its **Cycle Offset** to 0.5; you should get a proper **WalkRight** animation.

# Using animations for better looking transitions

You've probably already noticed that sometimes when our character stops walking, it blends to the **Idle** animation in a strange way (with sliding feet). That is mostly visible when legs in the **Walk** animation are in a pose that looks like a mirrored Idle pose. We are going to fix this problem in this recipe.

# Getting ready

You should use the same character as in the previous recipe and prepare one additional animation that will be a transition from a mirrored Idle pose to a normal Idle pose. You can also go to the `Chapter 04 Character movement\Recipe 04 Using animations for better looking transitions` directory and open the `Example.unity` scene. You will find our Humanoid character working the same way as in the previous recipe, but with a **ToIdle** animation added. You can also find the rig with all required animations in the `Rigs` directory.

# How to do it...

To use additional animations for a better-looking transition, follow these steps:

+   1. First, follow all the points from the *Using root motion to steer a character* recipe to have a working character driven by root motion animations.
+   2. In the **Animation Import Settings** of the character, find your additional **ToIdle** animation. You may need to add it as a new clip by clicking on the plus button in the **Clips** section (**Animations** tab) and choosing the **ToIdle** animation as the source.