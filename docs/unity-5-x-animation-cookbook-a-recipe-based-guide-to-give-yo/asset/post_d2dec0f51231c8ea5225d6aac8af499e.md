
## Getting ready

To make an animated door, you should have two objects ready: the **Door Frame** and the **Door**. The **Door** should be a child of the **Frame** in the **Hierarchy**. It should be placed in the *closed position*. We assume that our **Door** will slide upward when opening. You should also have a player character with the `Player` tag assigned. The character should use a **Rigidbody** component to move or have a kinematic **Rigidbody** component. We will use triggers for our doors, and triggers react to Rigid bodies only. You can also go to the `Chapter 02 Working with the animation view\Recipe 04 Animating objects local position - creating automatic doors` directory and find the `Example.unity` scene there. If you open it, you can find **Automatic Door** game object in the **Hierarchy** and a **Sheep** game object that will work as our player (it has the `Player` tag). If you run the game and go near the door, it will open.

## How to do it...

To create automatic doors, follow these steps:

- 1. Select your **Door Frame** game object, and add an Animator component to it. To do so, click on the **Add Component** button in the **Inspector** and choose **Animator**. Make sure to add the component to the **Door Frame** instead of the **Door** (the parent instead of the child). Set the **Update Mode** of the **Animator** component to **Animate Physics**.
- 2. With the **Door Frame** game object selected, go to **Window | Animation** to open the **Animation** View.
- 3. Create a new Animation Clip and call it `Closed`; this will be our animation for the closed door. To create a new clip, click on the drop-down list below the playback buttons and choose the **Create New Clip** option.
- 4. Click on the **Add Property** button and find your **Door** object in the list (a child of the **Door Frame** game object).
- 5. Unfold the **Door** game object foldout and find the **Transform** section. Unfold it and click on the plus icon near the **Position** property, as shown in the following screenshot:

![](img/616a75b593bef4212f618360fceea442_71_0.png)

- 6. Two key frames will be added for the **Door** game object's local position. We are not going to adjust them (assuming that the **Door** is in the closed position).
- 7. Add another Animation Clip and name it Door Opening.
- 8. Click on the **Add Property** button, choose your door game object, and add the **Transform | Position** key the same way as in step 5.
- 9. Move the second key frame to around the 3 second mark.
- 10. Select the **Door** game object (the child of the **Door Frame**).
- 11. Move it up to the open position. Make sure the record button is active (in the upper-left corner of the **Animation** View). If you play the animation, it should move the **Door** game object up.
- 12. Select the last frame of the **Door** Opening animation (in the **Animation** View). Go to the **Inspector** and copy the Door's **Transform** component. To do so, click on the small gear icon in the upper right corner of the component and choose the **Copy Component** option.
- 13. Create a new Animation Clip and name it `Opened`. Add the key frames to the **Door** child object the same way as in step 5. If your **Door** object moved, select it, click on the gear icon near the **Transform** component in the **Inspector**, and choose **Paste Component Values**. That will paste our copied **Transform** properties from the last frame of the `Door Opening` animation.
- 14. Make sure both frames in the animation have the same position.
- 15. Create one more Animation Clip (the last one) and name it `Door Closing`.
- 16. Make the animation of the **Door** game object slide down from open to closed positions. Remember that you can copy the Door's **Transform** component from appropriate animation frames and paste them in the animation you are working on.
- 17. Preview all the animations with the **Animation** View. You can switch between animations using the drop-down list below the playback buttons.
- 18. If you are happy with your animations, close the **Animation** View. Select your **Door Frame** game object and find its Animator component in the **Inspector**.
- 19. Click on the **Controller** field, which will show you the automatically created Animator Controller in the **Project** View. Double-click to open it.
- 20. Create a `bool` parameter and call it `Open`.
- 21. Create a loop of transitions between the animation states: **Closed** | **Door Opening** | **Open** | **Door Closing** | **Closed**. See the following screenshot:

![](img/616a75b593bef4212f618360fceea442_72_0.png)

