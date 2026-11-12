Follow these steps to create the variables:

+   1. Open the `BP_EnemyCharacter` Blueprint.
+   2. In the **Variables** category of the **My Blueprint** panel, click the + button to add a variable and name it `PatrolPoint1`.
+   3. In the **Details** panel, click the **Variable Type** drop-down menu and search for `Actor`. Hover over **Actor** to display a submenu, and then choose **Object Reference**. Check the **Instance Editable** attribute:

![](bbox=[0.125, 0.3, 0.853, 0.645])

Figure 9.27 – Creating a variable that references an Actor instance

+   4. Follow the same steps to create a second **Actor** variable called `PatrolPoint2`.
+   5. Create another **Actor** variable called `CurrentPatrolPoint`. This time, leave the **Instance Editable** attribute unchecked.
+   6. These are the variables we created. The open eye icon means that the variable is **Instance Editable**, so the references of **PatrolPoint1** and **PatrolPoint2** variables will be set in the Level Editor: