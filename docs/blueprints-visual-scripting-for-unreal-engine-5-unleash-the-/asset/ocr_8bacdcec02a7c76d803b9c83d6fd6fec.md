![](bbox=[0.127, 0.092, 0.866, 0.378])

# Figure 6.10 – Character Movement node

+   4. Click and drag the output pin from the **Character Movement** node to empty grid space and type `walk speed`. Select the **Set Max Walk Speed** option. It will connect the **Character Movement** node to the new node, setting the maximum walk speed value.

+   5. Connect the **Pressed** output execution pin from the **InputAction Sprint** trigger to the input execution pin of the **Set Max Walk Speed** node, to enable you to press **Left Shift** to modify the maximum movement speed.

+   6. Finally, change the **Max Walk Speed** value within the node from `0.0` to `2200`, to provide a nice boost of speed over the default of `600`.

We also need to ensure that the player slows down again once the *Shift* key is released.

+   7. To do so, drag the output pin from the **Character Movement** node again, and then search for and select another **Set Max Walk Speed** node to place it on **Event Graph**. This time, connect the **Released** output execution pin of the **InputAction Sprint** node to the input execution pin of the new node. Then, change the **Max Walk Speed** value from `0.0` to the default of `600`. To keep up with our good