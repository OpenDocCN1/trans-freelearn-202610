In the preceding code `agent` is a `public NavMeshAgent` variable that stores the reference to the Nav Mesh Agent component. We assign this reference manually in the **Inspector** by dragging the game object with the Nav Mesh Agent component to the **Agent** field of the script. The `hit` variable is a global `RaycastHit` variable that is used by the `Physics.Raycast()` method to store the ray cast result.

+   10. Save the script and attach it to the character. Drag and drop the character game object to the **Agent** field of the script (the script is attached to the same game object but can be attached to any game object).
+   11. Play the game and click on the ground to see the character move using both **NavMesh** Agent and root motion.

# How it works...

This recipe has a few key elements that make it work:

+   - **NavMesh and NavMesh Agent**: Nav Mesh Agent component is used to navigate in the game level with a baked **NavMesh**. Without one of those elements, our character wouldn't be able to move effectively with point and click input.
+   - **Disabling the agent's updateRotation and updatePosition**: By default, **NavMesh** Agents update the rotation and position of a game object. To use root motion, we need to disable this feature and update the game object's position and rotation with the root node animation instead.
+   - **Rigid Body and Animator with Apply Root Motion set to true**: We use Rigid Body to have collisions between all the objects in the game (not only other **NavMesh** Agents) and we use the **Apply Root Motion** option in the Animator to make the character move with root node animation.

# There's more...

In the `Scripts` directory of this recipe, you can also find a `NavAgent.cs` script that uses root motion without the **RigidBody** component. The `Update()` function of the script is very similar to the `NavMeshAgentWithRigidBody.cs` script that we were using in this recipe, but we enable updating the position by the Nav Mesh Agent component. We only disable rotation updating. We add a `OnAnimatorMove()` function to the script. This method is called every frame after Unity finishes evaluating all the states in the Animator.