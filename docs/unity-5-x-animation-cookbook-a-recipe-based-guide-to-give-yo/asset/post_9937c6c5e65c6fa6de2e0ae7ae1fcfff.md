
- **Animation-driven movement**: The **Door** game object is animated to open (move up) and close (move down), depending on the **bool Open** parameter. The `Door` game object has a collider to prevent any Rigid body going through it. It also has a **RigidBody** component set to **Is Kinematic**. All moving colliders should be kinematic Rigid bodies for optimization reasons.
- **Trigger game object**: The **Door Frame** has also a trigger game object as a child. A trigger is a game object with a collider set to **Is Trigger**. It recognizes when an object with a **RigidBody** component enters or exits the collider and calls the `OnTriggerEnter` and `OnTriggerExit` functions, respectively. We set the value of the **bool Open** parameter in those functions.

## See also

The concept of animating game objects in local space will be used multiple times throughout the course of this book. We are going to create an interesting example in the next recipe.

## Using the Hierarchy to animate local rotation – creating an orbiting planet

In this recipe, we will explore local transform animations further. We will use the **Hierarchy** and create smart parent-child relationships between our game objects to make an interesting effect of orbiting planets. Learning how to animate an object's local position and rotation and use the **Hierarchy** to our advantage is key to getting the most of Unity's animation system.

### Getting ready

Before we start, you need to prepare at least two objects to animate. We are using planets in this example. You can also use the example Unity project provided and go to the `Chapter 02 Working with the animation view\Recipe 05 Using the hierarchy to animate local rotation - creating an orbiting planet` directory. You will find an `Example.unity` scene there. There is a **Planets** game object with an Animator component and an Animator Controller assigned. If you select the **Planets** game object and go to **Window | Animation**, you can edit the animation of orbiting planets.

### How to do it...

To use Hierarchy to animate an object's local rotation, follow these steps:

1. Put your planet models into the scene.
2. One of our planets will be the parent of our little “planetary system.” Call it **Planet1** (as we will refer to it in this recipe a lot).
3. Add an empty child game object to **Planet1** and name it **Planet2Orbit**. The **Planet2Orbit** game object should be placed in the center of the **Planet1** game object.
4. Parent your second planet game object (let's call it **Planet2**) to the **Planet2Orbit** game object.
5. Set the position of the **Planet2** game object to 0x, 0y, 0z to place it exactly in the same position as the **Planet2Orbit** (and **Planet1**) game object.
6. Move your **Planet2** object to the desired distance from **Planet1**. Use only one axis (X, Y, or Z). See the following screenshot :

![](img/616a75b593bef4212f618360fceea442_77_0.png)

7. Add an Animator component to the root game object of our planetary system (**Planet1**).
8. Open the **Animation** View with the **Planet1** game object selected (go to **Window** | **Animation**).
9. Create a new Animation Clip by clicking on the **Create** button.
10. Make sure the record button is pressed (upper-left corner of the **Animation** View).
11. Select the **Planet2Orbit** game object from the **Hierarchy**.
12. Rotate the **Planet2Orbit** a bit in one axis (choose *Y* for instance) to create a new key frame. After you rotate it, the **Rotation** property of that object will be added automatically to the **Animation** View. Find the **Rotation** section, in the **Transform** component in the **Inspector** of the **Planet2Orbit** game object, and enter 0 in the axis you chose (*Y* in this example).
13. Move the timeline in the **Animation** View a few seconds forward to create the second key frame.
14. Make sure the **Planet2Orbit** game object is still selected and the record button is pressed.
15. Type `360` in the rotation section in the `Transform` component of the **Planet2Orbit** game object. Remember to type it in the same axis you chose before (*Y* in this example). This will create the second key frame and the animation will be perfectly looped.
16. Play the game to see the effect of an orbiting planet.

### How it works...

The key element of this recipe is the **Hierarchy** of our game objects. All animations created with Unity's **Animation** View are made in local space, which means that if you animate a parent object's position or rotation, its child objects will also move or rotate, respectively.

Our Hierarchy looks like this: **Planet1** | **Planet2Orbit** | **Planet2**:

- **Planet1**: This is our root game object. We can move it in space and our animation will still work perfectly.
- **Planet2Orbit**: This game object is the child of the **Planet1** game object. It is placed in the center of **Planet1**. Only this object is animated in the whole **Hierarchy**. We use this empty game object because we may want to rotate the root planet (**Planet1**) and we need another object in the **Hierarchy** to desynchronize the rotation of **Planet1** with the orbiting movement of **Planet2**.
- **Planet2**: This game object is the child of **Planet2Orbit**. It is translated in the local space of **Planet2Orbit** in one axis only. The rotation of the **Planet2Orbit** game object makes **Planet2** move in circles, which creates the orbiting planet effect.

### There's more...

- You can add more planets to our planetary system. In the provided example, there is also a **Moon** game object orbiting **Planet2**.
- You can also have nested game objects with their own Animator components attached. That will make it possible to have multiple Animator Controllers driving animations of different parts of the **Hierarchy**.

## Animating triggers – creating a death trap

In this recipe, we will animate a trigger position and use it to create a death trap. You can use Unity's Animation View to create various game play mechanisms with minimum scripting.

### Getting ready

Before we start, you should prepare a death trap model. We are using a “press trap” as an example. It contains two identical moving elements. See the following screenshot:

![](img/616a75b593bef4212f618360fceea442_79_0.png)

The model is not skinned and has no rig; instead, the moving elements are separate objects. We will animate them with Unity's **Animation** View. You can also use the example Unity project provided and go to the `Chapter 02 Working with the animation view\Recipe 06 Animating triggers - creating a death trap` directory. You will find an `Example.unity` scene there. If you play the game, you can see the trap working. If you open the scene, you will find the **Trap** game object with the **LeftTrap** and **RightTrap** game objects as its children. Those child objects are the only ones animated in this example. They have trigger objects as their children. Those trigger objects kill the player when he enters them. All other child objects of the **Trap** game object are only decorations and are optional.

### How to do it...

To animate the trigger position and create a death trap, follow these steps:

1. Create an empty object in the scene and name it **Trap**. It will be a parent object for our moving parts.
2. Put your models into the scene and parent them to the **Trap** game object. The models should have descriptive names, for instance, **LeftTrap** and **RightTrap**.
3. Add an Animator component to your **Trap** game object (the root object of our death trap). Set the **Update Mode** to **Animate Physics**.
4. Go to **Window** | **Animation** and open the **Animation** View.
5. Create a new Animation Clip by clicking on the **Create** button.
6. Select the **LeftTrap** game object and make sure the record button is pressed (in the upper-left corner of the **Animation** View).
7. Move the **LeftTrap** game object to the open position.
8. Adjust the timeline a few seconds forward and move the **LeftTrap** game object to the closed position.
9. Adjust the timeline a few seconds again and move the **LeftTrap** game object to the open position again to create a looping animation.
10. Repeat steps 7-9 for the **RightTrap** game object.
11. You should have a looping animation of the trap opening and closing. You can adjust the animation until you're happy with it. You can add a pause in the open position to make it easier for the player to go through the trap without being killed.
12. Close the **Animation** View.
