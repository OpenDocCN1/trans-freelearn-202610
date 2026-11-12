unchecked.

These are the actions of the **Resume** button. Now, let's add the actions of the **Reset All** button.

# # Resetting the save file

The **Reset All** button will delete the save game if it exists and then reload the game level. We will create a macro with the actions to delete the save game.

Follow these steps to create the macro:

+   1. In the **My Blueprint** panel, click the + button in the **Macros** category to create a macro. Change the name of the macro to `DeleteFile`. In the **Details** panel of the macro, create an input parameter named `In` of the **Exec** type and an output parameter named `Out` of the **Exec** type.

+   2. On the tab created for the **DeleteFile** macro, add the nodes seen in the following screenshot. We get a reference to the **FirstPersonCharacter** instance to retrieve the value of the **Save Slot Name** variable.

![](bbox=[0.126, 0.58, 0.864, 0.823])

Figure 11.25 – DeleteFile macro actions