+   7. So, get **Offset** from the variables. We want to put this in modulo (%) and then connect it out to the greater than (>) compare node.

+   8. Next, we want to get **ChanceWeight** from the variables. Then, we will multiply this by the **Max N** value to calculate the probability range. We will do two things with the results of this calculation, as in the next steps.

+   9. First, we want to get the difference from the **Max N** value with the results. Then, we will connect this to modulo (%).

+   10. Next, we want to add the probability range to the results of modulo (%). Then, we will pipe this into a clamp for safety. We will set the **Min** value to 0 and **Max** to **Max N**. The results of this clamp plugs into the less than (<) of the compare node shown as follows:

![](bbox=[0.18, 0.352, 0.823, 0.51])

# ## Creating transitions

Transitions help apply the appropriate actions to what's being perceived in the AI's environment. They also continue the execution flow, and this is what makes Finite State Machines so special. We will apply this in our project by transiting into Flee or Attack state, and after sometime, it will calculate whether it should flee or continue attacking. Once no enemy is found, the AI will transition back into a wandering state. Follow these steps:

+   1. So, from the **Branch** node we last created, let's set our **State** variable on our **True** pin for the **Branch** node. Then, we will set another **State** variable on the **False** pin for the **Branch** node. From there, let's set the **True Set State** value to **1**, then **False Set State** to **2**.

+   2. Next, let's rename our **Hero** variable to **Target** just to make it easier. Next, we want to set **Target** after our two other **Set State** variables.

+   3. Then, we want to connect both the **Set State** variable nodes to the **Set Target** variable node. The **Set Target** node should be set to **Break the ForEachLoopWithBreak**.