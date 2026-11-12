3. Drag another wire from the **Attack Player** sequence node and add a **BTTTask_ClearBBValue** task node. In the **Details** panel, change **Key** to **PlayerCharacter** and change **Node Name** to `Reset Player seen`:

![](bbox=[0.125, 0.175, 0.825, 0.65])

# Figure 9.55 – Clearing the PlayerCharacter value in the Blackboard

4. Save the Behavior Tree and close the Behavior Tree Editor. Press the **Play** button in the Level Editor to test the enemy behavior.

As you navigate the player character in front of the patrolling enemy, the enemy will stop its patrol and chase the player. When the enemy reaches the player, it will stop for 2 seconds before returning to its patrol path. If it re-establishes a line of sight with the player, then it will interrupt its patrol and begin chasing the player again.