# Getting ready

We are going to use the same character as in the *Creating a humanoid ragdoll with the ragdoll wizard* recipe. We will also use the `HandleRagdoll.cs` script from the same recipe to turn the ragdoll on. On top of that, we will need to have two additional meshes: the **Head** model and the **Neck** model. See the following screenshot for reference:

![](bbox=[0.26, 0.26, 0.73, 0.43])

Neck and head models

You can also open the example Unity project and go to the `Chapter 09 Physics and animations\Recipe 07 Dismemberment` directory. If you open the `Example.unity` scene there, play the game and press the space bar so that the poor character will be decapitated. You can find all the additional models in the `Models` directory.

# How to do it...

To create a decapitation effect, follow these steps:

+   1. Follow the *Creating a humanoid ragdoll with the ragdoll wizard* recipe to have a character with a working ragdoll and the `HandleRagdoll.cs` script (a script to turn the ragdoll on).
+   2. Import the **Neck** and **Head** models and place them in the scene near the character.
+   3. Create two empty game objects; name the first one **HeadMarker** and the second one **NeckMarker**.
+   4. Parent the **HeadMarker** to the head bone and the **NeckMarker** to the neck bone. Make sure that the local position of **HeadMarker** and **NeckMarker** is 0.
+   5. Parent the **Head** object to the **HeadMarker** and the **Neck** object to the **NeckMarker**. Zero out **Head**'s and **Neck**'s local rotation and position.