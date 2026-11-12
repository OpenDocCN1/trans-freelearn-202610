points is noticeable.

With our patrol points established, we can move on to building the intelligence of our enemy.

# # Creating the Blackboard keys

A Blackboard stores information using keys and values.
BB_EnemyBlackboard will have two keys, one for storing the current patrol point and another to store a reference to the player character. This information will be referenced by the Behavior Tree.

These are the steps to create the keys:

+   1. Open `BB_EnemyBlackboard` from the content browser.
+   2. Click on **New Key** and select **Object** as **Key Type**.
+   3. Name this new key `CurrentPatrolPoint`.
+   4. Click on the expansion arrow next to **Key Type** and change **Base Class** to **Actor** with the dropdown:

![](bbox=[0.122, 0.555, 0.871, 0.891])