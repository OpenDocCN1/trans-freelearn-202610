
![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_352_0.png)

Figure 10.1 – Creating the TargetActorKey variable

- 6. Create another variable in My Blueprint panel. In the Details panel, name the variable `Damage`, change Variable Type to Float, and check the Instance Editable attribute. Compile the blueprint and set DEFAULT VALUE to `0.25`, which means an attack will take 25% of the player's health:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_352_1.png)

Figure 10.2 – The Damage variable has a default value of 0.25

- 7. Hover the mouse on the **Functions** category of the **My Blueprint** panel, click the **Override** dropdown, and select **Receive Execute** to add the event.

- 8. In **Event Receive Execute**, we will ensure **Target Actor** is valid and then apply damage to it. The event gets the **Target Actor** reference from the blackboard and checks whether it is valid before calling the **Apply Damage** function:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_353_0.png)

Figure 10.3 – The Event Receive Execute actions

- 9. The nodes of steps 9–12 are to get the **Target Actor** reference from the blackboard and check whether it is valid. Drag a wire from the white pin of **Event Receive Execute** and add an **Is Valid** macro node.

- 10. Drag the **Target Actor Key** variable from the **My Blueprint** panel, drop in **EventGraph**, and select **Get Target Actor Key**.

- 11. Drag a wire from the **Target Actor Key** node and add **Get Blackboard Value as Actor**.

- 12. Drag a wire from **Return Value** of **Get Blackboard Value as Actor** and connect to the **Input Object** pin of the **Is Valid** node.

- 13. The nodes of steps 13–15 are to call the **Apply Damage** function using the **Target Actor** reference. Drag another wire from **Return Value** of **Get Blackboard Value as Actor** and add an **Apply Damage** node. Connect the **Is Valid** output pin to the white input pin of the **Apply Damage** node.

- 14. Drag a wire from the **Base Damage** input pin of **Apply Damage** and add a **Get Damage** node.

- 15. Drag a wire from the white output pin of Apply Damage and add a Finish Execute node. Check the Success parameter of the Finish Execute node.

- 16. Compile, save, and close the Blueprint Editor.

Now, we need to add our attack task to the attack sequence in BT_EnemyBehavior.

## Using the attack task in the Behavior Tree

The attack of the enemy is a melee attack, so the enemy will only perform the attack after reaching the player.

Follow these steps to use the DoAttack task:

- 1. In the Behavior Tree Editor, drag a wire down from the Attack Player sequence node and add a BTTask_DoAttack task node between the Move To and Wait task nodes.

- 2. In the Details panel, change the Target Actor Key selection to PlayerCharacter. Change Node Name to Damage Player to describe how we are using BTTask_DoAttack:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_355_0.png)

Figure 10.4 – The BTTask_DoAttack properties

- 3. Save the Behavior Tree. The Attack Player sequence should look like this:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_355_1.png)

Figure 10.5 – The Attack Player sequence with the BTTask_DoAttack node

The **Attack Player** sequence is complete. Now, we need to update the `FirstPersonCharacter` blueprint to decrease the health meter when damage is dealt out.

## Updating the health meter

The health meter bar is linked to the **Player Health** variable of the `FirstPersonCharacter` blueprint. We will use **Event AnyDamage** to decrease the value of the **Player Health** variable.

These are the steps to create **Event AnyDamage**:

- 1. In the content browser, access the `Content/FirstPersonBP/Blueprints` folder and double-click on the `FirstPersonCharacter` blueprint.

- 2. Right-click on the empty space of **Event Graph** and add the **Event AnyDamage** node. We will add these nodes to the Event:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_356_0.png)

Figure 10.6 – The Event AnyDamage actions

- 3. Drag a wire from the white output pin of the **Event AnyDamage** node and add a **SET Player Health** node.

- 4. Drag a wire from the input pin of SET Player Health and add a MAX (float) node. We use this node to ensure that Player Health will never be less than 0.0.

- 5. Drag a wire from the top input pin of the Max (float) node and create a Subtract node.

- 6. Drag a wire from the top input pin of the Subtract node and add a GET Player Health node.

- 7. Drag a wire from the bottom input pin of the Subtract node and connect to the Damage output pin of Event AnyDamage.

- 8. Compile, save the blueprint, and press the Play button to test.

Note that the player's health meter depletes when an enemy gets within range of the player and attacks.

Now that our enemy is attacking the player, we want to give more options on how the enemy can detect the player.

## Making enemies hear and investigate sounds

Enemies that can only pursue players who walk directly in front of them can easily be avoided. To address this, we will take advantage of the PawnSensing component to have the enemy detect nearby sounds that the player makes. If the player makes a sound within the detection range of an enemy, then the enemy will walk to the location of that sound to investigate. If they catch the player in their sight, they will try to attack; otherwise, they will wait at the location of the sound for a moment before returning to their patrol.

## Adding hearing to the Behavior Tree

We will add a sequence of tasks that occur when the enemy hears a sound. We want the enemy to continue attacking the player once they see them, so investigating a sound has a lower priority on the Behavior Tree.

To have the enemy investigate the point where it heard a sound, we will need to create two keys within the blackboard. The `HasHeardSound` key is of the Boolean type and will be used to store whether a sound has been heard. The `LocationOfSound` key is of the `Vector` type and will be used to store the location that the sound came from – hence, the location that the enemy AI should investigate.

Follow these steps to create the blackboard keys and add the **Investigate Sound** sequence node to the Behavior Tree:

- 1. In the content browser, access the `Content/FirstPersonBP/Enemy` folder and double-click on the `BT_EnemyBehavior` asset to open the Behavior Tree Editor.

- 2. Click the **Blackboard** tab:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_358_0.png)

Figure 10.7 – Switching the Behavior Tree Editor to Blackboard mode

- 3. Click the **New Key** button and select **Bool** as the key type. Name this new key `HasHeardSound`:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_359_0.png)

Figure 10.8 – The HasHeardSound Bool key

- 4. Click the New Key button again and select Vector as key type. Name this new key LocationOfSound:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_359_1.png)

Figure 10.9 – The LocationOfSound Vector key

- 5. Save the blackboard and click the Behavior Tree tab.

- 6. Move the Attack Player sequence and all its task nodes further to the left in the Behavior Tree, leaving room between Attack Player and Move to Patrol. This is where we will add our hearing sequence.

- 7. Drag a wire down from the **Selector** node and add a **Sequence** node. Rename this node `Investigate Sound`.

- 8. Right-click on the **Investigate Sound** node, hover over **Add Decorator** to expand the menu, and select **Blackboard** to add a decorator.

- 9. Click on the decorator, and in the **Details** panel, set **Observer aborts** to **Lower Priority**, **Blackboard Key** to **HasHeardSound**, and **Node Name** to **Heard Sound?**:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_360_0.png)

Figure 10.10 – The decorator settings

The following screenshot shows the **Investigate Sound** sequence node with the decorator. Note that the priority of nodes in a Behavior Tree is left to right:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_361_0.png)

Figure 10.11 – The Investigate Sound sequence node

We have created the blackboard keys, the sequence node, and the decorator. Our next step is to create the task nodes of the **Investigate Sound** sequence.

### Setting up the investigating tasks

The **Investigate Sound** sequence will be like the **Attack Player** sequence. If an enemy is patrolling and hears a sound, they will move to the location of the sound:
