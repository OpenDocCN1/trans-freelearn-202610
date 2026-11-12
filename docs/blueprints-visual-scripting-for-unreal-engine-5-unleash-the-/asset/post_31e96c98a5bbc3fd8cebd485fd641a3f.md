
Figure 15.6 – Promoting the return value to a variable

If you need to break all the connections of a pin, hold the *Alt* key and click on the pin. You can move all the connections of a pin to another compatible pin by holding the *Ctrl* key, dragging the connections, and dropping them on another pin. This is very useful because you don't need to redo the connections one by one. In the following screenshot, all the connections of the **As First Person Character** pin will be moved to the **As BP Player Character**.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_515_0.png)

Figure 15.7 – Dragging all connections to another pin

The Blueprint Editor offers several options for node alignment. To use them, select some Blueprint nodes and right-click on one of them to open a menu. **Alignment** is one of the options on this menu. The following screenshot shows the **Alignment** options available:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_516_0.png)

Figure 15.8 – The Alignment options

Most of the Alignment options are self-explanatory, but let's look at an example: Straighten Connection(s). The following screenshot shows three nodes selected:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_516_1.png)

Figure 15.9 – These nodes will be aligned

After applying Straighten Connection(s), the nodes will be aligned, as shown in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_517_0.png)

Figure 15.10 – The nodes after applying Straighten Connection(s)

There are shortcut keys to create some common nodes in Blueprints. If you want to create a **Branch** node, then hold the *B* key and left-click on the graph. To create a **Sequence** node, hold the *S* key and left-click on the graph, as shown in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_517_1.png)

Figure 15.11 – Shortcuts for Branch and Sequence nodes

Other shortcut keys are *F + left-click*, which creates a **For Each Loop** node, and *D + left-click*, which creates a **Delay** node. These can be seen in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_518_0.png)

Figure 15.12 – Shortcuts for the For Each Loop and Delay nodes

To create a comment box around some nodes, first select the nodes, then right-click on one of the selected nodes and select the **Create Comment** option from **Selection**, or you can just press the C key. The following screenshot shows a comment box that is labeled **More shortcut keys**. Inside the comment box, there are more examples of shortcut keys used to create flow control nodes.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_518_1.png)

Figure 15.13 – Shortcuts for some flow control nodes

When you get used to some shortcuts, you will see how they speed up your work. Now, let's look at tips to help you build better Blueprints.

## Blueprint best practices

In a project, you will deal with several Blueprint classes, and some of these Blueprint classes will be complex, with many nodes. The tips in this section will help you analyze your project and carry out some practices that will make your Blueprint classes more manageable. I separated these tips into two categories: Blueprint responsibilities and Blueprint complexities.

### Blueprint responsibilities

When creating a Blueprint, you need to decide what its responsibilities will be. This refers to what it will do and what it will not do. You need to make the Blueprint as independent as possible. A Blueprint must be responsible for its internal state.

To illustrate the concept of Blueprint responsibilities, let's work with a simple example created for teaching purposes. In a game, the player is represented by the **FirstPersonCharacter** Blueprint. If the player collides with an enemy Blueprint, then the player will die, and an explosion effect will be spawned. The following screenshot shows the event hit that was implemented in the enemy Blueprint:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_519_0.png)

Figure 15.14 – Event hit of an enemy Blueprint

After that, you'll create another Blueprint that can also kill the player. So, you copy **Event Hit** and the nodes of the previous screenshot and paste them into the new Blueprint. Then, you create another different type of enemy Blueprint and copy and paste **Event Hit** again. But you decide to change the way the player dies. The player does not explode anymore; instead, a death animation is executed. However, to make this change in your game, you will have to search for all Blueprints that can kill the player and modify the script of all of them. This is a problem because you might forget one of the Blueprints, and the script may have frequent changes.

There is a way to avoid this type of problem. The script that defines how player death works must be implemented in the player Blueprint, which, in this example, is the **FirstPersonCharacter** Blueprint. The point is that the player Blueprint is responsible for the way the player dies. Let's redo our example, but now, we will create a custom event named **Death** in the **FirstPersonCharacter** Blueprint, as shown in this screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_520_0.png)

Figure 15.15 – Creating the Death event in the FirstPersonCharacter Blueprint

This way, if there are changes in the way the player dies, then these changes will need to be done only in the **Death** event of the **FirstPersonCharacter** Blueprint.

When a collision occurs, the other Blueprints that can kill the player will just have to trigger the **Death** event of the **FirstPersonCharacter** Blueprint. The following screenshot shows the new version of **Event Hit** of the **Enemy** Blueprint:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_521_0.png)

Figure 15.16 – New version of Event Hit of Enemy Blueprint

So, you can use events and functions to define how a Blueprint can communicate with other Blueprints. If you need to send data between Blueprints, then it can be sent through input or output parameters.

Another topic related to Blueprint responsibilities is Level Blueprint. Each Level has a Level Blueprint, so if you create your game rules logic inside a Level Blueprint, then when you add another Level, you will need to copy and paste all Blueprint nodes to the Level Blueprint of the new Level. If your game rules logic changes, then you will need to modify all the Level Blueprints, and this can become a maintenance nightmare.

A Level Blueprint must be used only for logic and situations specific to one Level. A typical example is to put a hidden trigger on a Level. When the player overlaps it, an enemy is spawned in another room.

A better place to implement game rules logic is in a GameMode Blueprint class. The logic for other actors should be implemented in Blueprint classes rather than being implemented in the Level Blueprint because instances of a Blueprint class can be added to any Level, so you do not need to copy and paste Blueprint nodes to use the same functionality in another Level.

### Managing Blueprint complexities

A Blueprint **EventGraph** can become very complex and scary. When you open a Blueprint of this kind that was done by someone else, you might wonder, *what's going on?*

Some practices and Blueprint tools will help you to manage the complexities of a Blueprint and keep it readable.

The most important concept that will help you deal with complex Blueprints is **abstraction**. Abstraction is used to handle complexities by hiding low-level details, allowing the developer to focus on a problem at a high abstraction level without worrying about unnecessary details of other parts of the script.

In an **EventGraph**, there is a simple way to apply abstraction. You can select a group of nodes and convert them into a collapsed graph, Function, or Macro. To convert the nodes, right-click on the selected nodes. In the submenu that appears, within the **ORGANIZATION** category, you will see the options shown in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_522_0.png)

Figure 15.17 – Collapse options

Let's look at an example. The following screenshot shows some nodes connected to the **InputAction** Pause event. These nodes are responsible for showing the Pause Menu:
