# How to do it...

To use rigid body joints, follow these steps:

+   1. Import our swing doors to Unity. The model should contain three objects: **Frame**, **DoorLeft**, and **DoorRight**.
+   2. Place the objects in the scene and parent them all to an empty game object, which you may call **SwingDoors**.
+   3. Add a **Mesh Collider** to the **Frame** game object to prevent our character from going through it.
+   4. Add a **Box Collider** or a **Mesh Collider** set to **Convex** to the **DoorLeft** game object.
+   5. Add a **Rigidbody** component to the **DoorLeft** game object.
+   6. Add a **Hinge Joint** component to **DoorLeft**.
+   7. Set the **Axis** parameter to 0 X, 1 Y, and 0 Z. This is the axis in which our **Hinge Joint** can rotate.
+   8. Check the **Use Limits** option and set the **Min** to -120 and **Max** to 120 (exact limits may differ in your particular case). Those values describe the range in which our **Hinge Joint** can rotate from its neutral position.
+   9. If you want the doors to come back to their original position after they are pushed, check the **Use Spring** option.
+   10. Set the **Spring** parameter to 1 and **Damper** to 0.5 (you may need to experiment with the exact values).
+   11. Repeat steps 4-10 for the **DoorRight** game object.
+   12. Play the game and make any other rigid body collide with the doors to see the effect (you may use your character if it uses rigid body physics to move).

To create a rope using **Hinge Joints**, follow these steps:

+   1. Import our skinned **Rope** model to Unity. Rope should have enough bones to simulate its behavior (around 10-20 should be enough in most cases). Make sure the bones have identical or similar size and are not parented to each other (it will make the editing much easier).
+   2. Place the **Rope** model in the scene.
+   3. Go to the **RopeRig** game object and select all the bones.
+   4. Add **Capsule Collider**, **Rigidbody**, and **Hinge Joint** components to all the bones in the rig.
+   5. With all the bones selected, use the Inspector to scale the **Capsule Colliders** to fit the bones sizes.