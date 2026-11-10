The model is not skinned and has no rig; instead, the moving elements are separate objects. We will animate them with Unity's **Animation** View. You can also use the example Unity project provided and go to the `Chapter 02 Working with the animation view\Recipe 06 Animating triggers - creating a death trap` directory. You will find an `Example.unity` scene there. If you play the game, you can see the trap working. If you open the scene, you will find the **Trap** game object with the **LeftTrap** and **RightTrap** game objects as its children. Those child objects are the only ones animated in this example. They have trigger objects as their children. Those trigger objects kill the player when he enters them. All other child objects of the **Trap** game object are only decorations and are optional.

# How to do it...

To animate the trigger position and create a death trap, follow these steps:

+   1. Create an empty object in the scene and name it **Trap**. It will be a parent object for our moving parts.
+   2. Put your models into the scene and parent them to the **Trap** game object. The models should have descriptive names, for instance, **LeftTrap** and **RightTrap**.
+   3. Add an Animator component to your **Trap** game object (the root object of our death trap). Set the **Update Mode** to **Animate Physics**.
+   4. Go to **Window** | **Animation** and open the **Animation** View.
+   5. Create a new Animation Clip by clicking on the **Create** button.
+   6. Select the **LeftTrap** game object and make sure the record button is pressed (in the upper-left corner of the **Animation** View).
+   7. Move the **LeftTrap** game object to the open position.
+   8. Adjust the timeline a few seconds forward and move the **LeftTrap** game object to the closed position.
+   9. Adjust the timeline a few seconds again and move the **LeftTrap** game object to the open position again to create a looping animation.
+   10. Repeat steps 7-9 for the **RightTrap** game object.
+   11. You should have a looping animation of the trap opening and closing. You can adjust the animation until you're happy with it. You can add a pause in the open position to make it easier for the player to go through the trap without being killed.
+   12. Close the **Animation** View.