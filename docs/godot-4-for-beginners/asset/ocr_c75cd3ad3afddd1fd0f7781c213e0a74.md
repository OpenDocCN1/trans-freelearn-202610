+   3. Save the **Player** scene as player.tscn, and then drag and drop the file from **FileSystem** into the **Level1** scene.

You now have a player in your level! However, if you play the game, the player is stationary because no movement has been implemented yet. We will implement this later.

# TileMap collisions

As stated before, our current level is just a painting; none of the tiles have collision detection, and our player cannot move. To set up collisions on our tiles, we need to add an element to our physics layers. To do this, follow these steps:

+   1. Select the **TileMapLayer** node in **Inspector** and click on **Tile Set**.
+   2. In the **Physics Layers** property, click **Add Element**. This is shown in Figure 6.19:

![](bbox=[0.32, 0.37, 0.74, 0.66])

Figure 6.19 - Adding an element to Physics Layers of a TileSet