
![](img/aaaa027482c900e215ba6bfab3c17337_103_0.png)

- Let's open this `DogController` class and find the Event Begin Play node.

- Right-click on the graph and find the Run Behavior Tree node.

- Then, in the BTAsset pin, let's find DogTree, which we created earlier. Then, in the game, we will assign our DogController class to our character. This will spawn with the character and immediately run DogTree:

![](img/aaaa027482c900e215ba6bfab3c17337_103_1.png)

- Back to our Behavior Tree, let's pull down an arrow and create a new Selector composite in our graph. This is the start in the tree shown in the preceding screenshot. It will run our patented MailmanRadar Service node. What is unique about Service nodes is that they're used on composites only and are responsible for handling tasks that require running with the tree.

- For now, let's construct the next three node options in the tree. This will be three sequence nodes with Blackboard Compare Decorators on them. Decorators can apply conditions to our Composites or Task nodes. These decorators will check for the appropriate entry state to begin the execution.

- So, create our Composite node from the beginning of this tree. Then, let's right-click on this node and add a Decorator that we want to look for in the Blackboard:

![](img/aaaa027482c900e215ba6bfab3c17337_104_0.png)

## Blackboard Compare Decorator

There are a few fields we want to take notice of in the pane:

- Let's start with Flow Control. This has two properties: Notify Observer and Observer aborts. Notify Observer tells Flow Control which operation to fire on. The On Result Change fires when the computed condition to enter this tree changes. So, if we were barking originally but are now idle, the tree's Flow Control will fire off an Abort event. Which part of the tree is aborted is controlled by Observer aborts. The first option is Self. This is straightforward in that it aborts further execution within this tree. The second option is Low Priority. This option basically sees the rest of the tree will be aborted. This is good if you want the whole tree to start over. The last option is simply a combination of the first two options I mentioned.

- The next field is **Blackboard**, and this is responsible for telling the Decorator which condition returns **True**. In this instance, we want to set the **Blackboard Key** to **State**. Note the properties change. Change the **Key Query** value to the condition we apply to the **Key** value. Then lastly, set the **Key** value to the value we are checking our condition on.

Let's go back to our Behavior Tree:

- The first node in the Sequence branch is a **Selector** node. We want this behavior because we have two conditions that we want to be checked for, but we know that only one condition can be met at a time. The next node we need to create within this Sequence branch is a **Wait** node. This will simulate our dog finding or animating while it sniffs out or eats its food.

- The first node within our **Selector** node is the node responsible for moving to our bowl assuming we have it, using **Move To**. The **Move To** node takes one Blackboard Key argument, and this is compatible with Actors or Vectors. This provides a limited but direct way to execute movement from the tree:

![](img/aaaa027482c900e215ba6bfab3c17337_105_0.png)

- The next node within our **Selector** node needs to be created. So, let's go back to our Content Browser, and create a new Blueprint. Under **Custom Classes**, we want to search for **BT**, and we will create **BTTask Blueprint Base**. This will be called **FindBowl**, and it will be responsible for finding the food bowl for our dog.

- We will now save this new asset, go back to our dog tree under our **Selector** node, and create our new **FindBowl** task after the **Move To** node. We have now said that this node will execute if other child conditions, such as **Foodbowl**, is set as **false**.

- Now that we have our first State tree, we should update the comment on our tree nodes to understand what it means in shorthand. Look at how I've done in mine in the following screenshot:

![](img/aaaa027482c900e215ba6bfab3c17337_106_0.png)

- Now moving onto the next tree, we must create a similar setup, starting with a **Selector** composite node. We want to right-click, add a Decorator, and search for **Blackboard**. This needs to check whether the **State** variable is equal to `Barking` to execute this branch entry.

- Next, we will create a **Sequence** Composite node responsible for telling our AI to sniff around for the mailman. This will have a Blackboard Decorator on the composite to check if the **Mailman** variable is set. We also want to add another **Loop** Decorator responsible for calling this twice if the condition is met.

- With this set, the next three nodes within this **Sequence** branch will first scan the area, move our dog, and then wait at the location sniffing. So, in order to scan our area, I want to use **EQS (Environment Query System)**.

## Environment Query System

I want to cover this more in the next chapter, which covers how our AI can sense the environment, but I will at least introduce the idea and how we will use it today. Environment Query System is responsible for allowing the context object to call a query request to generate information based on filters and the test applied to the request. A query would contain a template of instructions for EQS to run. So, for example, if you want to scan an area for possible hiding places, you can scan for those actors that represent the hiding places. Then, you can apply filters to your query to get rid of the results you don't want to score and score your results based on direction from enemy for the most optimal hiding position.

In our case, we want to choose a strategic position to sniff out our Mailman. We will do this by creating Environment Query. Let's create this as follows, and in our next chapter, we will cover other ways of using EQS:

- So, with this in mind, let's head to the Content Browser and select our AI folder. Right-click on the folder and go to Misc and then to Environment Query. Name it EQS_PointsAround.

- Let's open this and go to root. In here, you'll notice a similar visual interface to that of Behavior Tree. Pull from the root and find SimpleGrid. This will generate a grid of items around our actor to be scored and returned to the requester.

- We want to change the Grid Size value to 2048 and that of Space Between to 512. The rest of the settings will be default; let's now add a test as shown in the following screenshot:

![](img/aaaa027482c900e215ba6bfab3c17337_107_0.png)

- This test will filter and then score our results based on the direction from our dog. In order to determine the DOT test to score, we have to tell the test what we want to have tested. So, Line A will be our item's Rotation. Line B will be the direction between us and the item.

- Lastly, as our dog will need to search the whole area, we would need our dog to have the opportunity to survey the whole area. By enabling Absolute Value, we are able to get the results to score in both directions.

- We want to adjust our filter to eliminate items directly in front of us or to our right by choosing a range in between 0 and 1. Let's change our Filter Type value to Range, the Float Value Min value to 0.4, and the Float Value Max value to 0.85.

- The last thing here is **Score**, and we will leave this as the default. Let's save this and go back to our Behavior Tree:

![](img/aaaa027482c900e215ba6bfab3c17337_108_0.png)

- From the **Sequence** node we created with the **Mailman is NotSet** Decorator, we want to now run our EQS query and store the results in our **Location** Blackboard key. We can pull from the node and find **Run EQS Query**.

- Then, we need to select our **EQS_PointsAround** option and set our **Location** Blackboard key.
