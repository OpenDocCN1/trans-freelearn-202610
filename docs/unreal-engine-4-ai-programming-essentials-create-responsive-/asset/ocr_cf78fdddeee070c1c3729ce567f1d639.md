6. With the **MoveToLocation** node ready, we now need to supply it with a random location. We will grab the current location of the controlled pawn, create an additional vector with a random value of -255 to 255 for the **X** and **Y** variables, leaving **Z** to **0.0**. Then, we will add the location from the controlled pawn to the vector we just created. You should have a blueprint similar to the following setup:

![](bbox=[0.203, 0.254, 0.801, 0.463])

7. Let's finalize this and move this new random location blueprint to the **MoveToLocation** node that we set up previously. Now, let's connect the results of the addition of the two vectors to the destination of the **MoveToLocation** node. With this done, you should have a blueprint setup similar to the following preview:

![](bbox=[0.19, 0.575, 0.81, 0.793])

Save it all!