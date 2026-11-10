+   9. Click on the **Apply** button in the **Sprite Editor**.
+   10. Create an empty game object in the scene's **Hierarchy** and give it a descriptive name (`Warrior` in our example).
+   11. Drag and drop all the body parts from your character sprite sheet onto **Warrior** game object (you have to drag and drop one body part at a time because dragging multiple sprites will create an Animation Clip).
+   12. Rename the body parts for better clarity. In our example, we have `Hips`, `Chest`, `Head`, `FrontUpperArm`, `FrontLowerArm`, `Front Hand`, `BackUpperArm`, `BackLowerArm`, `BackHand`, `FrontUpperLeg`, `FrontLowerLeg`, `FrontFoot`, `BackUpperLeg`, `BackLowerLeg`, and `BackFoot`.
+   13. Build a humanoid hierarchy from all the body parts in the scene's **Hierarchy**, with the **Warrior** game object as the root. In our example, the hierarchy looks like the following:
+   14. Adjust the **Order in Layer** field for every **Sprite Renderer** component in every body part. In our example, body parts have this order:

```
Warrior | Hips | Chest | Head
Chest | FrontUpperArm | FrontLowerArm | FrontHand
Chest | BackUpperArm | BackLowerArm | BackHand
Hips | FrontUpperLeg | FrontLowerLeg | FrontFoot
Hips | BackUpperLeg | BackLowerLeg | BackFoot
```

|  |  |
|---|---|
| 0 | BackUpperArm |
| 1 | BackLowerArm |
| 2 | BackHand |
| 3 | BackUpperLeg |
| 4 | BackLowerLeg |
| 5 | BackFoot |
| 6 | FrontUpperLeg |
| 7 | FrontLowerLeg |
| 8 | FrontFoot |
| 9 | Head |
| 10 | Chest |
| 11 | Hips |