- 22. Select the **Closed** | **Door Opening** transition. Add a condition to it, choose the `Open` parameter, and set the condition to true. Disable the **Has Exit Time** option in the transition.
- 23. Select the **Opened | Door Closing** transition. Add a condition to it, choose the `Open` parameter, and set the condition to false. Disable the **Has Exit Time** option in the transition.
- 24. Leave the **Door Opening | Opened** and **Door Closing | Closed** transitions without a condition. Make sure the **Has Exit Time** option is enabled in both those transitions.
- 25. Close the Animator Controller.
- 26. Select the Door Frame game object in the scene's Hierarchy.
- 27. Add a **Cube** child object to the Door Frame game object. To do so, right-click on the **Door Frame** game object and choose **Create | 3D Object | Cube**. This will be our trigger.
- 28. Select the **Cube** and scale it so that it stands out on both sides of the Door Frame. See the following screenshot for reference:

![](img/616a75b593bef4212f618360fceea442_73_0.png)

- 29. Remove the **Mesh Renderer** and **Mesh Filter** components from the **Cube**. To do so, click on the small gear icon in the upper-right corner of the given component and choose the **Remove Component** option. That will leave only the **Transform** and **Box Collider** components on the Cube.
- 30. Enable the **Is Trigger** checkbox on the Cube's **Box Collider** component.
- 31. Rename the Cube game object to `Trigger`.
- 32. Select the **Door Frame** game object and add a **Mesh Collider** component to it.
- 33. Select the **Door** game object and add a **Box Collider** component (or a **Mesh Collider** set to **Convex**) and a **Rigidbody** component. Enable the **Is Kinematic** option in the **Rigidbody** component.
- 34. Create a new C# script and name it `AutomaticDoors.cs`. Open it and write the following code:

```csharp
using UnityEngine;
using System.Collections;

public class AutomaticDoor : MonoBehaviour
{
    /*The anim variable is used to store the reference
    to the Animator component*/

    private Animator anim;

    void Start ()
    {
        /*We assign the Animator component of the parent object
        because this script is attached to the trigger, which
        is the child object of our animated doors*/

        anim = transform.parent.GetComponent<Animator> ();
    }
    /* This function is called when a Rigidbody intersects with
    the collider attached to our game object for the first time.
    Our collider has to be set to trigger. The Collider other
    parameter stores information about the object which collided
    with our trigger (entered the trigger).*/
    void OnTriggerEnter (Collider other)
    {

        //We check the tag of the object entering the trigger
        if (other.gameObject.CompareTag ("Player")) {

            /*If the tag equals "Player", we set the
            bool parameter "Open" to true in our
            Animator Controller - that plays the open
            animation and opens the doors*/

            anim.SetBool ("Open", true);
        }
    }
    /* This function is called when a Rigidbody exits the trigger
    (stops colliding with our trigger collider).*/
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
}
```

- 35. Assign the script to your **Trigger** game object and make sure your character has a **Rigidbody** component and the `Player` tag.
- 36. Play the game and approach the door; it should open. It should close when you exit the trigger.

## How it works...

This recipe illustrates a very important feature of Unity's animation workflow, the possibility to animate objects in local space. If we animate child game objects, their position, rotation, and scale will be animated in relation to their parent game objects. This makes it possible to create animated game objects, save them as prefabs, and reuse them in our games.

There are a few key elements of this recipe:

- **Door as a child of Door Frame**: We have two objects: an animated, moving **Door** and a stationary **Door Frame**. The **Door** game object is the child of the **Door Frame** game object in the **Hierarchy**. If we animate it, an Animator component will be added to the **Door Frame** game object (the parent), and the **Door** (the child) will be animated relative to the **Door Frame** (the parent).
- **Bool parameter "Open"**: This parameter is used by the door Animator Controller to switch between Opened and Closed states. We set it in the script attached to the trigger game object. When player enters the trigger, it sets the `Open` parameter to true, which tells the **Door** Animator Controller it should play the `Door Opening` animation. When the player exits the trigger, it sets the `Open` parameter to false, so the **Door** Animator Controller plays the `Door Closing` animation.
