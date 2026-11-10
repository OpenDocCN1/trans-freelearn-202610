
6. Drag and drop the **Idle**, **Walk**, and **Action** animations to the controller. Make sure that the **Idle** animation is the default state.

7. Create four transitions (see the following image):

- **Idle | Walk** with one condition: **Speed** parameter greater than 0.5. **Has Exit Time** should be set to false and **Transition Duration** set to around 0.1 seconds.
- **Walk | Idle** with one condition: **Speed** parameter less than 0.5. **Has Exit Time** should be set to false and **Transition Duration** set to around 0.1 seconds.
- **Any State | Action** with one condition: **Action** triggers parameter. **Has Exit Time** should be set to false and **Transition Duration** set to around 0.1 seconds.
- **Action | Idle** with no conditions: **Has Exit Time** should be set to true and **Transition Duration** set to around 0.1 seconds.

![](img/616a75b593bef4212f618360fceea442_170_0.png)

Animator Controller with Action state used by the Action Point

8. Create a new script and call it `ActionPoint.cs`.

9. In that script, we have an `IEnumerator PerformAction()` coroutine that handles the **Action Point** usage (it is started in the `Start()` function). In this coroutine, we first check if our character is close enough to the **Action Point**. If not, we set the **NavMesh Agent's** destination to the **Action Point's** position, wait one frame, and check again:

```
while ((agentTransform.position -
transform.position).sqrMagnitude > actionDistance *
actionDistance)
{
agent.SetDestination(transform.position);
agent.Resume();
yield return null;
}
```

10. The `agentTransform` variable holds the reference to the character's transform. The `actionDistance` is the distance from the Action Point in which our character should start performing the action.

11. When our character is closer to the Action Point than the `actionDistance`, we stop the NavMesh Agent and check if we want to match the character's position and/or rotation to our Action Point before the character starts playing the action animation. If not, we start playing the animation right away:

```
agent.Stop();
if (!matchBeforeAction)
{
    anim.SetTrigger(actionTrigger);
}
```

12. Next we check if we want to match character's position and/or rotation with Action Point's position/rotation. If so, we use linear interpolation to match the position and/or rotation. If the position and/or rotation of the character are close enough to the Action Point's position/rotation, we set the character's position/rotation to be exactly the same as Action Point's position/rotation:

```
while (matchPosition == true || matchRotation == true)
{
    yield return null;
    if (matchPosition && (agentTransform.position - transform.position).sqrMagnitude > 0.01f)
    {
        agentTransform.position = Vector3.Lerp(agentTransform.position, transform.position, Time.deltaTime * lerpSpeed);
    }
    else
    {
        matchPosition = false;
        agentTransform.position = transform.position;
    }
    if (matchRotation && Vector3.Angle(agentTransform.forward, transform.forward) > 1f)
    {
        agentTransform.rotation =
        Quaternion.Lerp(agentTransform.rotation,
        transform.rotation, Time.deltaTime *
        lerpSpeed);
    }
    else
    {
        agentTransform.rotation =
        transform.rotation;
        matchRotation = false;
    }
}
```

13. Lastly, we check if we want to play the animation after the character's position and/or rotation was matched with the Action Point. If so, we play the animation now (as the character position and/or rotation was adjusted):

```
if (matchBeforeAction)
{
    anim.SetTrigger(actionTrigger);
}
```

14. Save the script.
15. Create a new empty game object, call it **Action Point**, and place it in the scene, where the character should perform the action.
16. Attach the script to the Action Point game object. Drag and drop the character to the **Agent** field of the script's component.
17. You may need to adjust the **Action Distance** value of the script. You can also set the **Match Position**, **Match Rotation**, and **Match Before Action** options to achieve different results.
18. Play the game to see the effect.

## How it works...

The `ActionPoint.cs` script's role is to tell the character where to go and what animation to play. The `PerformAction()` coroutine also adjusts the character's position and rotation to match the position and rotation of the **Action Point**. We use the simple `Lerp()` function here for both the position and the rotation. In most cases, it's enough.

## There's more...

You may also use the `MatchTarget()` function to interpolate the character's position and rotation and match it with the **Action Point**. We were using this technique in the *Adding animation to off-mesh links* recipe in *Chapter 4, Character Movement*. In such cases, it would be best to have a special approach animation in which the character approaches the **Action Point**.

You may also want to use the randomized actions concept for creating **Action Points** with random actions. The concept is described in the *Using Blend Trees to create randomized actions* recipe. Instead of creating random actions, you can also prepare a `public enum Action` variable containing all your actions as enumerations and a public variable to store the Action in the Actionable Action `actionPointAction` variable to store the Action in the **Action Point**. Then you need to set the **RandomAction** parameter (as described in the mentioned recipe) to the `(float) actionPointAction` value. This will allow you to use **Blend Trees** for performing specified actions (instead of random ones).

Also, in many cases, you will need to divide your actions into three steps:

- **Pre-Action:** This animation can be used to approach the **Action Point** or as a "state transition" animation. For instance, when we want our character to sit on a chair it is good to have a **Pre-Action** animation of the character sitting (transition from **Idle** to **Sitting** states).
- **Looped-Action:** A looped animation suitable for this Action Point. In our sitting example, that would be a looped sitting animation. It can also be a series of animations (our character can, for instance, sit and eat or sit and drink).
- **Post-Action:** This is an animation that transitions from the **Looped-Action** animation to the **Idle** animation. In our example, it would be a character getting up from the chair.

## Synchronizing an animation with objects in the scene

In this recipe, we will synchronize a character's animation with an object placed in the scene. This is again a common mechanism used in games.

## Getting ready

To follow this recipe, you need two animated game objects: a character and an interactive object. In our example, we use a wheel lever that the character can rotate. We use only two animations, **WheelStart** and **WheelLoop**, for both the **Character** and the **Wheel** objects. The animations have the same number of frames and are synchronized in a 3D package already. You can also open the provided example Unity project and go to the `Chapter 05 Character actions and expressions\Recipe 06 Synchronizing an animation with objects in the scene` directory. You will find an `Example.scene` there. Play the game and press the space bar to see the **Character** and the **Wheel** play a synchronized animation. You will find all the necessary animations in the `Rigs` directory.

![](img/616a75b593bef4212f618360fceea442_174_0.png)

Character and Wheel objects playing synchronized animations

## How to do it...

To make the two objects play synchronized animations, follow these steps:

- 1. Import the **Character** and **Wheel** objects into Unity.
- 2. Set the **Loop Time** option in the **WheelLoop** animations for both the **Wheel** and the **Character**.
- 3. Place the **Character** and the **Wheel** in the scene and match their position according to what was set in the 3D package.
- 4. Create an **Animator Controller** for the Character.
- 5. Insert the **WheelStart** and **WheelLoop** animations into the controller. Make sure the **WheelStart** animation is the default state.
