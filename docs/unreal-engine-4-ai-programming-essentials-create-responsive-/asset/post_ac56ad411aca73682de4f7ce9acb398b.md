
## The navigation cost

As you can see, the AI does exactly as you predict. It avoids the mud because of the cost that is applied as it traverses NavModifierVolume, and sometimes, water's Fixed Area Entering Cost wards off the AI, although more than often, the AI is found traversing the water because once you're in, everything is okay! This was usually the trick for me. So, with this said, I hope you've understood the A* algorithm and its search to find the most optimal path.

## Summary

In this chapter, we covered the different tools that allow us to control and influence the behavior of our AI's movement using NavModifierVolumes and other recast properties. We also learned how to tell our AI's controlled pawn to navigate to the waypoints that belonged to them. Lastly, we covered more fundamentals, such as the pathing algorithm originally created by Edsger Dijkstra. Next, we learned that we can optimize his original algorithm in favor of performance and resources and achieve this through heuristics.

In the next chapter, we will create a behavior tree and adapt what we've learned from the previous chapters to create some interesting AI interactions. We will also take advantage of the sensory system provided in Unreal Engine 4.

## 5 Giving AI Choices

In this chapter, you will learn how to introduce autonomous behavior into your characters using Behavior Tree. Behavior Tree is a methodology that allows you to view your AI logic visually. Behavior Tree is a type of hierarchical task network resulting in a state-oriented design. So, each state will dictate our current task instead of a goal.

This chapter will cover:

- Behavior Tree
- Blackboard
- The components of Behavior Tree, including Selector, Decorator, Service, and so on
- Building a Behavior Tree to run on a dog character

### Behavior Tree in AIController

In this chapter, we will use Behavior Tree and scripts to create our autonomous state-oriented behavior. However, before we get to a higher-level control of our AI, let's understand some of the fundamental components of what allow us to control our AI. So, from the start, we have AIController, which is similar to PlayerController; this controller is responsible for interpreting all our AI input. This input is applied by the world when we request that it is moved.

With this in mind, we can introduce influence through multiple avenues within the code. We could tell AIController to move to a location, or we can tell AIController to run Behavior Tree. What's also very important to understand is that movement is applied with the CharacterMovement component. If you created a subclass from CharacterMovement, you can extend and continue to use the same Behavior Tree to introduce movement in theory. One example of this is the car AI project I created for the Unreal Engine 4 development forums.

The link for the same is https://forums.unrealengine.com/showthread.php?25073-UPDATED-5-16-A-I-Templates-Bot-Car-amp-Flying-AI.

The internal movement component was responsible for taking the Move To request from the Behavior Tree. So, this allowed me to tell my car to go to certain locations, know when it was finished, and simply repeat the process. This is powerful if you’re trying to create AI because a majority of what you need to do is move.

## Creating Behavior Tree

Moving onto Behavior Trees, let’s begin to construct our first tree! In this project, we will create AI that resembles the behavior of a neighborhood dog. It will change its behavior state randomly every so often and continue to search for any mailman, who may loiter by its dog pin.

This will touch on using Environment Query System (EQS), which is still an experimental but powerful feature in Unreal Engine 4. It will be responsible for handing the dog a new position to search for the mailman in. So, if you’re ready, let’s open up a new Third Person project.

Let’s create a few things for our dog to route between. The following are the steps to do so:

- 1. First, let’s make a new custom **Target Point** class and name it Foodbowl.

- 2. Next, let’s make a new custom **Target Point** class and name it DogHouse:

![](img/aaaa027482c900e215ba6bfab3c17337_93_0.png)

- 3. Place these two actors accordingly within the world. We will use these to reference our dog where to move to when in the appropriate state. So, basically, we will use Target Points or waypoints to reference the vector location of this actor within the blueprints later on.

- 4. Now, we need a new Enumeration class called `DogState`; this will hold our current state of the dog. This value is important for executing the correct tree for the appropriate responses we send to the AI, such as detecting the nearby mailman.

- 5. The DogState class will have Hungry, Barking, and Idle as the three available states. Hungry will be responsible for telling the dog to navigate to the dog bowl to eat food. The Barking state will tell the dog to search for the mailman. The Idle state will tell the dog to go to its dog house. At any time, the dog will begin to chase the mailman if he gets too close:

![](img/aaaa027482c900e215ba6bfab3c17337_94_0.png)

### Blackboard

This stores the memory for an individual agent or agents. This works hand in hand with the Behavior Tree. It allows easy and direct access to variables from any of your Task nodes. Imagine that your Behavior Tree is your EventGraph, and Blackboard is your variable that you use in EventGraph. With the Instance Synced function, you can replicate the variable to every instance of the Blackboard class within the world. Now, let's start creating our Blackboard:

- 1. We need to create a new Blackboard (which is under AI assets when you right-click within Content Browser) and name it DogBrain. This will store our State as Enum DogState for the Behavior Tree branches, Mailman as ThirdPersonCharacter if we have one found, Foodbowl as itself if we have found, and lastly our latest search Locations as a vector for EQS. You may see the elements we created as follows:

![](img/aaaa027482c900e215ba6bfab3c17337_95_0.png)

- 2. Let's save this all. We have to create Behavior Tree now that we have the bare components that make up our Behavior Tree. Let's go over these quickly:

  - DogState: This will store our current state because this is a state-oriented tree
  - DogHouse: This will represent where our dog house is located for the dog to sleep
  - Foodbowl: Similar to DogHouse, this will be the location of the food bowl when the dog is hungry

## Designing Behavior Tree

Let's name our Behavior Tree DogTree and make sure our DogBrain Blackboard asset is plugged into the ROOT entry node. The Blackboard asset set here has variables that can be accessed by the functions within the tree during execution. When variables are instanced and synced, you have a global variable for Mailman that all other dogs with the same Blackboard asset can see. Our Behavior Tree will be set up in visually similar way to the following screenshot:

![](img/aaaa027482c900e215ba6bfab3c17337_96_0.png)

What you can note here is that we will start with a **Selector** node. This is because we do not want to exit the tree if our child node fails. If it fails, we want to continue to the next node that will ultimately be succeeded. Within individual trees, we will do something different. Now that we are in a state, we sometimes want to control the sequence of events in order to replicate a particular behavior for this state. So, for our `Hungry` state, we will first need to have a bowl. Once we have our bowl, we can go eat. We will also include a waiting node, and this makes our dog seem like it's searching for its dog bowl. This happens only the first time. Next time the condition check to the bowl being set will be `true`, and we will immediately move to our `Foodbowl`.

In our `Barking` state, we will immediately go into a **Selector** node because we want to again execute whichever tasks there are and wait till one succeeds. This means the task in which the condition is met returning successful and resulting in failure for a **Selector Composite** node. For this, we need to execute a few functions, and all must return success.

This is important to understand because this basic logic is what allows you to construct a desired execution behavior in Behavior Tree. So, in the following breakdown, I will outline the process in the order of execution and how the construction of the tree depends on the direction of the flow of execution:
