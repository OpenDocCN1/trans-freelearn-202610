# How it works...

In this recipe, we are using a `Quaternion.LookRotation()` method that creates a rotation, which works the same way as we would use the `Transform.LookAt()` function. It is applied to the head bone's transform. Our character's rig and all its bones are standard transforms in Unity. We can modify their rotation or position as we would with other game objects, but we need to do it in the `LateUpdate()` function because all the animations have to be evaluated first. We cannot modify any of the bones' transforms in the `Update()` function because all our modifications would be overwritten by the animations.

The `Quaternion.LookRotation()` function creates a rotation that makes a transform forward axis point to the desired direction. In most cases, the head bone's forward axis doesn't match the face of the character; thus, we need to apply an additional rotation. To do so, we use the `public Vector3 additionalRotation` variable.

# There's more...

For humanoid characters, we can use the IK approach. To do so, follow these steps:

+   1. Make sure to check the IK Pass option in the Animator Controller's layer properties.
+   2. Create a new script and call it `CharacterLookAtIK.cs`.
+   3. Create a void `OnAnimatorIK(int layerIndex)` function in that script. This function is called in the IK Pass after all animations are evaluated.
+   4. In that script, we use the `SetLookAtPosition()` and `SetLookAtWeight()` functions on the Animator component. We use a `public Transform target` variable to set the look at position and a `public float weight` variable to set the weight of the look at. We also use the `Vector3.SmoothDamp()` method to damp any sudden changes in the position of our target:
+   5. Save the script and assign it to the character.
+   6. Assign the **Target** game object to the **Target** field in the script's **Inspector**. Make sure to set the **Weight** field to 1.

```
targetPosition = Vector3.SmoothDamp(targetPosition,
target.position, ref dampVelocity, dampTime);
anim.SetLookAtPosition(targetPosition);
anim.SetLookAtWeight(weight);
```