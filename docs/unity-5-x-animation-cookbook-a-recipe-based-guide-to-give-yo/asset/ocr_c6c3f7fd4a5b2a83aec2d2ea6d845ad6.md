```
void OnTriggerExit (Collider other)
{
    /*Again, we check if the object was the player*/
    if (other.gameObject.CompareTag ("Player")) {
        /*If it's true, we set the bool parameter "Open"
        to false in our Animator Controller. That plays
        the close animation and closes the doors.*/
        anim.SetBool ("Open", false);
    }
}
```

+   35. Assign the script to your **Trigger** game object and make sure your character has a **Rigidbody** component and the `Player` tag.
+   36. Play the game and approach the door; it should open. It should close when you exit the trigger.

# How it works...

This recipe illustrates a very important feature of Unity's animation workflow, the possibility to animate objects in local space. If we animate child game objects, their position, rotation, and scale will be animated in relation to their parent game objects. This makes it possible to create animated game objects, save them as prefabs, and reuse them in our games.

There are a few key elements of this recipe:

+   - **Door as a child of Door Frame**: We have two objects: an animated, moving **Door** and a stationary **Door Frame**. The **Door** game object is the child of the **Door Frame** game object in the **Hierarchy**. If we animate it, an Animator component will be added to the **Door Frame** game object (the parent), and the **Door** (the child) will be animated relative to the **Door Frame** (the parent).
+   - **Bool parameter "Open"**: This parameter is used by the door Animator Controller to switch between Opened and Closed states. We set it in the script attached to the trigger game object. When player enters the trigger, it sets the `Open` parameter to true, which tells the **Door** Animator Controller it should play the `Door Opening` animation. When the player exits the trigger, it sets the `Open` parameter to false, so the **Door** Animator Controller plays the `Door Closing` animation.