**EdgeGrab** and **EdgeGrabLoop** animations should have all **Bake Into Pose** options selected in **Import Settings**. **EdgeGrabLoop** should also have the **Loop Time** option selected. You can also use the provided example Unity project and go to the `Chapter 04 Character movement\Recipe 08 Using triggers to grab an edge while jumping` directory. You will find an `Example.unity` scene there. Open it, play the game, and press the space bar to jump towards the edge. After the character grabs it, press the up arrow to climb over the edge.

# How to do it...

To be able to grab a cliff's edge while jumping, follow these steps:

+   1. Import your character to Unity. Make sure your character can move and jump (follow the *Making a character jump with 3-phase animation* recipe if needed). The character should use a **Rigidbody** component for moving and jumping. You can still use root motion to move and steer the character.
+   2. Place the character in your scene and make sure it has the `Player` tag set.
+   3. You need to have a cliff to be able to grab its edge. Import a suitable model or use a Cube from the **Game Object** | **3D Object** | **Cube** menu. The cliff game object should have a collider (**Mesh Collider** for example) because we use a Rigid Body character.
+   4. Add a trigger game object (an empty game object with a **Box Collider** component set to **Is Trigger**). Name it `GrabTrigger` for clarity. Place the trigger near the edge of the cliff. The trigger is marked with the number **1**, as shown in the screenshot after step 5.
+   5. Add another empty game object and name it **RootTarget**. Place it in the position where your character's transform should be when it grabs the edge. It is marked with the number **2**, as shown in the following screenshot. You can adjust the exact position later.