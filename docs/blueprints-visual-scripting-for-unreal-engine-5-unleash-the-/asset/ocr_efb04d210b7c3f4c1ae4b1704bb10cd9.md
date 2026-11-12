# Figure 3.7 – Making the variable editable in the instances placed in the Level

8. Drag the `BP_Fire` variable from the **My Blueprint** panel and drop it into **Event Graph**. Choose the **Get BP_Fire** option to create a node. Drag from the blue pin of the **BP_Fire** node and drop in the graph to open **Context Menu**. Search for `hidden` and choose the **Set Hidden in Game (P_Fire)** function:

![](bbox=[0.125, 0.28, 0.865, 0.5])

# Figure 3.8 – Searching for a function in Context Menu

9. Right-click **Event Graph** and add **Event Hit**. Connect the **Event Hit** node to the **Set Hidden in Game (P_Fire)** node. The **New Hidden** parameter must be unchecked. These actions will unhide the particle system component of the instance referenced by `BP_Fire` when an instance of the `BP_Barrel` Blueprint is hit: