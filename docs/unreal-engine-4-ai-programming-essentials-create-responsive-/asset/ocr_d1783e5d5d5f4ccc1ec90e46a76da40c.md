# # Back to the action

Let's focus back on Enemy EventGraph at the bottom. We want to implement a way for our AI to wander. Perform the following steps:

+   1. Start by searching for Get Controlled Pawn.

+   2. Next, we want to perform Get Actor Location on the controlled pawn. Then the vector it returns will be pinned to a Vector + Vector node.

+   3. From there, we want to create a Random Unit Vector node to generate a random vector from 0-1 on each individual axis.

+   4. We must pull from the Stream variable again and create Random Float in Range from Stream. Set the Min value to -550 and Max to 550. Then, we want to multiply what it returns by Random Unit Vector.

+   5. Lastly, the multiplied vector is added to the Vector + Vector node that we created in Step 2:

![](bbox=[0.186, 0.408, 0.812, 0.624])

+   6. Now, we need to take the results of the Vector + Vector node and create a Move to Location node to instruct the possessed pawn to move to the Move to Location node of the vector.

+   7. We will take a look back at our first state, which checks for Actors within a radius. After ForEachLoopWithBreak is completed, we want it to execute Move to Location. So, pull from that and first create DoOnce.

+   8. We will do this because we don't want the bot to constantly be told to move to a location; we would rather have it do this after a random delay. So, now connect the Completed pin to Move to Location.