
- 12. Right-click and search for Apply Damage; then, connect the true exec pin of the Branch node to this node. Pull from OutHit HitActor and put it in the DamagedActor pin of Apply Damage:

![](img/aaaa027482c900e215ba6bfab3c17337_158_0.png)

- 13. Next, right-click and search for Draw Debug Line. We want the LineStart pin to connect to the results from Step 6, which is the beginning of our line. Then, pull OutHit Location, which will be plugged into LineEnd. Select the values for LineColor as red, for Duration as .2, and for Thickness as 3.

- 14. Lastly, pull the local Found Actor variable and set it. Pull from OutHit HitActor and set it in our local variable. Then, send this to Return Node.

- 15. We want to duplicate **Draw Debug Line** to connect to the **false** exec pin of the **Branch** node:

![](img/aaaa027482c900e215ba6bfab3c17337_159_0.png)

- 16. Instead of getting **OutHit Location** for **LineEnd**, we will get the results from Step 7.

- 17. Then, the end of this is plugged into **Return Node**. The **Hit Actor** pin in **Return Node** is populated with our local **Found Actor** variable:

![](img/aaaa027482c900e215ba6bfab3c17337_160_0.png)

## Controllers

In this example, we will give PlayerController the ability to take and apply damage. This is done once the AI is finished, and you'll be able to fight the AI. Also, as we will use AI Perception in this course, we should register the Stimuli source for our controller. Next, we will set up the AIController. We will give it the ability to sense stimuli. Then, we will create an Actor component, which will contain the function needed to update Behavior Tree.

Let's move on and begin to create these base components, as follows:

- 1. In the AI folder, right-click and select **Blueprint**. Then, from the options, select **Player Controller**. Then, we want to name this `OutController`.
- 2. Open **Event Graph** and find **Event BeginPlay**.
- 3. Right-click nearby and search for **Get Controlled Pawn**. Then, pull from **Return Value** and search for **Assign OnTakeAnyDamage**.
- 4. This will create an **OnTakeAnyDamage** event and output **Damage**.
- 5. We need to create a variable to hold our **Health** variable. Click on **Add variable** and make a Float variable named **Health**. Set the default to 5.
- 6. Looking at **OnTakeAnyDamage**, pull out **Damage** and subtract it from **Health**. Then, set the results in our **Health** variable. Next, we will check whether **Health** is less than or equal to 0.0.
- 7. Create a **Branch** node from the results. Pull from **True** and right-click to search for **Print String**. Populate this with **Player Died**, right-click, and then search for **DestroyActor**.
- 8. Right-click and search for **Get Controlled Pawn**; then, plug this into the **Target** pin on **DestroyActor** from the previous step. Now, we can take damage from our AI counterpart:

![](img/aaaa027482c900e215ba6bfab3c17337_161_0.png)

- 9. Right-click and search for the **E Input** event. Pull **Pressed** and release to search for **Sequence**. Pull from **Then 0** and release to search for **DoOnce**. Pull from **Then 1**, then release to search for **Delay**. Now, set **Duration** to 0.75 and plug **Completed** into the **Reset** pin in **DoOnce**.
- 10. Looking at **DoOnce**, pull from **Completed**, and release to search for **LaserfromController**. Then, pull from **Hit Actor** and release to search for **Apply Damage**. Set **Duration** to 1.0 and **Damage Causer** to **Self**.
- 11. Now, we want to add this Controller as **StimuliSource**. In the **Components** section, click on **Add Component** and find **AIPerceptionStimuliSource**.
- 12. Click on **Register as Source for Senses** and then click on **+**. Let's select **AI Sense_Sight** from the available options.

Now, compile this. We are done with our PlayerController setup. Now we need to create an AIController setup so that our AI can fire back at us:

![](img/aaaa027482c900e215ba6bfab3c17337_162_0.png)

Let's set up our AIController as follows:

