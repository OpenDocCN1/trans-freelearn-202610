Variables always have a specific type, such as **text** or **number**. This determines what kind of data can be stored in that variable. There are multiple types available, and we will dig deeper into this in the subsequent section.

We can distinguish between variables that we create on our own (input and output) or those that will be created by actions (flow variables). Each of these variables will show up in the corresponding area. Let’s take a look at each of them.

# ## Flow variables

This group of variables is produced by a flow action so that it can be used later in the flow. Let’s take a look:

+   - The **Run application** action (the `System` group) starts a given application and creates a variable called `AppProcessId` containing the Windows process identifier for that application. We could use `Terminate process` with this variable to shut down the application.
+   - The **Get files in folder** action (the `Folder` group) creates a variable that contains the list of files that reside in that folder. This list can be used to iterate through and read or modify these files.
+   - The **Display message** action (the `Message boxes` group) shows a message box for confirmation (such as **Ok** or **Cancel**) and therefore creates a variable that contains the text of the button that was pressed.

Flow variables cannot be set directly in the designer. However, they can be assigned a value by using the `Set variable` action. Depending on the action, a different variable with a different type will be created. In the preceding list, the first variable would be of the `number` type, while the second one would be of the `list` type. This type cannot be changed, so we must take these variables as they are.

Let’s create a new flow and just drag the **Display message** action into the workspace. In the upcoming dialog, take a look at the lower part in the