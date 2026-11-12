# Technical requirements

Before starting this chapter, please have UiPath Studio and Robot installed.

Later in the chapter, we will go through a process that causes runtime exceptions and fix it with the help of the Debug option in UiPath Studio.

You can download the process from the GitHub repository: https://github.com/PacktPublishing/UiPath-Associate-Certification-Guide.git.

# Understanding the debugging actions

Debugging a single workflow or the entire project can be performed from the DESIGN and DEBUG ribbon bars. Clicking on the Debug options from the ribbon bar opens up the Debug panel. The following screenshot shows the DEBUG ribbon bar in UiPath Studio:

![](bbox=[0.13, 0.38, 0.87, 0.45])

Figure 13.1 – The DEBUG ribbon bar

The Debug tabs allow the user to perform debugging of a single file or the whole project. The ribbon consists of several debugging actions. Let's understand each one of them in detail.

# Debug File

Clicking on Debug File will start the debugging process for that particular workflow.

# Stop

Clicking on the Stop button ends the debugging process irrespective of which activity is executing. The keyboard shortcut for the Stop button is F12.

# Step Into

When you want to debug one activity at a time, select the Step Into option from the debug ribbon bar. When the Step Into action is triggered, the debugger opens and highlights activities in the workflow container, such as the Flowchart or Sequence activity. Clicking Step Into once will highlight the activity to be executed. Clicking on Step Into one more time will execute the highlighted activity and then highlight the next activity.