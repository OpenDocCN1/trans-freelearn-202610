```
if (Vector3.Angle(lookDirection, transform.forward) > maxAngle)
{
    finalLookVector =
    Vector3.RotateTowards(transform.forward,
    lookDirection, Mathf.Deg2Rad*maxAngle, 0.5f);
}
else
{
    finalLookVector = lookDirection;
}
```

7. Finally, we calculate the Quaternion rotation value by using the Quaternion.LookRotation() method and multiplying its result by a public Vector3 additionalRotation value. First we need to turn this Vector3 into a Quaternion by using the Quaternion.Euler() function. Multiplying two quaternions is simply adding an additional rotation. We need to use the additionalRotation vector because, in most cases, the head bone's forward axis doesn't match the face of our character. By applying an additional rotation of +90 or -90 degrees in one of the axes (X, Y, or Z), we can make the script work for every rig. After calculating the rotation, we linearly interpolate the current bone.rotation value to our calculated rotation using the weight value. This way we can turn the look at on and off easily:

```
rotation = Quaternion.LookRotation(finalLookVector) * Quaternion.Euler(additionalRotation);
bone.rotation = Quaternion.Lerp(bone.rotation, rotation, weight);
```

+   8. Save the script and add it to the character.
+   9. Drag and drop the head bone of the character to the Bone field in the script's Inspector.
+   10. Drag and drop the look at target transform to the Target field in the script's Inspector.
+   11. You may need to adjust the Additional Rotation field. Experiment in Play Mode with +90 or -90 values in different axes to find a matching value. Modifying one axis at a time should be enough.
+   12. Move the target transform in Play Mode to see the effect (you can do it in the Scene View).