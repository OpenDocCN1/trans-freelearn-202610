18. Right-click on the variable and select **Add Watch**.

![](bbox=[0.294, 0.115, 0.706, 0.318])

19. The variable and its current value will appear in the **Watch** window.

![](bbox=[0.19, 0.378, 0.811, 0.501])

20. To continue running the program, select **Continue** from the **Debug** menu (or press F5 again).

21. To stop debugging, select **Stop Debugging** from the **Debug** menu (or press Shift+F5).

# Creating a Manifest from an installed module

In the previous recipe we saw how to modify the manifest file created by the Starter Kit as part of deploying a module. One shortcoming of that approach is that the manifest created by the Starter Kit is not always meant for the latest version of DNN. This is done so that starter modules are usable on the widest range of DNN versions. But if you know that your module will only deploy to the latest version of DNN, you can generate a new, up-to-date manifest file from an installed module.

# Getting ready

To follow along with this recipe you must have completed the previous recipe:

+   - *Deploying a module as a standalone package*