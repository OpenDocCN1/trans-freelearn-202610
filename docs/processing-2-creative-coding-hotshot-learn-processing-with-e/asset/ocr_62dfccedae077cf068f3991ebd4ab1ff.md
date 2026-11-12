![](bbox=[0.348, 0.145, 0.647, 0.393])

# # Objective Complete - Mini Debriefing

For the first task of our current mission, we created all the graphical elements we needed for our game. In steps 1 to 3, we created a grid that looks like a math notebook to make the game look like it has been drawn in class (during a break of course!).

In step 4, we added a randomly generated lunar landscape. We randomly generated the moon’s ground level offsets in our `setup()` method and stored it in an array. This way, we can make sure that the moon stays the same during one game, but can be recreated without a hassle when the player has finished a level and restarts it. To draw the moon, we created a complex polygon using the `beginShape()` and `endShape()` methods and defined the vertices of the polygon using the `vertex()` command.

The landing platform consists of a rectangle that is aligned to the grid to make it look like we have colored some of the squares. We also added feet to the platform and made them touch the ground level of the moon by using the array from the previous section.

Finally, we used an image of the ship and created a transformation context to be able to rotate it. We need the new context to make sure that our calls to `translate()` and `rotate()` only affect the drawing operations we use for the ship, and not all the other sections of our screen. A new context is started with `pushMatrix()` and ends with `popMatrix()`. Every scale, translate, or rotate operation we use in this context only affects the drawing operations of it. Our grid, moon, and landing platform are unaffected by them.