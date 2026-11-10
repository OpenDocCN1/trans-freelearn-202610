
animation **Threshold** is set to 2, and your current **Speed** parameter value equals 1.5, both **Walk** and **Run** animations will be played with 50 percent weight, which will most likely result in your character jogging slowly.

When blending **Walk** and **Run** animations, make sure to have the contact poses in the same normalized time in both animations. For instance, if the **Walk** animation has contact poses in 0 percent, 50 percent, and 100 percent of the animation, the **Run** animation should also have contact poses in 0 percent, 50 percent, and 100 percent of the animation. That will assure proper and smooth blending between the animations.

## There's more...

There are a few other interesting options in the **Blend Tree** settings:

- You can set the **Blend Type** of a **Blend Tree** to the following:
  - **1D**: A simple Blend Tree using one parameter to define the weight of a currently played animation.
  - **2D Simple Directional**: This option uses two parameters for blending the animations, such as the *X* axis and *Y* axis. It is best used for motions representing different movement directions, for instance, walk forward, walk left, walk right, and walk back. It shouldn't be used with multiple animations representing movement in the same direction (such as walk forward and run forward).
  - **2D Freeform Directional**: This option is similar to the previous one, but you can use multiple animations representing movement in the same direction (such as walk and run). You need to add a single animation representing the motion in the 0, 0 position (such as idle).
  - **2D Freeform Cartesian**: This option is similar to the preceding one, but is used when your animations don't represent movement in different directions. It can be used to blend multiple versions of the idle animation, for instance.
  - **Direct**: You can control the weight of each of the nodes (Motion fields) directly. This type is often used while blending facial expressions (see the *Animating facial expressions with Blend Shapes* recipe in Chapter 5, *Character Actions and Expressions*).

- You can set a few additional **Motion** field options:
  - **Time Scale**: You can alter the playback speed of each animation in the **Blend Tree** by changing the number in the **Time Scale** field: the one with a clock icon. It is set to 1 (100 percent) by default.
  - **Mirror**: You can mirror any of the animations in the **Blend Tree** by checking the **Mirror** option, the one with a mirrored humanoid icon.
- You can also check the **Automate Thresholds** option, which will distribute the **Motion** fields' **Thresholds** evenly throughout the whole parameter's range. For instance, if your parameter's range is 0 to 9 and you have four animations, the first one will have a **Threshold** of 0, the second one 3, the third one 6, the fourth one 9. You can change the parameter range by clicking on the 0 and 1 numbers below the blending graph.
- You can also use the **Compute Thresholds** option (**Automate Thresholds** have to be set to **false**). This option will compute the **Thresholds** based on the root motion information from your animations (speed magnitude, velocity in the X axis, velocity in the Y axis, velocity in the Z axis, angular speed in radians, and angular speed in degrees). We will cover root motion in the next recipe.

## Using root motion to drive Rigid Body characters' movement with animations

This recipe describes a very important concept called root motion. It allows the extraction of translation and rotation data from an animation and applies it to our character in the game.

### Getting ready

To use root motion, first you need to create a character with a walk or run animation, that has translation in it; see the following screenshot:

![](img/616a75b593bef4212f618360fceea442_127_0.png)

Frames of a run animation using root motion. The character is animated with root node translation

If you are using a **Humanoid** character, the hip bone is used as the root node, the one describing root motion. So hip translation will describe the translation of the character in game (you can still have motion in the Y axis as we can then adjust it in the **Import** settings). Its rotation will describe character rotation in the game. If you are using a **Generic** character, you need to choose the **Root Node** manually in the **Import** settings. Select the model, go to **Inspector**, click on the **Rig** tab, set the **Animation Type** to **Generic**, and select the **Root Node** from its drop-down menu (it contains the whole bone hierarchy of your rig).

You can also download the provided example Unity project and go to `Chapter 04 Character movement\Recipe 02 Using root motion to drive rigid body characters movement with animations` directory. You will find a scene called `Example.unity` there, with a **Humanoid** game object in the **Hierarchy**.

![](img/616a75b593bef4212f618360fceea442_127_1.png)

