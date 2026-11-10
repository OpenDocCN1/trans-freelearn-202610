# How to do it...

To use the `LookAt()` method for aiming, follow these steps:

+   1. Import your character to Unity and place it on the **Scene**.
+   2. Follow the previous recipe to make a character walk and aim.
+   3. Attach the `CharacterLookAt.cs` script to the character.
+   4. Create a new **Sphere** object and name it AimTarget.
+   5. Assign the chest bone to the `CharacterLookAt.cs` component's **Bone** field.
+   6. Assign the **AimTarget** to the **Target** field of the `CharacterLookAt.cs` component.
+   7. Make the **AimTarget** a child of the **Main Camera** game object. We are using the same `CameraLook.cs` script in this recipe. You can find the script in the `Shared Scripts` folder in the example Unity project provided.
+   8. Offset the **AimTarget** position in its local Z axis around 20 units.
+   9. Play the game to see the effect. Our character will try to aim at the **AimTarget**. You may need to adjust the **Additional Rotation** vector in the `CharacterLookAt.cs` component. In our example, it is set to 15 in X and 20 in Y.

# How it works...

We are using the same principle as in the *Using LookRotation() and SetLookAtPosition()* methods to make characters follow an object with their gaze recipe. Our script rotates the chest bone in such a way that the character appears to aim at the target. We use the **Additional Rotation** vector in the `CharacterLookAt.cs` script to adjust the rotation (chest is not facing straight forward in most animations). This way of aiming is good for games in which characters walk relative to the camera (it is suitable for TPP games with an over-the-shoulder camera). It fails when characters have to rotate more than 50-60 degrees. The next recipe solves this problem.