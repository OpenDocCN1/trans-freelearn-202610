# # State transition

This concludes what is necessary for our transition out of state 1. Next, we will do what is required for state 2, and this will allow us to detect the changes necessary to exit this state back to state 0 once we no longer see the enemy. Perform the following steps:

+   1. Going back to the switch node on State, which we created earlier (in Step 55) and creating a new connection from 2, let's create GetAiController.
+   
+   2. Next, we need to check whether our Enemy Actor variable is valid. We will do this by dropping out the Enemy Actor variable, pulling from the pin, and then creating the IsValid node.
+   
+   3. If the variable is valid, we want to move our AIController toward our enemy. We can do this by pulling from Return Value on GetAiController and then creating Move To Actor. Then, we can connect our valid Enemy Actor variable to our Move To Actor goal input.
+   
+   4. Next, we want to use the Tick event within the AIController to check whether we have the line of sight of our enemy when we're in state 2. We can do this by focusing on the Tick event, pulling from it and then creating a Branch node.
+   
+   5. Then, drop the State variable and check whether it equals 2. The results of this equal node are pumped into the condition of the Branch node.
+   
+   6. Looking at the Branch node, pull from True and create the getAiController node. Then, we will pull from Found and create a Branch node.
+   
+   Now, we need to create two new variables to maintain the chasing of our enemy. This will allow us to either timeout our chase after a designated amount of time or have the case end immediately, forcing the AI to return to routing between the waypoints we created.
+   
+   7. So, let's create a new variable called TimeoutChase; this will be a Boolean. Next, create a ChaseTime variable, and this will be a Float. This will tell our execution line to switch between different flows if we don't want to timeout our chase.
+   
+   8. Focusing again on the last Branch node we created, we want to plug in our newly created Timeout Chase variable into the condition.