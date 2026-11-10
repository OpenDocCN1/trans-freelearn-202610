You can also open the example Unity project and go to the `Chapter 09 Physics and animations\Recipe 08 Getting up from a ragdoll` directory. If you open the `Example.unity` scene there, play the game and press the space bar; the character will fall down using ragdoll. If you press the space bar again, it will blend from a ragdoll to a getting up animation and will be animated again.

# How to do it...

To make a character get up from being a ragdoll, follow these steps:

+   1. Place your character in the scene.
+   2. Make two copies of it and name the characters **CharacterAnimated**, **CharacterSkinned**, and **CharacterRagdoll**. We will use these names for better clarity.
+   3. Make all characters stand in the exact same place in the scene.
+   4. Select the **CharacterRagdoll** and follow the *Creating a humanoid ragdoll with the ragdoll wizard* recipe to make a character a working ragdoll. We are not going to use the `HandleRagdoll.cs` script this time, so you can stop after using the Ragdoll Wizard.
+   5. Select **CharacterRagdoll** game object's rig and name it `Ragdoll`.
+   6. Select the **CharacterAnimated** and expand its hierarchy.
+   7. Remove all objects but the rig of the character. You cannot change the rig's name.
+   8. Select the **CharacterSkinned** and expand its hierarchy.
+   9. Rename the **CharacterSkinned** rig to `SkinnedRig`.
+   10. Select the **SkinnedRig** and the skinned mesh (or meshes if you have more). Drag and drop them onto **CharacterAnimated**. A window warning about prefab connection loss may appear. Click on **Continue**. This will parent the **SkinnedRig** and the mesh skinned to it to the **CharacterAnimated** game object.
+   11. Select **CharacterRagdoll**, grab the **Ragdoll** rig, and drop it onto the **CharacterAnimated** game object to parent it. Again click on **Continue** if the warning appears.
+   12. You can delete the **CharacterRagdoll** and **CharacterSkinned** game objects.
+   13. Create an Animator Controller or open an existing one.
+   14. In this controller, create two Triggers: **StandUpFaceUp** and **StandUpFaceDown**.