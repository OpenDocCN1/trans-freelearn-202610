If already primed, destroy self. The result of this chain of nodes should look like the following screenshot:

![](bbox=[0.125, 0.15, 0.865, 0.34])

# Figure 6.23 – Event Hit actions

Once you have left a useful comment around the Blueprint nodes, compile, save, and click on Play to test the new interactions. You should see and hear the Cylinders explode once they have been shot twice by the player's gun.

We saw how to use the Branch node to decide the execution flow based on the Primed variable that represents the current state of the Cylinder. We also learned how to use Blueprint nodes to play sound and spawn particle effects.

# Summary

We've now started going down the path of making our game feel satisfying to the player. We have added sound and visual effects, a player character that has most of the capabilities we would expect from a modern shooter, and targets that react to the player's interactions. The skills we have covered in the first chapters have been combined to start creating increasingly complex and interesting behavior.

In this chapter, we created some customized player controls to allow sprinting and zooming in with our gun. In the process, we explored how the movement controller translates information from a player's inputs into the game experience. We also opened the door to creating simple animations