- 1. Right-click and click on **Blueprint**. In all classes, search for **AI Controller**. Select it and then click on **Select** to create it. Name it `EnemyController` and open it in EventGraph.
- 2. Let's create a new Float variable named **Health**. Then, set the default value to **5.0**.
- 3. The beginning of this is the same on both characters except for one minor change. So, we can copy the code from `OurController` or start from Step 2 in the previous section. We will copy the section shown in the following screenshot into `EnemyController` from `OurController`:

![](img/aaaa027482c900e215ba6bfab3c17337_163_0.png)

- 4. At the end of the **Bind Event to OnTakeAnyDamage** event, pull to release and search for **Run Behavior Tree**. Set the **BTAsset** value to **EnemyAI**, which is the asset we created earlier in the chapter. Now, the code in the tree will run on this **AIController**.
- 5. Now that the AI can receive damage, we need it to sense the player. We will do this with the **AI Perception** component.
- 6. On the **Components** section, click on **Add Component** and search for **AIPerception**. After adding this, under **AI Perception** on the right-hand side, select **Senses Config** from the drop-down menu and hit **+**. Then, set it to **AI Sight config** and select **Sense** from the drop-down menu.
- 7. Drop down **Detection by Affiliation** and check for **Detect Neutrals**.
- 8. Go under **Events** and click on **OnPerceptionUpdated** to get the currently sensed actors.
- 9. From **OnPerceptionUpdated (AIPerception)**, pull and release to search for **Sequence**. We want **Then 0** to be drawn and then released to search for **DoOnce**. Then, we want **Then 1** to be set to the **Duration** pin of the **Delay** node. Set **Duration** to **1**. Now, connect **Completed** to **Reset**.
- 10. Focus on DoOnce. We will pull from Completed and then release to search for Stop Movement. Right-click and search for Blackboard. Pull from Blackboard and release to search for Set Value as Object. Right-click, search for Make Literal Name, and plug this into Key Name. Get the first element from UpdatedActors and plug this into Object Value:

![](img/aaaa027482c900e215ba6bfab3c17337_164_0.png)

Now, the AI will immediately report the actors that get sensed to Behavior Tree, which then can be used immediately in our AI's next decision.

## Waypoints

Similar to the AI Sense project, these waypoints will have a reference to the next waypoint. This simple link between two waypoints allows us to create paths for our AI to navigate to.

Now, we should move on making these waypoints; perform the following steps:

- 1. To begin, let's right-click in the space inside the Folder section and go to Blueprint. At the bottom, search in All Classes for TargetPoint. Select it and then hit Select to create it. Then, name it Waypoint and open this in EventGraph.
- 2. On the left-hand side under My Blueprint, add a variable waypoint named NextWaypoint. We will use this to find the next waypoint to traverse to.
- 3. Add four waypoints to the level and link them together using the NextWaypoint variable. The connection should look similar to A->B->C->D->. D should then connect to A, thus creating a loop.

## BT Composites, Task, Decorator, and Service

Tasks are executed by composites. Composites are important because they directly affect the flow control within your Behavior Tree.

Composites come in three forms at the time of writing this: Sequence, Selector, and Simple Parallel. Here's a description of each:

- **Sequence**: This executes each node, returning success on the last node; however, if any node fails, it will immediately return failure and abort the rest of the leaves.
- **Selectors**: This executes each node, returning success immediately and aborting the rest of the leaves. If a node returns failure, it continues to only return failure if the last child returns failure.
- **Simple Parallel**: This executes one task and a subtree at the same time, which allows you to walk and allow another tree of decision making to be at the top of the walk task, for example.

Tasks are usually the last node in the change to be called as they contain the code that would affect the AI actions directly. We will make our own task and learn how it communicates with the Behavior Tree.

**Decorators** are executed on the entry of a composite or task, which can determine whether the composite or node should be executed. These are great to create custom checks for this specific task. For example, it only executes Break Door if it has the Door and Beast modes.

**Services** are executed while a subtree is active. This means that it has an available tick. This allows us to gather or update information to make immediate changes in game. The shooting service, for example, would be responsible for shooting whenever the player is in line of sight. This also then allows you to govern the shooting and prevent interruptions when you don't want them.

In this section, we will use everything except Simple Parallel. Let's begin!
