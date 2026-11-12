The following screenshot shows the **Investigate Sound** sequence node with the decorator. Note that the priority of nodes in a Behavior Tree is left to right:

![](bbox=[0.125, 0.175, 0.865, 0.405])

Figure 10.11 – The Investigate Sound sequence node

We have created the blackboard keys, the sequence node, and the decorator. Our next step is to create the task nodes of the **Investigate Sound** sequence.

# # Setting up the investigating tasks

The **Investigate Sound** sequence will be like the **Attack Player** sequence. If an enemy is patrolling and hears a sound, they will move to the location of the sound: