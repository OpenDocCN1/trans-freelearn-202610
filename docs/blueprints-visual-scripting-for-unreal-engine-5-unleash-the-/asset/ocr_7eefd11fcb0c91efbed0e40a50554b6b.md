![](bbox=[0.123, 0.091, 0.823, 0.693])

Figure 9.53 – The Decorator settings

+   9. Save the Behavior Tree. The Decorator will only allow the node to run when the `PlayerCharacter` key has a reference. It will also abort the Move to Patrol sequence to execute the Attack Player sequence.

The only parts missing to make the enemy chase the player are the Task nodes of the Attack Player sequence.