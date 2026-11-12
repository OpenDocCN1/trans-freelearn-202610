
![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_466_0.png)

Figure 13.42 – Creating a data table

- 2. After selecting **Data Table**, the Unreal Editor will ask you to choose a structure that represents the data type of the table. Select the **WeaponType** structure:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_466_1.png)

Figure 13.43 – Selecting the structure used by the data table

- 3. Rename the created data table `WeaponTable` and double-click on it to open the Data Table Editor.

- 4. Click on the **Add** button to add a row to the table. Each row has a row name that identifies the row, which must be unique. To change a row name, you need to right-click on a row and select **Rename**. In the example in the following screenshot, the row name is a simple index. Add some rows to the data table, as shown in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_467_0.png)

Figure 13.44 – Editing a data table

The data table can also be imported from a plain text **Comma-Separated Values (CSV)** file. The following screenshot shows an example of a CSV file. A spreadsheet editor can export a spreadsheet to the CSV format:

| | Name | Category | Damage | Fire Rate | Range | Accuracy |
|---|---|---|---|---|---|---|
| 1 | Desert Eagle | Pistol | 30 | 0.200000 | 0.300000 | 0.600000 |
| 2 | M1887 | Shotgun | 60 | 0.300000 | 0.100000 | 0.500000 |
| 3 | Uzi | Submachinegun | 40 | 0.500000 | 0.400000 | 0.700000 |
| 4 | AK-47 | Assault Rifle | 50 | 0.600000 | 0.500000 | 0.800000 |
| 5 | Dragunov SVD | Sniper Rifle | 70 | 0.300000 | 1.000000 | 0.900000 |

Figure 13.45 – WeaponTable in the CSV format

- 5. To import a CSV file, click on the **Import** button in the Content Browser and select the CSV file. A dialog box will appear, asking you to select a structure in the **Choose DataTable Row Type** field, as shown in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_468_0.png)

Figure 13.46 – Importing a data table from a CSV file

- 6. To use a data table in a Blueprint, create a variable in the Blueprint Editor and select **Data Table > Object Reference** as **Variable Type**:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_469_0.png)

Figure 13.47 – Creating a data table reference

- 7. Compile the Blueprint, and in **DEFAULT VALUE**, select one data table, as shown in the following screenshot:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_469_1.png)

Figure 13.48 – Setting the default data table

The following screenshot shows some actions to get data from a data table:

- **Get Data Table Row**: Returns a structure with the data of a specific row
- **Get Data Table Row Names**: Copies all the row names of a data table to an array
- **Get Data Table Column as String**: Copies all the values of a column to an array of strings:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_470_0.png)

Figure 13.49 – Getting content from a data table

The following screenshot shows an example of how a data table is used.
**Select Weapon** is a function that receives **Weapon ID** as an input parameter and searches in **Weapon Table** for a weapon whose row name is equal to **Weapon ID**. If it finds the weapon, then it copies the weapon data to the **Current Weapon** variable:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_470_1.png)

Figure 13.50 – Getting data from a row of a data table

We saw some data structure assets that we can create to organize data in a project. Now, we will learn about more Blueprint nodes that are useful to organize your script.

## Flow control nodes

There are some nodes that control the flow of the execution of a Blueprint. These nodes determine the execution path based on conditions. We've already seen the **Branch** node, which is a commonly used flow control node. Let's learn about the other main types of flow control nodes.

## Switch nodes

A **switch node** determines the flow of execution based on the value of an input variable. There are different types of switch nodes. The next screenshot shows an example of the **Switch on Int** node:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_471_0.png)

Figure 13.51 – An example of the Switch on Int node

The **Selection** input parameter receives an integer value that determines the output pin that will be executed. If the input value has no pin, then the **Default** pin will execute. You can change the start index in the **Details** panel of **Switch on Int**. The output pins are added by using the **Add pin +** option.

Another type of switch is the **Switch on String** node, which is shown in the following screenshot. The output values must be added in the **Details** panel of the **Switch on String** node, under **Pin Options | Pin Names**:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_472_0.png)

Figure 13.52 – An example of the Switch on String node

There is also **Switch on Enum**, which uses the values of an enumeration as the available output pins.

## Flip Flop

Every time a **Flip Flop** node is executed, it toggles between two output pins – **A** and **B**. There is a Boolean output parameter named **Is A**. If the **Is A** parameter is `True`, then it indicates that pin **A** is running. If it is `False`, then pin **B** is running.

Note

The **Flip Flop** node does not work properly inside a function because **Flip Flop** has an internal variable that is deleted when the function ends. So, every time, the **A** pin will be executed inside a function.

The following screenshot shows an example of the **Flip Flop** node:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_473_0.png)

Figure 13.53 – An example of the Flip Flop node

If the player is using dual wield pistols, then when they fire, only one of the pistols fires. In the next shot, the other pistol fires. **Fire Left Pistol** and **Fire Right Pistol** are custom macros, used here to simplify this example.

## Sequence

When a **Sequence** node is triggered, it executes all the Actions connected to the output pins in order. Thus, it executes all the Actions of a pin, then all the Actions of the next pin, and so on. It is useful to organize groups of Actions.

The next screenshot shows the **Sequence** node in use. The **Print String** functions are used to show the order of execution:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_473_1.png)

Figure 13.54 – An example of the Sequence node

The **Add pin +** option adds output pins. To remove a pin, right-click on the pin and choose the **Remove execution pin** option.

## For Each Loop

A **For Each Loop** node receives an array as an input parameter and performs the Actions of the **Loop Body** output pin for each element of the array. The current **Array Element** and **Array Index** are available as output pins. The **Completed** output pin is executed when **For Each Loop** finishes its execution.

In the next screenshot, the **For Each Loop** node is used to iterate through an array of **Point Light** references that contain the lamps of a room:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_474_0.png)

Figure 13.55 – An example of the For Each Loop node

The lamps are turned on by using the **Set Visibility** function of **Point Light Component**, which is inside the **Point Light** array element.

## Do Once

The **Do Once** node executes the output pin only once. If it is triggered again, then its output pin will not be performed. The **Reset** input pin needs to be triggered to allow the **Do Once** node to run the output pin again.

The following screenshot shows an example of how it is used:

![](img/e1fd47ad71d32bbc4bf189d4cf9ed94d_475_0.png)

Figure 13.56 – An example of the Do Once node

When the player presses **Space Bar**, the **Do Once** node is triggered, and a charged weapon is used. After that, a timer is created to execute **Full charge Custom Event**, which resets the **Do Once** node after `30.0` seconds. If the player presses **Space Bar** again before 30 seconds have elapsed, then the **Do Once** node will not execute its output pin.

## Do N

The **Do N** node allows you to specify how many times the output pin can execute. After the number of executions has completed, the Actions of the output pin will only be executed again if the **Reset** pin is triggered.

The following screenshot shows an example of how the **Do N** node is used:
