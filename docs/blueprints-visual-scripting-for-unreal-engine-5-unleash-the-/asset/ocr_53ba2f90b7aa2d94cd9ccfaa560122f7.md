![](bbox=[0.125, 0.09, 0.865, 0.425])

Figure 10.12 – The task nodes of the Investigate Sound sequence

Follow these steps to add the task nodes:

+   1. Drag a wire from the Investigate Sound sequence node and add a Move To task node. In the Details panel, change Blackboard Key to LocationOfSound.

+   2. Drag a wire from the Investigate Sound sequence node and add a Wait task node. In the Details panel, set Wait Time to 3.0 seconds.

+   3. Drag another wire from the Investigate Sound sequence node and add a BTTask_ClearBBValue task node. In the Details panel, change Key to HasHeardSound and change Node Name to Reset Heard Sound.

+   4. Save the Behavior Tree and close the Behavior Tree Editor.

Now, we need to return to the BP_EnemyController blueprint and add some actions that will instruct our AI how to react to sounds in the game.