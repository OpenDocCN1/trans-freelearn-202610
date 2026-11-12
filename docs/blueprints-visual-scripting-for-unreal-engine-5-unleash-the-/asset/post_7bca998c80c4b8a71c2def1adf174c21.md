
variable is referencing an instance. Connect the nodes, as shown in the following screenshot. Compile this Blueprint:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_106_0.png)

### Figure 4.6 – Actions of Event ActorBeginOverlap

### Important Note

Always use the Is Valid macro before executing a function using an object reference variable. The variable can be invalid for several reasons. Executing a function using an invalid variable will cause an error at runtime.

- 12. In the Level Editor, click the **Create** button located on the toolbar. Hover over **Lights** to display a submenu and drag **Point Light** and drop it somewhere in the Level to create an instance:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_106_1.png)

### Figure 4.7 – Creating a Point Light

13. In the **Details** panel, click on the name of the **Point Light** instance and change the name to `Lamp`. Set the **Mobility** attribute to **Movable** to be able to change the light properties at runtime.

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_107_0.png)

### Figure 4.8 – Details panel of the Point Light instance

14. Drag the `BP_LightSwitch` Blueprint class from **Content Browser** and drop it in the Level in a place near the **Point Light** instance that we added to the Level. The next screenshot shows the **Details** panel of `BP_LightSwitch`. The `Light` variable appears in the **Details** panel because we checked the **Instance Editable** attribute. Click the dropdown menu of the `Light` variable to show all **Point Light** instances that are in the Level and select the **Point Light** instance that we renamed to `Lamp` in the preceding step. Essentially, this is Direct Blueprint Communication. `BP_LightSwitch` has an object reference to another Actor/Blueprint and can call its Actions:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_108_0.png)

### Figure 4.9 – Referencing an instance of the Level

15. Click the **Play** button to see the `BP_LightSwitch` Blueprint in action. Every time your character overlaps the instance of `BP_LightSwitch`, it toggles the visibility of the selected **Point Light**. The following screenshot shows an example using the **Third Person** template. The **Point Light** variable is on the wall, and the `BP_LightSwitch` Blueprint is on the floor:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_109_0.png)

### Figure 4.10 – Touching the BP_LightSwitch to turn on the light

In this section, we learned how to create a variable that refers to an instance of another Blueprint. But sometimes we need to access attributes of the subclass of the instance being referenced. In this case, we need to cast the reference.

## Casting in Blueprints

There is a node named Cast To that tries to convert reference variable types to new specified types. To understand casting, it is necessary to remember the concept of inheritance between classes, which we covered in *Chapter 3, Actors and the Gameplay Framework*.

The following diagram represents a Blueprint called `BP_GameModeWithScore`. **Game Mode Base** is the parent class of this Blueprint. Based on the inheritance concept, we can use a variable of the **Game Mode Base** object reference type to reference an instance of `BP_GameModeWithScore`. However, this variable is unable to access the variables and functions of a *subclass* like those defined in the `BP_GameModeWithScore` Blueprint, because a **Game Mode Base** reference only knows the variables and functions that are defined in the **Game Mode Base** class:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_110_0.png)

### Figure 4.11 – BP_GameModeWithScore inherits from Game Mode Base

Therefore, if we have a **Game Mode Base** object reference, we can try to cast this reference using the `Cast To BP_GameModeWithScore` function. If the instance is of the `BP_GameModeWithScore` type, then **Cast To** will succeed and return a `BP_GameModeWithScore` object reference that we can use to access the variables and functions of `BP_GameModeWithScore`.

Another use of the **Cast To** node is to safely test whether an object reference is of a desired type and this step-by-step example will illustrate both use cases:

- 1. Create or use an existing project, based on the Third Person template, with the starter content.
- 2. Click the **Add** button in **Content Browser** and choose the **Blueprint Class** option.
- 3. On the next screen, choose **Game Mode Base** as the parent class.
- 4. Name the Blueprint `BP_GameModeWithScore` and double-click it to open the Blueprint Editor.
- 5. In the **My Blueprint** panel, create a variable named `GameScore` of type integer and create a function named `AddGameScore`:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_112_0.png)

### Figure 4.12 – Creating a variable and a function

6. In the **Details** panel of the `AddGameScore` function, add an **Input Parameter** named `Score` of type integer. This function is used to add points to the `GameScore` variable:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_113_0.png)

### Figure 4.13 – Adding an input parameter

- 7. In the graph of the function, add the **Actions** shown in the following screenshot. To add the **GET** and **SET** nodes of the **Game Score** variable, simply drag the variable, drop it into the graph, and choose either **GET** or **SET**. The **Print String** function is used to display the current value of the **Game Score** variable on the screen:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_113_1.png)

### Figure 4.14 – The Actions of the Add Game Score function

### Important Note

The node that is between **SET** and **Print String** is a converter. To create it, simply connect the **SET** output parameter to the **In String** input parameter of **Print String**. Because the parameters are of different types, the converter is created automatically.

- 8. Compile and save the `BP_GameModeWithScore` Blueprint. The next step is to set the Level to use `BP_GameModeWithScore` as **Game Mode**.

- 9. In the Level Editor, click the **Settings** button located on the right of the toolbar and choose **World Settings**:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_114_0.png)

### Figure 4.15 – Accessing the World Settings

- 10. In the **GameMode Override** attribute, click the drop-down menu and choose `BP_GameModeWithScore`, as shown in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_115_0.png)

### Figure 4.16 – Changing the Game Mode used by the Level

- 11. Create a Blueprint and use Actor as the parent class. Name it as BP_Collectable and open it in the Blueprint Editor.

- 12. Click the Add button in the Components panel and choose the Static Mesh Component. In the Details panel, choose the SM_Statue Static Mesh and in Materials, go to Element 0 and choose M_Metal_Gold. Also, change Collision Presets to OverlapAllDynamic, as shown in the following screenshot:

| 属性组 | 具体设置 |
|--------|----------|
| STATIC MESH | Static Mesh: SM_Statue |
| MATERIALS | Element 0: M_Metal_Gold<br>Element 1: M_Statue |
| COMPONENT TICK | — |
| PHYSICS | — |
| COLLISION | Simulation Generates Hit Events<br>Phys Material Override: None<br>Generate Overlap Events<br>Can Character Step Up On: Yes<br>Collision Presets: OverlapAllDynamic |

### Figure 4.17 – Setting up the Static Mesh

- 13. Right-click **Event Graph** and add **Event ActorBeginOverlap**. **Other Actor** is the instance that overlaps the `BP_Collectable` Blueprint. Drag from the blue pin of **Other Actor** and drop in the graph to open **Context Menu**.

- 14. Choose the **Cast To ThirdPersonCharacter** action, as shown in the following screenshot. **ThirdPersonCharacter** is the Blueprint that represents the player in the Third Person template. We are using the **Cast To** action to test whether the instance referenced by **Other Actor** is the player:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_117_0.png)

### Figure 4.18 – Casting the Other Actor reference

- 15. Right-click **Event Graph** and add the **Get Game Mode** function. Drag from the blue pin of **Return Value** and drop it in the graph to open **Context Menu**. Choose the **Cast To BP_GameModeWithScore** action.

- 16. Drag from the blue pin of **As BP Game Mode With Score**, drop it in the graph, and choose the **Add Game Score** action in the **Context Menu**. Type `50` in the **Score** input parameter.

- 17. Right-click **Event Graph** and add the **DestroyActor** function. Connect the white pins of the nodes. The content of **Event ActorBeginOverlap** is shown in the following screenshot:
