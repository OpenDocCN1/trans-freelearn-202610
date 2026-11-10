You can download the example project and go to the `Chapter 10 Miscellaneous\Recipe 03 Using the Rotate function to animate an object` directory. Open the `Example.unity` scene there and play the game. You will see the windmill rotating around the *Y* axis and its wings rotating around their local *Z* axis.

# How to do it...

To use the `Rotate()` function for animating objects, follow these steps:

+   1. Import our windmill to Unity. The model should contain three objects: **WindmillBase**, **Windmill** (parented to **WindmillBase**), and **WindmillWings** (parented to **Windmill**).
+   2. Place the model in the scene.
+   3. Create a new C# script and name it `ScriptRotation.cs`. In this script's `Update()` function, we use only one method: `transform.Rotate()`. We have a series of `public` variables to be able to control the rotation speed and axis. The `Rotate()` method can rotate an object in the `World` or `Self` (local) space. We have a `public Space rotationSpace` variable to be able to control it from the **Inspector**:
+   4. Assign the script to the **Windmill** game object.
+   5. Set the **Rotation Space** to **World**, and the **Rotation Axis** to (0, 1, 0). This means our object will rotate in the global *Y* axis (the up vector). Set the **Rotation Speed** to your liking (it's in degrees per second; a value of 45-90 should be OK).
+   6. Assign the same script to the **WindmillWings** game object.
+   7. Set the **Rotation Space** to **Self** and the **Rotation Axis** to (0,0,1). This means the object will rotate around its local *Z* axis (the forward vector of the object). Again, set the **Rotation Speed** to your liking.
+   8. Play the game and observe the effect. The **Windmill** game object rotates in the global *Y* axis and the **WindmillWings** game object rotates around its local *Z* axis and follows the **Windmill's** *Y* axis rotation (because of the hierarchy setup).

```
transform.Rotate(rotateAxis * rotationSpeed *
    Time.deltaTime, rotationSpace);
```