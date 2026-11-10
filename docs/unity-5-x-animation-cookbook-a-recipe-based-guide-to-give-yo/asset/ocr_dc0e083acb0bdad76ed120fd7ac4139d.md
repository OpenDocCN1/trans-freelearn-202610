15. Create four transitions:

+   - **Any State | StandUpFaceDown** with one condition: **StandUpFaceDown** Trigger. **Has Exit Time** should be set to false and **Transition Duration** set to 0.
+   - **Any State | StandUpFaceUp** with one condition: **StandUpFaceUp** Trigger. **Has Exit Time** should be set to false and **Transition Duration** set to 0.
+   - **StandUpFaceDown | Idle** with no conditions: **Has Exit Time** should be set to true and **Transition Duration** set to around 0.2.
+   - **StandUpFaceUp | Idle** with no conditions: **Has Exit Time** should be set to true and **Transition Duration** set to around 0.2.

16. Assign the controller to the character's Animator component.

17. [Optional] You can find the `ShowRig.cs` script in the `Shared Scripts` folder. If you attach it three times to the **CharacterAnimated** game object, you may drag and drop its three rigs, one to each `ShowRig` script. You may also assign different colors for each rig. If you play the game, you will be able to see the three rigs working. In the following screenshot, the red rig is the ragdoll, the yellow one is the one that is animated (we cannot change its name, in our example it's called `metarig`), and the green one is the one our mesh is skinned to (it's not clearly visible because it's hidden inside the mesh).

![](bbox=[0.18, 0.58, 0.78, 0.8])

Ragdoll rig (red), animated rig (yellow), and skinned rig (green)