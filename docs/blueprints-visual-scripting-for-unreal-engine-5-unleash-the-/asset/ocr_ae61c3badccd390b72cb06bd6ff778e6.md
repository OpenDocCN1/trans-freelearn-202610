commenting practice, click and make a selection box around all four nodes, right-click on one of the selected nodes, and select **Create Comment from Selection**, then label the group of nodes as `Sprint`.

+   8. Now compile, save, and press **Play** to test your work. You should notice a significant boost in speed if you press down the *Left Shift* key.

We learned how to map a keypress to an Action and modify the max speed of the character to simulate a sprint ability. Our next step is to add an ability to the player that allows them to get a closer view of a target.

# # Animating a zoomed view

A core element of modern first-person shooters is a variable **field of view (FOV)** in the form of a player's ability to look down the scope of a gun to get a closer view of a target. This is a significant contributor to the feeling of accuracy and control that modern shooters provide. Let's add a simple form of this functionality to our prototype. To do that, follow these steps:

+   1. In an empty section of the grid next to your **Mouse input** nodes, right-click, search for `zoom`, and add a **Zoom** Event node.

+   2. We want to modify the FOV value that is contained within the **First Person Camera Component**, so go to the **Components** panel and drag **First Person Camera** out onto **Event Graph**.

+   3. Drag the output pin of **First Person Camera** node into empty grid space, search for the **Set Field Of View** node, and place it. Lowering the FOV gives the effect of zooming into a narrower area in the center of the screen. Since the `Default` FOV value is set to `90`, for our zoom, let's set the FOV in the set node to `45`, like this: