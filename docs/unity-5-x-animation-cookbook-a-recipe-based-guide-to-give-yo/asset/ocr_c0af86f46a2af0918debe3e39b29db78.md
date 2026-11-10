# Getting ready

We are going to use the same assets as in the previous recipe, but instead of using the `CharacterLookAt.cs` script, we will use additional animations: **AimForward**, **AimForwardUp** (aim 45 degrees up), **AimForwardDown** (45 degrees down), **AimLeft** (45 degrees left), **AimLeftUp** (45 degrees left and 45 degrees up), **AimLeftDown** (45 degrees left and 45 degrees down), **AimRight** (45 degrees right), **AimRightDown** (45 degrees right and 45 degrees down), **AimRightUp** (45 degrees right and 45 degrees up). See the following screenshot for reference:

![](bbox=[0.1, 0.3, 0.9, 0.44])

Nine directional aim animations

You can also use the provided example Unity project and go to the `Chapter 06 Handling combat\Recipe 09 Using blend trees to aim` directory. You will find an `Example.unity` scene there, with a **Humanoid** character. Play the game to see the effect. You can find all the necessary animations in the `Rigs` directory.

# How to do it...

To use the **Blend Trees** methods for aiming, follow these steps:

+   1. Import your character with all the necessary animations to Unity and place it on the **Scene**.
+   2. Follow the *Using Avatar Masks and animator controller layers to walk and aim* recipe to make the character walk and aim.
+   3. Open the controller and go to the **Aim Layer**.
+   4. Delete the **AimForward** state and create a new **Blend Tree**.
+   5. Create two float parameters: **AimVer** and **AimHor**.