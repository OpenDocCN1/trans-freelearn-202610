+   • The next field is **Blackboard**, and this is responsible for telling the Decorator which condition returns **True**. In this instance, we want to set the **Blackboard Key** to **State**. Note the properties change. Change the **Key Query** value to the condition we apply to the **Key** value. Then lastly, set the **Key** value to the value we are checking our condition on.

Let's go back to our Behavior Tree:

+   1. The first node in the Sequence branch is a **Selector** node. We want this behavior because we have two conditions that we want to be checked for, but we know that only one condition can be met at a time. The next node we need to create within this Sequence branch is a **Wait** node. This will simulate our dog finding or animating while it sniffs out or eats its food.

+   2. The first node within our **Selector** node is the node responsible for moving to our bowl assuming we have it, using **Move To**. The **Move To** node takes one Blackboard Key argument, and this is compatible with Actors or Vectors. This provides a limited but direct way to execute movement from the tree:

![](bbox=[0.179, 0.425, 0.826, 0.63])

+   3. The next node within our **Selector** node needs to be created. So, let's go back to our Content Browser, and create a new Blueprint. Under **Custom Classes**, we want to search for **BT**, and we will create **BTTask Blueprint Base**. This will be called **FindBowl**, and it will be responsible for finding the food bowl for our dog.

+   4. We will now save this new asset, go back to our dog tree under our **Selector** node, and create our new **FindBowl** task after the **Move To** node. We have now said that this node will execute if other child conditions, such as **Foodbowl**, is set as **false**.