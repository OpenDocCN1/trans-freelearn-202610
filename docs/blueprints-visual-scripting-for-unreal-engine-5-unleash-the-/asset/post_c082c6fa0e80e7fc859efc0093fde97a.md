
![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_476_0.png)

Figure 13.57 – An example of a Do N node

The player can press **Space Bar** to fire a special weapon. After the third shot, they need to press the *R* key to reset the **Do N** node to be able to fire three more times.

## Gate

The **Gate** node has an internal state; it can be opened or closed. If it is open, then the output pin will execute when the **Gate** node is triggered. If it is closed, then the output pin will not execute.

These are the input pins of the **Gate** node:

- **Enter**: An execution pin that receives the flow of execution
- **Open**: An execution pin that sets the state of **Gate** to open
- **Close**: An execution pin that sets the state of **Gate** to closed
- **Toggle**: An execution pin that toggles the state of the **Gate** node
- **Start Closed**: A Boolean variable that determines whether the **Gate** node should start in the closed state

The example in the next screenshot is of a Blueprint that applies damage to actors that are overlapping it:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_477_0.png)

Figure 13.58 – An example of the Gate node

When an actor is overlapping, the Gate node stays open, and damage is applied to the actor on every tick. If the actor stops overlapping, then the Gate node will close, and the damage will no longer be applied.

## MultiGate

When a MultiGate node is triggered, one of the output pins is executed. A MultiGate node can have multiple output pins. To add another output pin, use the Add pin + option.

These are the input pins of the MultiGate node:

- Reset: An execution pin used to reset the MultiGate node and allow new executions of the output pins.
- Is Random: A Boolean variable. If the value is True, then the order of execution of the output pins is random.
- Loop: This is a Boolean variable that indicates how the MultiGate node will behave after the last output pin is executed. If the value is True, then the MultiGate node continues to execute the output pins. If False, then the MultiGate node will stop executing the output pins.
- Start Index: An integer value indicating the first output pin to be executed.

The following screenshot shows an example of the MultiGate node:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_478_0.png)

Figure 13.59 – An example of the MultiGate node

When the Tab key is pressed, a MultiGate node is used to set a different Static Mesh at each execution. The Loop parameter is checked, so MultiGate will continue executing from the first output pin after the last output pin is executed.

These are the flow control nodes available in Blueprints. It's good to know about their existence because we can find problems that can be easily solved using one of them.

## Summary

In this chapter, we learned how to use data structures to organize data in Blueprints. We learned how to store various elements in an array and how to retrieve any of those elements. We learned how to use other types of containers, such as sets and maps, to store data.

After that, we learned how to create and use enumerations, structures, and data tables, and we saw examples of how they can be related. It was presented to us as several flow control nodes, such as Switch, Gate, and For Each Loop.

This chapter showed various Blueprint features that will help us to organize data so that it can be used effectively. The flow control nodes can simplify the EventGraph because, for each situation, there may be a more suitable node.

In the next chapter, we will learn about world and local coordinates, vector operations, and the use of traces to test collisions.

### Quiz

- 1. Arrays do not allow repeated elements.
- a. True
- b. False

- 2. For each enumeration type, there is a Switch On node available in Blueprints.
- a. True
- b. False

- 3. All variables in a structure must be of the same type.
- a. True
- b. False

- 4. A data table uses a structure to define the data type of the table.
- a. True
- b. False

- 5. Which node is not a flow control node?
- a. For Each Loop
- b. Spawn Actor from Class
- c. Do Once
- d. Gate

## Chapter 14: Math and Trace Nodes

The representation of a 3D world is based on mathematical concepts. If you do not understand these concepts, then it will be more difficult to understand certain operations performed in a 3D game. This chapter explains some math concepts that are needed for 3D games. We will learn about the difference between world and relative transforms and how to use them when working with components. We will learn how to use vectors to represent the position, direction, velocity, and distance. The concept of traces is explained and various types of traces are presented. This chapter also shows how to use traces to test collisions in a game.

These are the topics covered in this chapter:

- World and relative transforms
- Points and vectors
- Introduction to traces and trace functions

By the end of the chapter, you will know how to use vectors and traces to make the player interact with other actors in the level.

### World and relative transforms

The `Actor` class has a **Transform** structure. This structure has three variables, which are used to represent **Location**, **Rotation**, and **Scale**. The **Transform** structure of an actor that is on the Level can be modified by using the **Details** panel.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_482_0.png)

Figure 14.1 – Modifying the transform in the Details panel

In the Level Editor, you can use the transformation widget that appears when you select an actor. There are buttons to select the type of transformation the widget will apply to an actor, as we can see in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_482_1.png)

Figure 14.2 – Using the widget to modify the transform

The 3D space is represented by three axes: x, y, and z. These axes are represented by colors: red is the x axis, green is the y axis, and blue is the z axis.

The **Location** variable of the **Transform** structure has a set of values for X, Y, and Z, which determines the position on each axis. These values are also known as the world location of the actor. The following screenshot shows some actions that we can use to get and set an actor's location:

- **GetActorLocation**: Returns the current location of the actor
- **SetActorLocation**: Sets **New Location** for the actor
- **AddActorWorldOffset**: Uses the **Delta Location** input parameter to modify the current location of the actor.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_483_0.png)

Figure 14.3 – Getting and setting the actor location

The **Rotation** variable of the **Transform** structure has a set of values for X, Y, and Z in degrees, which determines the rotation on each axis. The following screenshot shows the following rotation nodes:

- **GetActorRotation**: Returns the current rotation of the actor
- **SetActorRotation**: Sets **New Rotation** for the actor
- **AddActorWorldRotation**: Adds the **Delta Rotation** input parameter to the current rotation of the actor.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_484_0.png)

Figure 14.4 – Getting and setting the actor rotation

The Scale variable of the Transform structure has a set of values for X, Y, and Z, which determines the scale on each axis. The following screenshot shows the nodes that are used to get and set an actor's scale:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_484_1.png)

Figure 14.5 – Getting and setting the actor scale

When a Blueprint has Actor components, the transforms of those components are known as relative transforms because they are relative to the component's parent. The following screenshot shows an example of components. DefaultSceneRoot is a small white sphere that is hidden in the game and used to store the actor's position in the world. It can be replaced with another scene component.

Below it, in the component's hierarchy shown in the screenshot, there is a Static Mesh component named Table, and below the Table component in the hierarchy, there is another Static Mesh component named Statue. The transform of Table is relative to the DefaultSceneRoot transform, and the transform of the Statue component is relative to the Table transform. So, if
