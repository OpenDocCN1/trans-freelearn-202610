event named **CheckGoal** in the **FirstPersonCharacter** Blueprint that will be called by BP_CylinderTarget each time a target is destroyed.

These are the actions of the **CheckGoal** custom event that we will create:

![](bbox=[0.126, 0.192, 0.868, 0.335])

Figure 8.44 – Checking whether the goal was achieved

To do that, follow these steps:

+   1. Right-click on the Event Graph of the **FirstPersonCharacter** Blueprint and add a custom event. Rename it `CheckGoal`.
+   2. Drag a wire from the white output pin of the **CheckGoal** node to empty grid space and add a **Branch** node.
+   3. Drag a wire from the **Condition** input pin of the **Branch** node to empty space and add a **Greater Equal** node.
+   4. Drag a wire from the top input pin of the **Greater Equal** node and add a **GET Targets Eliminated** node.
+   5. Drag a wire from the bottom input pin of the **Greater Equal** node and add a **GET Target Goal** node.
+   6. Drag a wire from the **True** output pin of the **Branch** node and add a **Delay** node. Enter `1.0` in the **Duration** parameter. This node is used to wait 1 second before showing the **WinMenu**.
+   7. Drag a wire from the **Completed** output pin of the **Delay** node and add the **End Game** node. This node will call the **End Game** custom event that we created to show the **WinMenu**.
+   8. Compile, save, and close the Blueprint Editor.