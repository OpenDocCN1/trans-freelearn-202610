+   4. With the **Warrior** game object selected, go to **Window** | **Animation** to open the **Animation** View.
+   5. Create the required Animation Clips using the **Animation** View (the same way as in the *Creating a frame-by-frame sprite animation with the Animation View recipe*). In this example, we use two animations: **Warrior2dIdle** and **Warrior2dWalk**.
+   6. An Animator Controller will be automatically created and assigned to the Animator component on the **Warrior** game object.
+   7. Open the created Animator Controller (you can easily do it by selecting the **Warrior** game object in the **Hierarchy** and going to **Window** | **Animator**).
+   8. Create a new `float` parameter in the Animator Controller and name it Speed. We will use it for controlling the animation flow.
+   9. Create two transitions: from **Warrior2dIdle** to **Warrior2dWalk** and from **Warrior2dWalk** to **Warrior2dIdle**. The first one should have a condition of Speed greater than 0.1 and the second should have a condition of **Speed** less than 0.1. See the following screenshot:

![](bbox=[0.13, 0.44, 0.86, 0.79])

10. Set the **Has Exit Time** to false and the **Transition Duration** to 0 in both transitions (you cannot blend frame by frame sprite animations).