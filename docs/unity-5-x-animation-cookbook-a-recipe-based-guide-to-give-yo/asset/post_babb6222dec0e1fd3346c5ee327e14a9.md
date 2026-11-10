
9. Close the Animator Controller and assign it to the character's Animator component.

10. Create a new script and call it EdgeGrab.cs (you can find the finished script in the Scripts directory of this recipe). The goal of this script is to play appropriate animation when we touch the edge of the cliff. We also need to disable collisions and physics and make sure our character will be in the right place while playing the animation (in the RootTarget game object's position).

11. First we need a Grab() function to handle the situation when player starts touching the edge (we will call this function from our GrabTrigger game object later):

```
public void Grab (Transform target) {
    grabTarget = target;

    if (grabTarget != null)
    {
        anim.SetTrigger("GrabEdge");
        adjustPosition = true;
        steeringScript.enabled = false;
        jumpScript.enabled = false;
        rb.isKinematic = true;
        anim.applyRootMotion = true;
    }
}
```

12. This function has a target parameter which will hold the reference to the RootTarget game object. We assign this parameter to a class member variable grabTarget (we will use it later). We are starting to play the GrabEdge animation by setting the GrabEdge Trigger in the controller. We are also setting a flag (class member) adjustPosition to true. This flag is used later to determine whether we should match our character's position with the RootTarget game object's position. Then we disable two scripts stored in steeringScript and jumpScript variables. We are doing it only because we don't want them to interfere with our edge grab action. We are also setting the isKinematic option on our RigidBody component (the rb variable stores the reference to it) to disable collisions and gravity. Lastly, we enable root motion for our Animator Component (reference to which is stored in the anim variable). That makes the GrabEdgeClimb animation work.

13. Next we have to write the GrabLerp() function that handles the character's position and RootTarget's position matching:

```
void GrabLerp()
{
    if (grabTarget == null || !adjustPosition)
    {
        return;
    }
    if ((transform.position -
    grabTarget.
```

## 5 Character Actions and Expressions

This chapter explains the usage of animations for character actions and expressions and covers the following recipes:

- Creating an appear or a disappear animation
- Creating background characters and critters with animation-driven behavior
- Using **Blend Trees** to create randomized actions
- Using `Quaternion.LookRotation()` and `Animator.SetLookAtPosition()` methods to make characters follow an object with their gaze
- Action Points – performing an action in a specified spot
- Synchronizing an animation with objects in the scene
- Using IK for interacting with scene objects
- Animating facial expressions with Blend Shapes

## Introduction

In most games, characters perform a variety of actions and expressions. This chapter explains how to use animations to create believable behaviors and interactions.

## Creating an appear or a disappear animation

If our game has monsters in it, there is a huge probability that we will need to use an appear or a disappear animation to introduce such characters in the scene. Often times, monsters spawn near our players to ambush them. This is where the appear animations come in handy.

## Getting ready

Before we start, you should have a character with at least two animations: a looped **Idle** animation and an **Appear** animation. This animation should start below the ground (or high in the air if your character is a flying one). It should end with the **Idle** pose. See the following image for reference:

![](img/616a75b593bef4212f618360fceea442_157_0.png)

Appear animation key frames (the animation is done "in place")

You can download the provided example; open the project in Unity and go to the `Chapter 05 Character actions and expressions\Recipe 01 Creating an appear or disappear animation` folder. You will find a scene called `Example.Scene` there. When you play it, a Spider character will be spawned (instantiated) and it will play the **Appear** animation. In the `Rigs` directory, you can find the `Spider.fbx` asset with the required animations.

## How to do it...

To create an appear animation, follow these steps:

1. Import your animated character with **Idle** and **Appear** animations.
2. Make sure to check the **Idle** animation's **Loop Time** option.
3. Place the imported character in the scene.
4. Create a new Animator Controller and call it `Appear` (or you can edit an existing controller).
5. Drag and drop your **Appear** animation into the controller and make sure it is the default state. If not, right-click on it and choose the **Set As Layer Default State** option.
6. Drag and drop your **Idle** animation into the controller.
7. Create one transition:
   - **Appear** | **Idle** with no conditions: **Has Exit Time** should be set to true and **Transition Duration** set to 0.2 seconds.
8. Assign the Animator Controller to the character standing on the scene (to its Animator component).
9. To better see the **Appear** animation in action, create a prefab from the character. To do so, drag and drop it from the **Hierarchy** to any folder in the **Project** View.
10. Delete the character from the **Hierarchy** and write a script to spawn (instantiate) the character in runtime. In this recipe, we use the `Spawner.cs` script from the example Unity project's `Shared Scripts` directory.
11. This script spawns a prefab (a list of prefabs) in the same position and with the same rotation as the **Spawner** game object (the object the `Spawner.cs` script is attached to).

## How it works...

We use a very simple mechanism here—when Unity instantiates an animated game object, it always plays the game object's default animation state. So, when we set our **Appear** animation to be the default state, it is played when the character is spawned (instantiated). This way we can create interesting animations to make the characters appear near our players. In this example, our character crawls out from the ground.

## There's more...

To make the effect complete, you should also add a spawn VFX to the character. You can find an example of that: a particle system prefab called **RocksEffect** in the `Prefabs` directory in the provided Unity project. This is an “unearthing” effect. We spawn it with the same `Spawner.cs` script. Additionally, this prefab has an `AutoDestroy.cs` script that destroys the spawned effect after a few seconds.

## Creating background characters and critters with animation-driven behavior

In this recipe, we are going to create animated characters that will serve as decorations in the game. Such characters' behavior is driven only by animations; thus, we can have quite a large number of them in the game.

## Getting ready

Before we start, we need to have a character with a few animations. In this example, we are using a bird. It has **Idle**, **StartFlying**, **FlyingInCircles**, and **Land** animations. The **Idle** animation is looped and played when our birds sits on the ground. The **StartFlying** animation is a transition between **Idle** and **FlyingInCircles**, which is a looped animation of the bird flying around. The last animation, the **Land** animation, is a transition between **FlyingInCircles** and **Idle**. We don't use root motion for those animations because we want the birds to always land in the same position (we don't want to check where the ground is, it's just a decoration).

You can also download the provided example Unity project and go to the `Chapter 05 Character actions and expressions\Recipe 02 Creating background characters and critters with animation driven behavior` directory. You will find a scene called `Example.scene` there, with some birds sitting around. If you press the space bar, the birds will start flying. If you press it again, they will land. In the `Rigs` directory, you can find the `Bird.fbx` character with all required animations.
