
![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_624_0.png)

Figure 18.31 – The first part of the IncreaseExperience function

- 16. The next screenshot shows the second part of the IncreaseExperience function. If the Yes pin of the Xp Reaches New Level macro is executed, then we increment the Current Level variable by one and set the Level Up Var variable to True. Add the nodes of the screenshot and compile the Blueprint:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_624_1.png)

Figure 18.32 – The second part of the IncreaseExperience function

- 17. Compile the Blueprint and close the Blueprint Editor.

We have finished the BP_ExpLevelComp component. To test it, we need to add the component to an Actor.

## Testing the Actor Component

We will add the BP_ExpLevelComp component to ThirdPersonCharacter and use a timer to increase the experience points every second. We will print a message on screen every time ThirdPersonCharacter levels up.

Follow these steps to test the `BP_ExpLevelComp` component:

- 1. Open the `ThirdPersonCharacter` Blueprint located in the `Content > ThirdPersonBP > Blueprints` folder.

- 2. In the **Components** panel, click the ADD button and search for `level`. Select **BP Exp Level Comp**:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_625_0.png)

Figure 18.33 – Adding BP Exp Level Comp

- 3. In the **Details** panel, you can modify the default values of the **Exp Level** array or add more array elements:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_626_0.png)

Figure 18.34 – The Exp Level array is editable

- 4. In the EventGraph, add the Event BeginPlay node and the Set Timer by Event node. We will use the timer to call an event once a second, which will increase the experience points. Set the Time parameter to 1.0 and check the Looping parameter:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_627_0.png)

Figure 18.35 – Creating a timer to run an event once a second

- 5. Drag a wire from the Event pin of the Set Timer by Event node and select Add Custom Event in the Context Menu. Rename the custom event GainXP. This event will run the Increase Experience function of the BP Exp Level Comp component, passing a value of 100 as the input parameter. These are the nodes we will add to the GainXP event:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_627_1.png)

Figure 18.36 – The nodes of the GainXP custom event

- 6. Drag BP Exp Level Comp from the Components panel and drop it in the EventGraph to create a Get node.

- 7. Drag a wire from the Get BP Exp Level Comp node and add the Increase Experience function node. Type 100 in the Experience Points parameter.

- 8. Add a **Branch** node to check whether the Actor has leveled up. Add the **Print String** and **Append** nodes to the EventGraph and connect them as shown in *Figure 18.36*. The conversion node is created automatically when you connect pins of different types.

- 9. In the **A** pin of the **Append** node, type `Leveled up` to with a space at the end. This node will create a `String` using the values of the **A** and **B** pins.

- 10. Compile the Blueprint and click the **Play** button of the Level Editor. A message will be printed on screen every time `ThirdPersonCharacter` levels up:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_628_0.png)

Figure 18.37 – Printing a message on screen when the character levels up

If you need to display the experience points and the level of the character on screen, you can get the values by using the **Get Current XP** and **Get Current Level** nodes of the **BP Exp Level Comp** component:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_628_1.png)

Figure 18.38 – Getting the Current XP and Current Level variables

We've learned how to create an Actor Component. Our next step is to create a Scene Component to attach other components to it.

## Creating Scene Components

We will create a Scene Component named BP_CircularMovComp that rotates around the Actor. Then, we will attach a Static Mesh Component to the BP_CircularMovComp component to simulate a rotating shield.

Follow these steps to create our Scene Component:

- 1. Access the Chapter18 folder that we created in the first example of this chapter.

- 2. Click the Add button in the content browser and choose the Blueprint Class option.

- 3. On the next screen, choose Scene Component as the parent class. Name the Blueprint BP_CircularMovComp and double-click it to open the Blueprint Editor.

- 4. In the My Blueprint panel, create the RotationPerSecond and DeltaAngle variables of the Float type:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_629_0.png)

Figure 18.39 – Creating the variables of the component

- 5. Compile the Blueprint and select the RotationPerSecond variable. In the Details panel, set DEFAULT VALUE to 180.0. This value is in degrees, so the component will complete a rotation around the Actor in 2 seconds:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_629_1.png)

Figure 18.40 – Setting the default value of the Rotation Per Second variable

- 6. We will use Event Tick to create a smooth movement for the component. Event Tick is executed every frame. It has the Delta Seconds parameter, which stores the time elapsed since the last execution of Event Tick. We will multiply Delta Seconds by Rotation Per Second to find DeltaAngle, which is the value of the angle that we will rotate in the current frame. Add the nodes of the screenshot in the EventGraph:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_630_0.png)

Figure 18.41 – Calculating the value of Delta Angle

- 7. The next screenshot shows the other nodes that we will add to Event Tick. These nodes are used to calculate and set the new relative location of the Scene Component and to modify the local rotation to make the Scene Component always point toward the center of rotation. You need to right-click the Delta Rotation parameter of AddLocalRotation and select Split Struct Pin to access the Delta Rotation Z (Yaw) pin:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_630_1.png)

Figure 18.42 – The other nodes of Event Tick

- 8. Compile the Blueprint. This is all `BP_CircularMovComp` does. Our next step is to add this component to an Actor and then attach a Static Mesh Component to it.

- 9. Open the `ThirdPersonCharacter` Blueprint located in the `Content > ThirdPersonBP > Blueprints` folder.

- 10. In the **Components** panel, select **Capsule Component** to make sure that our Scene Component will be attached to the Root Component. Click the **ADD** button and search for `circular`. Select **BP_CircularMovComp**:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_631_0.png)

Figure 18.43 – Adding BP_CircularMovComp

- 11. In the **Components** panel, select **BP_CircularMovComp** that was added and click the **ADD** button. Select the **StaticMesh** component to attach it to **BP_CircularMovComp**. The movement and rotation applied on **BP_CircularMovComp** will affect **StaticMesh**:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_632_0.png)

Figure 18.44 – Attaching a StaticMesh component to our Scene Component

- 12. Now, we need to configure the components. Access the **Viewport** tab so we can see the result of the next change. Select **BP_CircularMovComp** in the **Components** panel. In the **Details** panel, set the *x* (red) value of **Location** to 70.0. You can modify the value of **Rotation Per Second** in the **DEFAULT** category:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_633_0.png)

Figure 18.45 – Setting the relative Location of BP_CircularMovComp

- 13. Select the StaticMesh component in the Components panel. In the Details panel, select Shape_Cube as the Static Mesh and M_Tech_Hex_Tile_Pulse as the Material. You can view the components in the Viewport tab. Set the z (blue) value of Location to -80.0 and Scale to x = 0.1, y = 1.0, and z = 1.5:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_634_0.png)

Figure 18.46 – Setting up the Static Mesh Component

- 14. Compile the Blueprint and click the **Play** button of the Level Editor. **BP_CircularMovComp** will make the shield rotate around the character:
