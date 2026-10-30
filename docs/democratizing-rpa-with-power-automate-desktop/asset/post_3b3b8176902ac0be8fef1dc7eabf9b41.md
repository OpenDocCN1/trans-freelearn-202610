
For example, you might want to do some housekeeping at the end of a flow and delete files or close a program. But, if you are still in the middle of the design, you could disable these actions so that you can concentrate more on the design. This is also helpful when debugging a flow (which we will do in the next section). But be careful not to disable steps that are part of loops or conditionals. These actions encapsulate one or more other actions with a beginning and an ending. The following picture shows an if conditional action and another if process action. The corresponding end statement for the latter condition is disabled, which leads to an error.

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_77_0.png)

Figure 3.3 – Disabling an action and error message

Delete: Instead of disabling an action, it could, of course, be deleted, which removes the action and all the parameter settings from the flow.

## The parameters dialog

As soon as we drop an action from the **Actions** pane onto the workspace, a dialog pops up requesting us to provide information – the parameters dialog. The following screenshot shows the parameters dialog of the **Get files in folder** action:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_78_0.png)

Figure 3.4 – The parameters dialog

This dialog shows the specific parameters for the chosen action. Since almost every action is different, the corresponding parameters dialog will also show different parameters. There are indeed actions that have similar parameters, for example, **Copy file(s)** and **Move file(s)**, but these are more or less the exceptions.

The parameters dialog lets you enter all the necessary information that the action needs to be executed. This information can be text or numbers, the selection of a drop-down menu, a toggle switch such as the one in the previous screenshot (**Include subfolders**), and sometimes even more complex information.

Sometimes, the parameters are also divided into the most used and advanced ones. These are hidden when the dialog first appears but can be revealed by expanding the **Advanced** area. We can also see this area in the preceding screenshot.

### Important Note

The parameters dialog does not show the mandatory fields required for the action to work. The only way to find out whether a parameter is mandatory is to leave it blank and save the action. Missing information will be displayed as an error in a message window at the bottom of the window. The UI flow will not run until these errors have been corrected.

Actions can also produce or capture information that could be used in some of the next steps in the flow. For example, in *Figure 3.4*, the **Get files in folder** action creates a list of files in the given folder. To make this information available for subsequent actions, variables are produced to store this. In the preceding example, a variable called **Files** is created by the action. The following screenshot shows that this section of the dialog can also be expanded:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_79_0.png)

Figure 3.5 – Expanded variables area

When expanded, it is possible to rename the variable accordingly. Here, we renamed the variable to `MySpecialVariable`. There are naming conventions which we will discuss in the next chapter in more detail. If the data of a variable will not be used in the flow, it is possible to disable the creation of the variable by pressing the corresponding toggle switch.

After an action has been provided with all the required information, it can be saved. Variables that are produced by the action will then be displayed in the **Flow variables** area on the right side. And now, these variables are also available to be used as input for another action. To use a variable as input,

we don’t hardcode information into the parameters input box, but instead, use the {x} symbol on the right. Check out Chapter 5 for more information on variables and to get all the details.

Now that we know the details of the designer window, let’s take a look at the additional functionality provided by the global menu options.

## Global menu options

We already touched upon the menu bar in the previous chapter. These functions can be very valuable when we are editing a flow. Let’s take a look at the most important menu options:

- **File | Save / Save As...**: Save a flow or create a copy by using the Save As... option and provide a new name for the flow.
- **Edit | Undo / Redo**: These are well-known options that allow you to undo or redo any action in the designer. This is also very useful if you accidentally delete an action.
- **Edit | Select all / Clear selection / Invert selection**: One or more actions in the workspace can be *selected*. If you click on one action, it becomes selected. Holding down the shift key and selecting another action will result in all the actions in between will also be selected. This group of actions can now be deleted, activated or deactivated, or even moved together to some other position in the workspace. These menu options deal with this selection.
- **Edit | Go to line**: Sometimes it is nice to be able to jump to a specific line immediately, for example, when working with large flows and many actions or in case of an error because the error message also contains the line number.
- **Debug | Toggle breakpoint / Remove all breakpoints**: When an action is selected, this menu option will place a breakpoint on that line. Another way to do this is by clicking directly on the left side of the line number. We will discuss breakpoints more in the next section.

If the flow is stopped, all execution results of the actions before that breakpoint will be preserved. As seen in Figure 3.6, the breakpoint is after the **Launch Outlook** action. If we stop the flow execution now, Outlook would still be open. This behavior is also helpful when following a phased approach for flow design (see the next section). Breakpoints can also be very useful in controlling the values of the variables. We will make use of breakpoints in the next section.

Another option to pause a flow is to use the *pause* button, which becomes visible when the flow is running. In fact, the *play* button changes to a *pause* button, as you can see in the following screenshot:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_82_0.png)

Figure 3.7 – The pause button

This also allows you to pause the flow anywhere without using a breakpoint.

## Working with subflows

Within each flow, we can also create subflows. This is a concept to structure a larger UI flow into different pieces, which in turn can then be called and referenced in the main flow:

### Main flow

Large flow with loops

### Main flow

Starting actions

Loop in subflow

Housekeeping and end of the flow

Figure 3.8 – Deconstructing a flow

The main flow containing the overall logic of a task then becomes much more readable and easier to understand. All variables are available in the main flow and can be changed, no passing parameters must be specified. It is even possible to call different subflow from within a subflow if you want to structure the work even further. The following figure shows the main flow from our previous example and two subflows:

### Main Flow

### HandlWordDocument Subflow

### WriteToExcel Subflow

### Figure 3.9 – Main flow and subflows

A subflow is stored within the flow itself and can only be used in the containing flow. If you look at the **Actions** pane, you can also see that there is a **Run flow** actions group with a **Run desktop flow** action in it. This additional option will be discussed in *Chapter 4*.

To create a subflow, click on the **Subflows** drop-down menu at top of the workspace, as shown in the following screenshot. In the drop-down menu, you can also see any already existing subflows:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_84_0.png)

### Figure 3.9 – Creating a subflow

The new subflow appears as an additional tab next to the main flow and it starts with a blank workspace to drag actions into. Another option is to cut and paste one or more actions from the main flow over to the subflow. Within the main flow, you can now use the **Flow control | Run subflow** action to specify the subflow to run in the corresponding parameters dialog.

Now that we’ve learned some ways to edit flows, let’s explore ways to use those skills to effectively create them.
