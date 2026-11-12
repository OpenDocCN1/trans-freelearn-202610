# # Sending the instructions

Now that we have our project set up and saved—or not saved for our brave fellows—let’s move forward to the fun part: blueprints! Let’s take a look at the steps to send the instructions:

+   1. Open your `MyController` blueprint within **Content Browser** and zoom in to the **EventGraph**:
+      So, the plan is to have our bot move around randomly. This will be built in a simple fashion, so we will do the computing by hand.
+   
+   2. First, let’s create an **Event Tick** node that will be triggered in every frame as the game runs.
+   
+   3. We will add a **Delay** node to receive signals from **Event Tick** to set the **Duration** pin to **1**.
+   
+   4. Next, we will add a **Move To Location** node, which will signal the `AIController` class to tell its pawn to move to the destination point specified.
+   
+   5. As we didn’t use **Path Finding** at this time, which is a subject we will touch upon later, go to the **MoveToLocation** node and set **bUsePathFinding** to false or leave it unselected.

The EventGraph for your `MyController` class should look similar to the following screenshot:

![](bbox=[0.19, 0.54, 0.81, 0.735])