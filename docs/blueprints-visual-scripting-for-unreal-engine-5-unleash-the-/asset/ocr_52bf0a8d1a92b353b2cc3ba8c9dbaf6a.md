+   1. In the content browser, double-click the BT_EnemyBehavior asset to open the Behavior Tree Editor.
+   2. Select the **Move To Patrol** sequence node. In the **Details** panel, change **Node Name** to `Wander`.
+   3. Select the Move To task node and change **Blackboard Key** to `WanderPoint`.
+   4. Drag a wire out from the **Wander** sequence node to the left of the other nodes and add the BTTask_FindWanderPoint node.
+   5. In the **Details** panel, set **Wander Key** to `WanderPoint`. Also, change **Node Name** to `Get Next Wander Point`.
+   6. Save and close the Behavior Tree Editor.

That is all we need to change in the Behavior Tree to include the enemy wandering behavior. There are some adjustments that we need to do in the two blueprints.

# Last adjustments and test

We will remove **Event BeginPlay** of BP_EnemyCharacter that was used to set `Patrol Point`. We don't need it anymore because we are using the random `WanderPoint`.

The other change we need to do is modify the **Target Goal** value of `FirstPersonCharacter` to be higher so that the game can continue for longer. We will set this value to `20` so that the player must eliminate 20 enemies before winning the game.

These are the steps to adjust the blueprints:

+   1. Open the BP_EnemyCharacter blueprint.
+   2. In the Event Graph, delete **Event BeginPlay** and all the nodes connected.
+   3. Compile, save, and close the Blueprint Editor.