If you play the game, you can move the character with `WSAD` or the arrow keys and run with `Shift`. The movement is described by root motion, but steering is done with scripts (we simply rotate the character). In the `Rigs` directory, you can find the imported `Humanoid.fbx` file with all the required animations.

### How to do it...

To move a character using animations, follow these steps:

1. Import your character with root motion animations into Unity. We are using three animations in this example: Idle, WalkRoot, and RunRoot.
2. Select the character asset file and go to the Inspector. Make sure the Animation Type is set properly for your character (Generic or Humanoid). If you are using a Generic character, make sure to set its Root Node.
3. Go to the Animation tab and select the Idle animation. Select all the Bake Into Pose options. This bakes all the root motion data into the animation and makes it completely stationary. We don't want the Idle animation to move or rotate our character. You may also set the Base Upon option of the Root Transform Rotation to Original. This will make your character stand in the same pose as authored in the 3D software.
4. For WalkRoot and RunRoot animations, select the same options as for the Idle animation, but uncheck Bake Into Pose for Root Transform Position (XZ). This will make our character move in the X and Z axes only. The Y movement of the root node and its rotation will be baked into the animation and will not be stored as root motion anymore.
5. Set the Loop Time options to true for all animations.
6. Apply the import settings.
7. Place your character onto a scene.
8. Select it in the Hierarchy and add a Capsule Collider to it. You may need to adjust the Capsule Collider properties to better fit your character. In our examples, we need to set the Height property to 2 units and the Y axis in the Center property to 1 unit.
9. Add a Rigidbody component to the character and set its Constraints to Freeze Rotation in every axis.
10. Navigate to the Animator component of the character (it is added automatically for animated game objects). Set Update Mode to Animate Physics. Make sure the Apply Root Motion option is checked in the Animator component.
11. Create an Animator Controller asset for the character.
12. Drag and drop the Idle animation into the Animator Controller to make it the default state.
13. Create a float DesiredSpeed parameter in the Animator Controller.
14. Right-click on the empty space in the Animator Controller and choose Create State | New From Blend Tree.
15. Click on the **Blend Tree** and change its name in the **Inspector** to **WalkAndRun**.
16. Double-click on the **Blend Tree** to open its settings.
17. Click on the **Plus** button and choose the **Add Motion Field** option twice.
18. Drag and drop your **WalkRoot** animation in the first (upper) field and your **RunRoot** animation in the second (lower) field.
19. Uncheck the **Automate Thresholds** option.
20. Set the **WalkRoot** animation **Threshold** to 1 and **RunRoot** animation **Threshold** to 2.
21. Double-click on the empty space in the Animator Controller to get out of the **Blend Tree** settings.
22. Create two transitions:
    - **Idle | WalkAndRun** with the condition set to **DesiredSpeed** parameter greater than 0.5, **Has Exit Time** set to false, and **Transition Duration** set to 0.2 seconds.
    - **WalkAndRun | Idle** with the condition set to **DesiredSpeed** parameter less than 0.5, **Has Exit Time** set to false, and **Transition Duration** set to 0.2 seconds.
23. Write a script to set the **DesiredSpeed** parameter and assign it to the character. Play the game to see the effect (make sure your scene has a floor with a collider to prevent your character from falling down).
24. You can also use the `MoveAndSteer.cs` script from the `Scripts` directory in this recipe. In the `Update()` function, we get the player input and save the values in `hor` and `ver` variables:

```
hor = Input.GetAxis("Horizontal");

ver = Input.GetAxis("Vertical");
```

25. Then we use the `Rotate()` method to rotate our character in the global Y axis (`Vector3.up`) based on the `hor` variable and a `public float rotationSpeed` variable (set to 90 degrees per second):

```
transform.Rotate(Vector3.up * hor * rotationSpeed *
Time.deltaTime);
```

26. We modify the `desiredSpeed` variable based on player input (we check if the player holds the *Shift* key):

```
if (Input.GetKey(KeyCode.LeftShift) ||
Input.GetKey(KeyCode.RightShift))
{
```
