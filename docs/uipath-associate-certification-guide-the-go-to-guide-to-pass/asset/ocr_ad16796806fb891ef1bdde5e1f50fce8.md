Let's now practice some simple list manipulation methods. The steps outlined next provide a guideline to perform the required actions.

# Declaring list variables

In this section of the chapter, we will take a look at how to declare list variables through the Variables panel.

Open UiPath Studio and create a new Process activity, as follows:

+   1. Open the Main.xaml file.
+   2. Drag and drop a Sequence activity from the Activities panel into the Designer panel.
+   3. Select the Sequence activity and click on the Variables panel.
+   4. Create a new list variable named StudentsList with the type List<String>. The list data type will not show in the dropdown initially. Hence, click on the Browse for Types option in the dropdown, and look for System.Collections.Generic.List in the Browse and Select a .Net Type pop-up window.
+   5. Select List<T> under System.Collections.Generic, select the data type from the dropdown as String, and click on the OK button to create a variable. The following screenshot shows the configuration in the Browse and Select a .Net Type pop-up window:

![](bbox=[0.303, 0.552, 0.692, 0.745])

Figure 7.10 – List variable creation