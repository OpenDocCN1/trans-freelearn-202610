
- 12. Let's focus back on the Designer tab of this user widget. Then, click on the ResetButton event we made in previous steps.

- 13. We want to go down in Events and click on the + option on OnPressed. This will give us the event we just created.

- 14. Here, we want to pull from the pin and call Get All Actors Of Class. Next, we want the Actor Class value to be Flocking Ball. From there, Out Actors will return all instances of Flocking Ball in the world into an array.

- 15. The last thing to do is pull from Out Actors and call Reset Ball. This will notify all our agents to reset the simulation:

![](img/aaaa027482c900e215ba6bfab3c17337_150_0.png)

- 16. Now, we must go back to Flocking Ball in EventGraph to add more blueprint code. Find an empty place above the Reset Ball event.

- 17. Right-click and search for Event Possessed. Cast a new controller to PlayerController and then pull from As Player Controller and search for Create Widget. Set the Class pin for this node to FlockingUI.

- 18. Next, we must pull from Return Value and call Add to Viewport. Lastly, we need to show our mouse to the player so that they can interact with the widget.

- 19. Right-click and search for Get Player Controller. Pull from Return Value and search for Show Mouse Cursor. Comment this area Enable Flocking UI & Mouse Visibility:

![](img/aaaa027482c900e215ba6bfab3c17337_151_0.png)

Let's compile everything. Click on Save All and go back to our Map. If you hit Play, you should see the agents bounce off the walls they collide. Then, if we turn up the different behaviors, we will see that they begin to affect the direction of our agents.

The following screenshot shows what the level looks like after putting everything together:

![](img/aaaa027482c900e215ba6bfab3c17337_151_1.png)

## Summary

Let's take some time to briefly talk about what we did in this chapter. First, we set up our agents to bounce off walls. This allows us to watch the agents simulate movement and how the four behaviors would affect them. Next, we will implement the Follower and Leader behavior, and this creates groups within the agents during simulation.

The last thing we did was implement the flocking movement, which is broken up into three different behaviors. The first behavior is Alignment, and it is responsible for aligning agents with nearby agents. The second behavior is Cohesion, and it is responsible for directing agents towards the center of the nearby agents. The third behavior is Separation, and it is responsible for directing the agent away from nearby agents.

Then, we simply created a UI to control the weights we created in the course and reset the agents at any time during the simulation.

Now that we are done covering different movement behaviors, it is time we combined all that we demonstrated about AI in this book. In the next chapter, we will create an AI character, which will patrol, seek, and destroy any enemy. So, let's move on to the next chapter to get started!

## Creating Patrol, Chase, and Attack AI

In this chapter, we will combine some of the components we used in the previous chapters, including AI Sense and other components, to have AI navigate. Then, we will add some randomness in the time-out time that will make the AI chase after the characters it detects. Now, in this chapter, we can create AI using Behavior Tree while utilizing other AI components. A combination of these components can create responsive and convincing AI behavior.

The goal of this chapter is to create an AI that will fire at you using Behavior Tree; this last AI component will benefit you because there is no perfect solution. So, understanding each of the available tools allows you to exploit the advantages of each component for your use in AI. These tools allow you to create AI that is responsive and convincing.

### Creating a Blackboard

Blackboards define the local variable space for Behavior Trees. These Blackboards can also sync with other instances of the same Blackboard. Blackboards can be created first because you'll find yourself modifying them often until you discover everything you need for your Behavior Tree.

We will create our Blackboard data first and then supply it to our Behavior Tree. Now, let's start! Here are the steps:

- 1. Right-click on the Content folder and create a new folder named AI.

- 2. Now, right-click inside the folder and scroll down to find Artificial Intelligence; then, click on Blackboard. Let's name this EnemyData.

- 3. We will open EnemyData and then create two object variables to be used by Behavior Tree. Name the first one TargetActor and the next one CurrentRoute.

> In case you're using Enumeration, you can define the enumeration the same way we will define actors for these objects.

- 4. Click on TargetActor and select the Key Type option in the drop-down menu. Then, change the Base Class value to Actor. We want to do the same with CurrentRoute, as in the following screenshot:

![](img/aaaa027482c900e215ba6bfab3c17337_154_0.png)

Behavior Tree is a tree of nodes whose structural diagram dictates the flow control, each leaf node representing the actual code for the AI to execute. This results in the AI making a sequence of decisions appropriate to the input it receives. To see a sample of what we will end up making today, here is how Behavior Tree looks in Unreal Engine 4:

![](img/aaaa027482c900e215ba6bfab3c17337_155_0.png)

There are many forms of state machines available in the world. Behavior Tree is a form of a tree with a subtree of nodes. It has composites that allow you flow control, resulting in the execution of leaves, which also have additional flow control. This level of control allows you to create deep Behavior Trees with a lot of control.

Let's begin to create our own and get a quick look of what to expect later in the course:

- 1. We will right-click on EventGraph and scroll down to Artificial Intelligence. Now, let's click on Behavior Tree and name this EnemyAI.

- 2. Open up EnemyAI, and you should see EnemyData populated in the ROOT node. If not, click on the ROOT node and set the Blackboard asset to EnemyData:

![](img/aaaa027482c900e215ba6bfab3c17337_156_0.png)

## Mid-range attack

This will be a line trace from the center of our eye. This will be how you attack the AI, and this is how the AI will attack you. We will simply run a line trace and then draw a debug line over this, creating a cool red beam out of our player's forehead! We will test to see whether it works, and after this, we will integrate it into both players.

Now, to begin creating this function, we will do something different that will save us time. We will put the function in Blueprint Function Library to be shared across every Blueprint graph.

To keep the function useful for both AIController and PlayerController, we want to give a controller as the input and then to output an actor. This function will also handle drawing the red beam where necessary:

- 1. Let's right-click, go to Blueprints and then to Blueprint Function Library. Name this EnemyLibrary and open it up.

- 2. On the left-hand side, add new Function. Name it LaserFromController. Make the first input a controller named theController and then output an actor named Hit Actor. Lastly, create a Local Variable called foundActor. Make the type Actor.

- 3. Pull from the controller and search for Get Controlled Pawn. Then, from here, we will create a line trace based on the character's rotation. Pull from Get Controlled Pawn and search for Get Actor Forward Vector.

- 4. We want to multiply Get Actor Forward Vector by 9999999.

- 5. Go back to Get Controlled Pawn from Step 3. Then, cast it to ThirdPersonCharacter, and now we can access the Mesh variable. We want the location of our head bone with Get Socket Location.

- 6. Then, pull from ReturnValue and find Vector + Vector; now, add 12.0 in the Z variable:

![](img/aaaa027482c900e215ba6bfab3c17337_157_0.png)

- 7. Now, we must add the results of Step 4 to the results of Step 6 with a Vector + Vector node. Then, we will move on to a line trace for multiple objects. Search for LineTraceForObjects and then plug in the results into the End pin of the node we just created.

- 8. Now, let's set the Start value of the node we created in the previous step to the output from the Vector + Vector node to the start. Then, we should have a beam start from his head and draw it forward in space.

- 9. Pull from Object Types and create an array; then, populate the pawn.

- 10. Pull from ActorsToIgnore and create an array; then, populate the Cast to ThirdPersonCharacter node into the 0 element.

- 11. From LineTraceForObjects, break OutHitResult. Now, pull from OutHit bBlockingHit and search for Branch.
