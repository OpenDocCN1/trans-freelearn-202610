# Creating a generic ragdoll with character joints

This time we will create a ragdoll for a non-humanoid character. We cannot use the Ragdoll Wizard in this case, but instead we will make the ragdoll manually.

![](bbox=[0.22, 0.25, 0.79, 0.42])

A generic character ragdoll in action

# Getting ready

For this recipe, we need a nonhumanoid character. We use our old fellow Spider as an example. The character needs to be rigged. You can also open the example Unity project and go to the Chapter 09 Physics and animations\Recipe 05 Creating a generic ragdoll with character joints directory. If you open the Example.unity scene there and play the game, you will see a falling spider with the ragdoll already turned on. You can find the Spider in the Rigs directory.

# How to do it...

To create a generic ragdoll, follow these steps:

+   1. Import the character to Unity and place it in the scene.
+   2. Select all the bones of the character’s rig that will be used in the ragdoll simulation. In our example, we skipped the fangs.
+   3. Add Capsule Collider, Rigidb ody, and Character Joint components to the selected bones.