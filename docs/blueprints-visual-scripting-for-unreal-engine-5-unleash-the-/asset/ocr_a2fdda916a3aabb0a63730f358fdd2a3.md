# # Teaching our AI to walk with the Behavior Tree

Behavior Tree is a tool used to model the behavior of characters. It has control flow nodes and task nodes.

The two primary control flow nodes you will utilize are **Selector** and **Sequence**. A **Selector** node runs each of the nodes connected underneath it – called its **children** – from left to right, but it succeeds and stops running as soon as one child successfully runs. Thus, if a **Selector** node has three children, then the only way the third child node will run is if the first two children failed to execute because the conditions attached to them were `false`. A **Sequence** node is just the opposite. It also runs all the children in a sequence from left to right, but the **Sequence** node only succeeds if all the children succeed. The first child to fail causes the whole sequence to fail, ending the execution and aborting the sequence.

Follow these steps to create our first Behavior Tree:

+   1. In the content browser, double-click the `BT_EnemyBehavior` asset to open the Behavior Tree Editor.
+   
+   2. In the **Details** panel, click the **BEHAVIORTREE** category and select **BB_EnemyBlackboard** as **Blackboard Asset**. The **KEYS** dropdown of **BB_EnemyBlackboard** will appear in the **Blackboard** panel at the bottom: