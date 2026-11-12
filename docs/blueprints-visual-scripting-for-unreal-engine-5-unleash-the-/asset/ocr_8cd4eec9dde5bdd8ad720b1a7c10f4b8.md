+   9. Drag a wire from the Key input pin of Clear Blackboard Value and add a Get Key node.

+   10. Drag a wire from the white output pin of Clear Blackboard Value and add a Finish Execute node. Check the Success parameter of the Finish Execute node.

+   11. Compile, save, and close the Blueprint Editor. In the content browser, rename BTTask BTTask_ClearBBValue.

We will use our new Task to clear the PlayerCharacter reference after an enemy attack to give the player a chance to run away.

# Adding conditions to the Behavior Tree

We will need another Sequence node that will connect to tasks to get the enemy to chase the player and a Selector node that will run one of the two Sequence nodes. We need to ensure that the new Tasks only run when the enemy sees the player. To do this, we'll add a Decorator. A Decorator attaches to the top of a node and provides conditions that must be met before the Tasks can be performed.

The following screenshot shows the updated version of our Behavior Tree after the changes in this section. The Tasks of the Attack Player sequence will be added in the next section: