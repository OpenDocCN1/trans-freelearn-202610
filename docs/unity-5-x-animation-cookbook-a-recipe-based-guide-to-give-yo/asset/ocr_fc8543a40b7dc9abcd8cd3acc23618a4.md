+   4. Add a **Draw** event in the frame where the character's hand reaches the sword in the sheath.
+   5. Create an **Animator Controller** with the **Idle** and **Draw** animations.
+   6. Create a **Draw** `Trigger` parameter in the controller.
+   7. Create two transitions:
+      - **Idle | Draw** with one condition:**Draw** `Trigger` parameter. **Has Exit Time** should be set to `false` and **Transition Duration** set to around 0.2 seconds.
+      - **Draw | Idle** with no conditions: **Has Exit Time** should be set to `true` and **Transition Duration** set to around 0.2 seconds.
+   8. Place the **Warrior** character in the scene.
+   9. Assign the controller to the **Warrior** game object's **Animator** component.
+   10. Place the **Sword** prop in the scene. Move it near the character's hips and pose it as it needs to be when it lays in the sheath.
+   11. When you are happy with the **Sword's** position, create an empty child object in it. Name it **SheathMarker** and drag it onto the hip bone of the character to parent it to **Warrior's** hips. This way the **SheathMarker** will store our **Sword's** initial position and rotation relative to our character's hips. You should also parent the Sword to **SheathMarker**. See the following screenshot for reference:

![](bbox=[0.12, 0.56, 0.88, 0.81])

Sheath Marker (red) stores sword's desired position and rotation