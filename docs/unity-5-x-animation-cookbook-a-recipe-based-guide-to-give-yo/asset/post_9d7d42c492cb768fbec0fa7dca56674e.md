
6. In the Update() function of the script, we first disable updating rotation and position in the NavMesh Agent. That will prevent the agent from moving our character's transform. Then we calculate the float direction variable's value. We do it in a very similar way to what we did in the Using root motion to steer a character recipe, but instead of creating our own desiredMoveDirection vector, we are using the NavMesh Agent's desiredVelocity vector. This vector describes the speed and direction the agent would like to move with:

```
agent.updatePosition = false;
agent.updateRotation = false;

direction = Vector3.Angle(transform.forward,
agent.desiredVelocity) *
Mathf.Sign(Vector3.Dot(agent.desiredVelocity,
transform.right));
```

In the preceding code agent is the variable in which we store the reference to the Nav Mesh Agent component. We set this reference in the Start() function.

7. Next we calculate the float speed variable value: it's simply the magnitude of the agent.desiredVelocity vector. We also set the Direction, Speed, DirectionRaw, and SpeedRaw parameters in our Animator Controller using the anim variable that stores the reference to the Animator component. The anim variable is set in the Start() function. Lastly, we set the agent.nextPosition to be the same as our transform position every frame. This prevents the agent from moving away from our character's transform.

8. Save the script and attach it to the character.

9. We also need a script to tell the NavMesh Agent where we want to go. There is a ClickToMove.cs script in the Shared Scripts folder in the provided Unity example. In the Update() function, we use the agent.SetDestination() method when the player presses the left mouse button. A ray is cast from the mouse cursor position in the main camera's forward direction. If the ray hits a collider, the hit position is used to set the new destination for the NavMesh Agent:

```
if (Input.GetKeyDown(KeyCode.Mouse0))
{
    if (Physics.Raycast(Camera.main.ScreenPointToRay(
    Input.mousePosition), out hit))
    {
        agent.SetDestination(hit.point);
    }
}
```

In the preceding code `agent` is a `public NavMeshAgent` variable that stores the reference to the Nav Mesh Agent component. We assign this reference manually in the **Inspector** by dragging the game object with the Nav Mesh Agent component to the **Agent** field of the script. The `hit` variable is a global `RaycastHit` variable that is used by the `Physics.Raycast()` method to store the ray cast result.

- 10. Save the script and attach it to the character. Drag and drop the character game object to the **Agent** field of the script (the script is attached to the same game object but can be attached to any game object).
- 11. Play the game and click on the ground to see the character move using both **NavMesh** Agent and root motion.

## How it works...

This recipe has a few key elements that make it work:

- **NavMesh and NavMesh Agent**: Nav Mesh Agent component is used to navigate in the game level with a baked **NavMesh**. Without one of those elements, our character wouldn't be able to move effectively with point and click input.
- **Disabling the agent's updateRotation and updatePosition**: By default, **NavMesh** Agents update the rotation and position of a game object. To use root motion, we need to disable this feature and update the game object's position and rotation with the root node animation instead.
- **Rigid Body and Animator with Apply Root Motion set to true**: We use Rigid Body to have collisions between all the objects in the game (not only other **NavMesh** Agents) and we use the **Apply Root Motion** option in the Animator to make the character move with root node animation.

## There's more...

In the `Scripts` directory of this recipe, you can also find a `NavAgent.cs` script that uses root motion without the **RigidBody** component. The `Update()` function of the script is very similar to the `NavMeshAgentWithRigidBody.cs` script that we were using in this recipe, but we enable updating the position by the Nav Mesh Agent component. We only disable rotation updating. We add a `OnAnimatorMove()` function to the script. This method is called every frame after Unity finishes evaluating all the states in the Animator.

In that function, we set the agent.velocity vector to be the same as the velocity of the root node. We also update the transform rotation to be the same as the root node’s rotation:

```
void OnAnimatorMove()
{
    agent.velocity = anim.deltaPosition / Time.deltaTime;
    transform.rotation = anim.rootRotation;
}
```

## Using triggers to grab an edge while jumping

Grabbing an edge while jumping is a common feature, especially in platform games. It is easily done in Unity with a small amount of scripting. This recipe covers a simple edge grab functionality.

## Getting ready

You need three new animations for our character: EdgeGrab, EdgeGrabLoop, and EdgeGrabClimb. The first one is a transition from the InAir animation to the EdgeGrabLoop animation. It should have minimum movement in the root node if possible. The second one is a looped animation of hanging on the cliff's edge. The last one is an animation that uses root motion to climb the cliff's edge and ends with an Idle pose. See the following screenshot for reference:

![](img/616a75b593bef4212f618360fceea442_151_0.png)

Frames of the EdgeGrabClimb animation using root motion to climb over the cliff's edge

**EdgeGrab** and **EdgeGrabLoop** animations should have all **Bake Into Pose** options selected in **Import Settings**. **EdgeGrabLoop** should also have the **Loop Time** option selected. You can also use the provided example Unity project and go to the `Chapter 04 Character movement\Recipe 08 Using triggers to grab an edge while jumping` directory. You will find an `Example.unity` scene there. Open it, play the game, and press the space bar to jump towards the edge. After the character grabs it, press the up arrow to climb over the edge.

## How to do it...

To be able to grab a cliff's edge while jumping, follow these steps:

- 1. Import your character to Unity. Make sure your character can move and jump (follow the *Making a character jump with 3-phase animation* recipe if needed). The character should use a **Rigidbody** component for moving and jumping. You can still use root motion to move and steer the character.
- 2. Place the character in your scene and make sure it has the `Player` tag set.
- 3. You need to have a cliff to be able to grab its edge. Import a suitable model or use a Cube from the **Game Object** | **3D Object** | **Cube** menu. The cliff game object should have a collider (**Mesh Collider** for example) because we use a Rigid Body character.
- 4. Add a trigger game object (an empty game object with a **Box Collider** component set to **Is Trigger**). Name it `GrabTrigger` for clarity. Place the trigger near the edge of the cliff. The trigger is marked with the number **1**, as shown in the screenshot after step 5.
- 5. Add another empty game object and name it **RootTarget**. Place it in the position where your character's transform should be when it grabs the edge. It is marked with the number **2**, as shown in the following screenshot. You can adjust the exact position later.

![](img/616a75b593bef4212f618360fceea442_153_0.png)

GrabTrigger and RootTarget game objects' placement

- 6. Open the character's Animator Controller. Drag and drop your **GrabEdge**, **GrabEdgeLoop**, and **GrabEdgeClimb** animations into the controller.
- 7. Add a `Trigger` **GrabEdge** parameter and a `Trigger` **PullUp** parameter to the controller.
- 8. Create these transitions:
  - **Jump | GrabEdge** with one condition: **GrabEdge** trigger parameter. **Has Exit Time** should be set to false and **Transition Duration** set to around 0.2 seconds.
  - **InAir | GrabEdge** with one condition: **GrabEdge** trigger parameter. **Has Exit Time** should be set to false and **Transition Duration** set to around 0.2 seconds.
  - **GrabEdge | GrabEdgeLoop** with no conditions. **Has Exit Time** should be set to true and **Transition Duration** set to around 0.2 seconds.
  - **GrabEdgeLoop | GrabEdgeClimbing** with one condition: **PullUp** trigger parameter. **Has Exit Time** should be set to false and **Transition Duration** set to around 0.2 seconds.
  - **GrabEdgeClimbing | Idle** with no conditions: **Has Exit Time** should be set to true and **Transition Duration** set to around 0.2 seconds.
