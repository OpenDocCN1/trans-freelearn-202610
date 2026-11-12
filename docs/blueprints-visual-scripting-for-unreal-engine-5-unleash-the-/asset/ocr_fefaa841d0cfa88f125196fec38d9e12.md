+   4. Right-click on the empty space of **Event Graph** and add an **Event Hit** node. The following screenshot shows the first part of the **Event Hit** actions:

![](bbox=[0.125, 0.175, 0.865, 0.405])

Figure 10.22 – The first part of the Event Hit actions

+   5. Drag a wire from the **Other** output pin of the **Event Hit** node and add a **Cast To FirstPersonProjectile** node. Connect the white pins of **Event Hit** and **Cast To FirstPersonProjectile**.

+   6. Drag a wire from the white output pin of the **Cast To FirstPersonProjectile** node and add a **Branch** node.

+   7. Drag a wire from the **Condition** pin of the **Branch** node and add a **Greater** node.

+   8. Drag a wire from the top input pin of **Greater** node and add a **Get Enemy Health** node. Set a value of `1` in the bottom input of the **Greater** node.

+   9. Drag a wire from the **True** output pin of the **Branch** node and add a **Decrement Int** node.

+   10. Drag a wire from the input pin of the **Decrement Int** node and add a **Get Enemy Health** node.

+   11. Drag a wire from the **False** output pin of the **Branch** node and add a **Spawn Actor from Class** node. In the **Class** parameter, select the