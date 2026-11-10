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