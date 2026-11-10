
## How it works...

Animation Events call public functions from the scripts assigned to the game object playing the animation. To make it work, the **Function** field of the Animation Event needs to have the same name as the function we want to call from the script.

Events are extremely useful because they allow us to synchronize function calls with animations. The best moment to check if player hits the target is when we see a visual hit in the animation—a fist is in its extreme position in a punch, or a sword is in its extreme position in a swing. Without Animation Events, we would have to always manually delay function calls to achieve similar results.

## There's more...

Animation Events can also have an `int`, `float`, `string`, or an object type parameter. To use these parameters, we have to implement them in our functions called by the event. These parameters can be useful for playing special effects or sounds (you can find more information about it in the *Using Animation Events to trigger sound and visual effects* recipe in `Chapter 7`, *Special Effects*).

## Using transitions from Any State to play hit reactions

Often, there are situations when we would like to transition to an animation from any other one. The most common example is the hit reaction. Our characters can be hit while standing, walking, or even attacking. Creating and managing all those transitions would be cumbersome and difficult. In Unity, we can use a special **Any State** in the controllers to achieve transitions from any given animation.

## Getting ready

We are going to use the characters from the previous recipe and add one animation to the **Spider** character: **HitReaction**. You can also use the example project and go to the `Chapter 06 Handling combat\Recipe 03 Using transitions from any state to play hit reactions` directory. You can find the `Example.unity` scene there, with two characters: **Humanoid** and **Spider**. When you play the game, **Spider** will react to hits. In the `Rigs` directory of the previous recipe, you can find the required animations.

## How to do it...

To use Any State transitions, follow these steps:

- 1. Follow the previous recipe to import the characters and place them on the scene.
- 2. Create a new **Animator Controller** for the **Spider** (you can also edit the existing one).
- 3. Create a new `bool` parameter in the controller and call it **Dead**.
- 4. Create a new `Trigger` parameter in the controller and call it **Hit**.
- 5. Drag and drop the **Idle**, **Death**, and **HitReaction** animations of the **Spider** into the controller. The **Idle** animation should be our default state.
- 6. Find the blue **Any State** node in the controller. See the following screenshot for reference:
- 7. Right-click on it and create the following transitions:
  - **Any State** | **HitReaction** with one condition: **Hit** `Trigger` parameter. **Has Exit Time** should be set to `false`, **Can Transition To Self** set to true, and **Transition Duration** set to 0.2 seconds.
  - **Any State** | **Death** with one condition: **Dead** `bool` parameter set to `true`. **Has Exit Time** should be set to `false`, **Can Transition To Self** set to false, and **Transition Duration** set to 0.2 seconds.
  - **HitReaction** | **Idle**: For this one, you need to right-click on the **HitReaction** state first. It should have no conditions, **Has Exit Time** set to `true`, and **Transition Duration** set to 0.2 seconds.
- 8. Assign the controller to the **Spider's Animator** component.
- 9. Create a new script and call it `EnemyWithHits.cs`.

![](img/616a75b593bef4212f618360fceea442_192_0.png)

Transitions from Any State

- 10. Derive this script from the `Enemy.cs` script instead of `MonoBehaviour`. You can find the `Enemy.cs` script in the previous recipe.
- 11. To make our hit reactions work, we need to override the `Hit()` and `Die()` functions. In the first one, we check if our character's `isAlive` flag. If our **Spider** is dead already, we don't attempt to play hit reactions. If it is alive, we play the hit reaction by setting the **Hit Trigger** parameter in the controller. Next we call the base implementation of the `Hit()` function:
```
public override void Hit(int damage)
{
    if (!isAlive)
    {
        return;
    }
    anim.SetTrigger("Hit");
    base.Hit(damage);
}
```
- 12. In the `Die()` function, we set the `isAlive` flag to false and call the base implementation of the `Die()` function:
```
public override void Die()
{
    isAlive = false;
    base.Die();
}
```
- 13. Assign the script to the **Spider** character and play the game to see the effect.

## How it works...

Any State is a powerful tool for organizing animation graphs. If we know we need to access an animation state from any other state, we can create a transition from Any State node, instead of creating a web of transitions from all the necessary states.

This special state's transitions have one additional parameter: **Can Transition to Self**. If set to `false` it blocks transitions to the same state.

Any State transitions are most useful for hit reactions and death animations.

## Using root motion to create a dodge move

We can achieve a lot of gameplay actions in combat using just animations. A great example of such an action is the dodge move.

## Getting ready

To follow this recipe, we need a character with SpiderIdle and Dodge animations (we will be using our Spider character in this example). The dodge animation should be created with root motion. Please check the example in the following screenshot:

![](img/616a75b593bef4212f618360fceea442_194_0.png)

Spider dodge with root motion translation

You can also go to the `Chapter 06 Handling combat\Recipe 04 Using root motion to create a dodge move` directory. Open the `Example.unity` scene there. You will find the same **Humanoid** and **Spider** characters we had in previous recipes. They have the same animations and scripts as in the *Using transitions from Any State to play hit reactions* recipe. The **Spider** has one additional animation: **Dodge**. If you hit the space bar in **Play Mode**, the **Spider** will attempt to dodge the sword swing. You can find all the necessary animations in the `Chapter 06 Handling combat\Recipe 02 Using animation events to trigger script functions\Rigs` directory.

## How to do it...

To make a character dodge, follow these steps:

- 1. Import the character into Unity and place it in a scene.
- 2. If you want the character to get damage and play hit reactions, follow the previous recipe.
- 3. Create a new Animator Controller or edit the one from the previous recipe.
- 4. Drag and drop the **Dodge** animation into the controller.
- 5. Create a **Dodge** `Trigger` parameter in the controller.
- 6. Create two transitions:
  - **SpiderIdle** | **Dodge** with one condition: **Dodge** `Trigger` parameter. **Has Exit Time** should be set to false and **Transition Duration** set to 0.2 seconds.
  - **Dodge** | **SpiderIdle** with no conditions: **Has Exit Time** should be set to true and **Transition Duration** set to 0.2 seconds.
- 7. Add a **Rigidbody** component to the **Spider** character and freeze its rotations.
- 8. Add a collider to the **Spider** character (we are using **Sphere Collider** for our **Spider**). You may also need to create a zero friction **Physical Material** and add it to the collider. It allows the animation to control Rigid Body movement without any additional friction.
- 9. Set the **Animator** component's **Update Mode** to **Animate Physics** in our **Spider**.
- 10. Attach the controller to the **Animator** component of our **Spider**.
- 11. Create a new script and call it `Dodge.cs`. In this script's `Update()` function, we simply set the **Dodge** `Trigger` in the controller when player presses the space bar:
```
if (Input.GetKeyDown(KeyCode.Space))
```
