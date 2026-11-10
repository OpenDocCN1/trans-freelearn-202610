
## Using Quaternion.LookRotation() and Animator.SetLookAtPosition() methods to make characters follow an object with their gaze

Sometimes you need a character to look at an object in the game, for instance, at the camera. To do so, we can use two methods: `Quaternion.LookRotation()` and `Animator.SetLookAtPosition()`. We will cover both in this recipe (the second one is covered in the There's more... section).

### Getting ready

To follow this recipe, you need a character with one `Idle` animation. You can also go to the `Chapter 05 Character actions and expressions\Recipe 04 Using LookRotation and SetLookAtPosition methods to make characters follow an object with their gaze` directory. Open the `Example.scene` scene there. You will find the `HumanoidLookAt` and `HumanoidIKLookAt` game objects there. The first one uses a generic `LookAt()` method and the second one uses the `Animator.SetLookAtPosition()` function. To see the effect, play the game, switch to the Scene View, and move the `Target` game object around (a red shiny sphere).

![](img/616a75b593bef4212f618360fceea442_165_0.png)

Characters looking at the Target game object

### How to do it...

To make characters follow an object with their gaze, follow these steps:

- 1. Import the character into Unity and place it in a scene.
- 2. Make sure to create an Animator Controller with at least one animation, or use an existing one.
- 3. To use the first Quaternion.LookRotation() method, create a new script and call it CharacterLookAt.cs.
- 4. In that script, we use the void LateUpdate() function to alter bone rotation after all animations are evaluated. In that function, we first check if a public float weight variable is less than or equal to 0. If so, we don't do anything (we turn off the look at behavior):
- 5. If that is not true, we calculate our desired lookDirection. This is a vector in which our character should look. We calculate it by subtracting our public Transform bone position from the public Transform target position. This vector is then damped in time using the SmoothDamp() function. This prevents it from sudden changes. We use a public float dampTime variable to determine the time in which we smooth the vector out. The dampVelocity vector is class member variable required by the SmoothDamp() function to store the changes in the lookDirection vector between frames:
- 6. Next we check if the angle between our desired lookDirection vector and the character's transform.forward vector is greater than our public float maxAngle value. If so, we calculate the finalLookVector. This is our character's transform.forward vector, which is rotated toward the desired lookDirection vector by the maxAngle degrees. This way we create a cone of vision for our character and prevent it from breaking the neck joint. We need to use the Mathf.Deg2Rad constant to change our maxAngle degrees to radians because the Vector3.RotateTowards() function uses radians instead of degrees. If the angle between lookDirection and transform.forward is less than or equal to maxAngle, we don't alter the lookDirection:

```
if (weight <= 0f)
{
    return;
}
```

```
lookDirection = Vector3.SmoothDamp(lookDirection,
    target.position - bone.position, ref dampVelocity,
    dampTime);
```

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

- 7. Finally, we calculate the Quaternion rotation value by using the Quaternion.LookRotation() method and multiplying its result by a public Vector3 additionalRotation value. First we need to turn this Vector3 into a Quaternion by using the Quaternion.Euler() function. Multiplying two quaternions is simply adding an additional rotation. We need to use the additionalRotation vector because, in most cases, the head bone's forward axis doesn't match the face of our character. By applying an additional rotation of +90 or -90 degrees in one of the axes (X, Y, or Z), we can make the script work for every rig. After calculating the rotation, we linearly interpolate the current bone.rotation value to our calculated rotation using the weight value. This way we can turn the look at on and off easily:

```
rotation = Quaternion.LookRotation(finalLookVector) * Quaternion.Euler(additionalRotation);
bone.rotation = Quaternion.Lerp(bone.rotation, rotation, weight);
```

- 8. Save the script and add it to the character.
- 9. Drag and drop the head bone of the character to the Bone field in the script's Inspector.
- 10. Drag and drop the look at target transform to the Target field in the script's Inspector.
- 11. You may need to adjust the Additional Rotation field. Experiment in Play Mode with +90 or -90 values in different axes to find a matching value. Modifying one axis at a time should be enough.
- 12. Move the target transform in Play Mode to see the effect (you can do it in the Scene View).

### How it works...

In this recipe, we are using a `Quaternion.LookRotation()` method that creates a rotation, which works the same way as we would use the `Transform.LookAt()` function. It is applied to the head bone's transform. Our character's rig and all its bones are standard transforms in Unity. We can modify their rotation or position as we would with other game objects, but we need to do it in the `LateUpdate()` function because all the animations have to be evaluated first. We cannot modify any of the bones' transforms in the `Update()` function because all our modifications would be overwritten by the animations.

The `Quaternion.LookRotation()` function creates a rotation that makes a transform forward axis point to the desired direction. In most cases, the head bone's forward axis doesn't match the face of the character; thus, we need to apply an additional rotation. To do so, we use the `public Vector3 additionalRotation` variable.

### There's more...

For humanoid characters, we can use the IK approach. To do so, follow these steps:

- 1. Make sure to check the IK Pass option in the Animator Controller's layer properties.
- 2. Create a new script and call it `CharacterLookAtIK.cs`.
- 3. Create a void `OnAnimatorIK(int layerIndex)` function in that script. This function is called in the IK Pass after all animations are evaluated.
- 4. In that script, we use the `SetLookAtPosition()` and `SetLookAtWeight()` functions on the Animator component. We use a `public Transform target` variable to set the look at position and a `public float weight` variable to set the weight of the look at. We also use the `Vector3.SmoothDamp()` method to damp any sudden changes in the position of our target:
- 5. Save the script and assign it to the character.
- 6. Assign the **Target** game object to the **Target** field in the script's **Inspector**. Make sure to set the **Weight** field to 1.

```
targetPosition = Vector3.SmoothDamp(targetPosition,
target.position, ref dampVelocity, dampTime);
anim.SetLookAtPosition(targetPosition);
anim.SetLookAtWeight(weight);
```

- 7. Play the game and move the **Target** game object to see the result (you can do it in the **Scene View**).
- 8. To turn the **look at** on and off smoothly, interpolate the `weight` value in time using the `Mathf.Lerp()` function.

## Action Points – performing an action in a specified spot

Action Points are a common concept used for characters in games that have to perform a certain action in a certain spot. You will find a lot of them in RPG games where NPCs populate towns and perform different actions creating an illusion of a living community. We are going to address a simple case of an Action Point in this recipe.

### Getting ready

We are going to use a character with three animations: **Walk**, **Idle**, and **Action**. We are also going to use the `SetSpeedFromAgent.cs` script from the *Using Blend Trees to blend walk and run animations* recipe in *Chapter 5, Character Movement*. You can also open the provided example Unity project and go to the `Chapter 05 Character actions and expressions\Recipe 05 Action points performing an action in a specified spot` directory. You will find an `Example.scene` scene there. Play the game to see the effect—the Humanoid character will approach an Action Point and perform a pick up animation.

### How to do it...

To create a simple Action Point, follow these steps:

- 1. Import the character to Unity.
- 2. Put it on the scene and add a **NavMesh** Agent component to it.
- 3. Bake the **NavMesh** in the scene.
- 4. Create an Animator Controller and assign it to the character's Animator component.
- 5. Create a `float` **Speed** parameter and a `Trigger` **Action** parameter in the controller.
