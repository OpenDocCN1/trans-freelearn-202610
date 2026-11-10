# Using Sub-State Machines in Animator Controller

This recipe shows how to use Sub-State Machines. This concept helps to organize the flow in Animator Controller.

# Getting ready

To start, you need to have a character with a few animations. In this example, we are using a character with **NormalIdle**, **NormalMove**, **CombatIdle**, and **CombatMove** animations. These animations will be then grouped in two **Sub-State Machines: Combat** and **Normal**.

You can also open the provided example Unity project and go to the `Chapter 06` `Handling combat\Recipe 01 Using sub state machines in animator controller` directory. In the `Rigs` directory you, will find all the necessary animations. In the `Controllers` directory, you can find the finished controller with two Sub-State Machines. You can find the **Humanoid** character in the `Example.unity` scene. If you press the space bar, our character will switch from **Normal** to **Combat Sub-State Machine**.

# How to do it...

To create a Sub-State Machine in a controller, follow these steps:

+   1. Create and open a new Animator Controller.
+   2. Right-click on the empty space and choose **Create Sub-State Machine**. A new Sub-State Machine will be created. Name it `Normal`.
+   3. Create another Sub-State Machine and name it `Combat`.
+   4. Create one `bool` parameter: **CombatState**.
+   5. Create two transitions:
+      - **Combat** | **Normal** with no conditions.
+      - **Normal** | **Combat** with no conditions.
+   6. Double-click on the **Normal** Sub-State Machine.
+   7. Create a `float` **Speed** parameter.
+   8. Drag and drop the **NormalIdle** and **NormalMove** animations to the controller (make sure you are in the **Normal** Sub-State Machine).