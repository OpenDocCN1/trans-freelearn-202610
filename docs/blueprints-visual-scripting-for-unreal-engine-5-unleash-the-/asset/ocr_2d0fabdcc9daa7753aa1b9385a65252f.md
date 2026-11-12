Now, we will modify **BP_CylinderTarget** to call the **CheckGoal** event each time a target is destroyed:

+   1. In the **Content Browser**, access the `/Content/FirstPersonBP/Blueprints` folder and double-click on the **BP_CylinderTarget** Blueprint.
+   2. Move **Event Graph** to the end of the **Event Hit** actions. Drag the **DestroyActor** node to the right to make room for another node.
+   3. Drag a wire from the **As First Person Character** output pin of the **Cast To** node and add the **Check Goal** node. Connect the white output pin of the `++` node to the white input pin of the **Check Goal** node and connect the output pin of the **Check Goal** node to the white input pin of the **DestroyActor** node:

![](bbox=[0.123, 0.403, 0.866, 0.563])

# Figure 8.45 – Calling the Check Goal event

+   4. Compile, save, and play the game. If all Blueprints are set up correctly, then you should see the game pause and the **WinMenu** appears as soon as you destroy a second target. Clicking on the **Restart** button will reload the level and clicking **Quit** will close the session: