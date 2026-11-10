
You do not have to always animate the properties of a game object. Using the transitions in the Animator Controller will create a smooth blend between them.

## Animating an object's world position – creating a moving platform

In this recipe, we will create a very common gameplay mechanism: a moving platform. We will use a Rigidbody for our character and an animated, kinematic Rigidbody for the platform.

### Getting ready

Before we start, you should have a scene with your character and a platform you want to animate. You can use the example project; go to the `Chapter 02 Working with the animation view\Recipe 03 Animating objects world position - creating a moving platform` directory. There is a scene called `Example.unity` there. If you open it, you will find a **Sheep** character in the **Hierarchy**. This is our character, using the **Rigidbody** component and a Simple Move script to move. There is also a **Moving Platform** game object in the **Hierarchy**. This is the kinematic rigid body with a `Platform` script attached to it. It also has an Animator component and an Animator Controller with just one animation in it. This animation makes the platform move.

### How to do it...

To create and assign an animated moving platform, follow these steps:

- 1. Select your platform game object in the **Hierarchy**.
- 2. Open the **Animation** View.
- 3. Make sure the record button is pressed (in the upper-left corner of the **Animation** View).
- 4. Move the platform to the desired position in the first frame.
- 5. Adjust the timeline a few seconds forward and move your platform to new destination.
- 6. To make the animation loop, select the first key frame and press Ctrl + C (cmd + C on Mac). Then adjust the timeline forward from the second key frame, the same amount of seconds like the previous step. If the first key frame is on the 0 seconds mark and the second on the 10 seconds mark, set the timeline to 20 seconds mark. Press Ctrl + V to paste the copied key frame. In our example, the platform is going from one floating island to another and back. It also has some pauses on each end to make it easier for the player to jump on to it.
- 7. When you are happy with your animation, exit the Animation View.
- 8. An Animator Controller will be created and an Animation Clip will be assigned to it automatically. Also, an Animator component will be added to the platform game object.
- 9. Find the Animator Controller on the platform game object and set the Update Mode property to Animate Physics. Set the Culling Mode to Always Animate.
- 10. Add a Rigidbody component to the platform and select the Kinematic checkbox.
- 11. Add a collider component (a Mesh Collider or a Box Collider) to your platform. If you are using a Mesh Collider, make sure to select the Convex checkbox.
- 12. Create a new script, and name the file Platform.cs.
- 13. Open the script and write the following code:

```
using UnityEngine;
using System.Collections;

public class Platform : MonoBehaviour {

/*This function is called by Unity every time this object
starts to collide with any other game object with a Collider
component attached.The Collision collisionInfo object
parameter stores the information about the collision and the
object we are colliding with.*/

void OnCollisionEnter(Collision collisionInfo)
{
/*We are checking if the object we are colliding with
has a RigidBody component and the RigidBody is not set
to kinematic. Optionally we can also check the tag of
the object we are colliding with here (to make it work
only for the player for instance).*/
if (collisionInfo.rigidbody != null
&& !collisionInfo.rigidbody.isKinematic)
{
/*We are setting the parent of the object we are
colliding with to the platform game object (the
object our script is attached to).This will make
our character move with the platform instead of
slide from it.*/
    collisionInfo.transform.parent = transform;

}
}

/*This function is called by Unity every time this object stops colliding with any object with a Collider component attached.
The Collision collisionInfo parameter stores the same information as in the OnCollisionEnter function.*/

void OnCollisionExit(Collision collisionInfo)
{

    /*We are checking the same conditions as before*/

        if (collisionInfo.rigidbody != null
        && !collisionInfo.rigidbody.isKinematic)
        {
        /*We are setting the parent of the object we are
colliding with to null. The object has no parent
        at all and stops moving with the platform*/
            collisionInfo.transform.parent = null;
        }
    }
}
```

- 14. Attach the script to the platform game object and play the game to see the effect.
- 15. This moving platform will work with characters using **Rigidbody** components to move. You can import the `ThirdPersonCharacter` prefab from Unity's `Standard Assets`. You can also write your own simple character movement. To do so, see the *There's more section.*

### How it works...

This is the most simple but working moving platform solution. It uses a few key elements:

- **Animation-driven movement**: The platform is moved only by the Animation Clip created with the Animation View. This allows you to experiment with the movement easily.
- **Kinematic Rigid body**: To animate a game object with a **Rigidbody** component attached, you need to set the **Kinematic** checkbox to true. It completely disables the physics of the Rigid Body. You can still animate the object with **Kinematic** set to false, but physics will still have an impact on the movement (the object will not be able to penetrate other objects, it will rotate on collisions, and so on).
- **Animate Physics option**: Set in the **Update Mode** parameter of the **Rigidbody** component. This option makes the Rigid body to be animated in the physics loop (you can think of it as the `FixedUpdate()` function equivalent). It prevents the Rigid Bodies colliding with this object to jitter and behave in strange ways.
- **Animation in world space**: Our platform is animated in world coordinates. This means that the animation sets the object's position regarding the scene's 0x, 0y, 0z point. It does not matter where the object is placed in the scene; after playing the animation, it will be placed in the positions stored in the animation's key frames. We will discuss local space animation in the *Animating object's local position – creating automatic doors* recipe.
- **Moving platform as a parent to the character**: We are setting the platform as the parent to the in-game character, which collides with it. Rigid bodies parented to other **Transforms** try to move with them in game. This is the easiest and rather bulletproof way of making our character move with/relative to the platform game object. And because the platform moves with the **Update Mode** set to **Animate Physics**, no jittering will occur. Instead of parenting the character to the platform, you could also experiment with creating a physical material with appropriate friction, or write your own custom solution that would add the platform's speed to the character's speed.

### There's more...

In the provided example, we created our own script for moving the character using a **Rigidbody** component. You can find the script in the `Shared scripts` directory. It is called `SimpleMove.cs`. To make your character move:

- 1. Add a collider component (we use a **Sphere Collider** in the example) and a **Rigidbody** component to it.
- 2. Create a zero friction `Physics Material` and assign it to the collider component.
- 3. Set the **Rigidbody** component **Constraints** to **Freeze Rotation** in the X, Y, and Z axes.
- 4. Create an empty game object, position it around 0.2 units above the character's feet, and parent it to the character. Name it `GroundCheck` for clarity.
- 5. Attach the script to the character's game object. Attach the `GroundCheck` game object to the `Ground Check Transform` field of the `Simple Move` script component.
- 6. If you want your character to have an animation, create an Animator Controller with `float Speed`, `bool Ground`, and `Trigger Jump` parameters.
  Prepare idle, walk, and jump animations. Idle and walk should be looped, jump should end in the air.
- 7. Create a transition between idle and run using the `Speed` parameter with the `Speed > 0.1` condition.
- 8. Create a transition between run and idle using the `Speed` parameter with the `Speed < 0.1` condition.
- 9. Create transitions from run and idle to jump using the `Jump` trigger parameter.
- 10. Create transitions from jump to run and idle using the `Ground` and `Speed` parameters. Conditions should check if `Ground` is true and if `Speed < 0.1` (transition to idle) or `Speed > 0.1` (transition to run).
- 11. Attach the Animator Controller to your character.
- 12. Parent the game camera to your character or use the provided `CameraFollow.cs` script found in the Shared scripts directory. You can also use one of the camera scripts found in Unity's **Standard Assets**.

## Animating object's local position – creating automatic doors

In this recipe, we will learn how to animate an object's local position to be able to use the same animated object in multiple locations in the scene. We will create another common gameplay mechanism, automatic door, as an example.
