create a new flow and define a simple variable, FileToDelete, of the text type and a default value of C:\deletethis.txt.

Next, locate the Delete file(s) action in the File action group and drag it in onto the workspace. The action parameter dialog appears and we can put in the variable name as the file to delete. But before we press OK now, we would want to use the On error functionality to handle the error. If we press the corresponding button in the dialog, it should look like this (I expanded the dialog a bit) on the left side:

![](bbox=[0.125, 0.3, 0.87, 0.52])

Figure 4.16 – On error dialog

There are two main options we can choose here:

+   - **Throw error** (right side): This is the default setting and will stop the flow from running.
+   - **Continue flow run** (left side): If we click on this option, the dialog changes to the one on the left side in the preceding screenshot. Now we can decide what should happen if an error occurs.

When we choose to continue with the flow run, we can determine with the drop-down box below this option what should happen. Here, we have the following options: