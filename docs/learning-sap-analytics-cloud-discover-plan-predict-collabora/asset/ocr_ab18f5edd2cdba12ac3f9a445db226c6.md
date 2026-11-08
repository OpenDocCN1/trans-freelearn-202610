# Copying a model

The copy option in the model's toolbar section is a very handy option to create a new model based on an existing model. It is illustrated in the following screenshot.

![](bbox=[0.4, 0.2, 0.6, 0.24])

When you use this option to copy a model, the data and private dimensions are also copied to the new model. Public dimensions are referenced from the copied model. Note that you can use this option only for the models created in SAP Analytics Cloud version 2016.21 or later. Further, note that if a model uses a pool dimension (which is used for allocation), you cannot use the copy utility. Execute the following steps to make a copy of an existing model:

+   1. Go to main menu | Browse | Models.
+   2. Put a check mark in front of the model you want to copy. If the selected model is allowed to be copied, the Copy icon will be enabled.
+   3. Click on the Copy icon.
+   4. Enter a name for the new model and provide a description, which is optional.
+   5. Click on OK to make the copy.