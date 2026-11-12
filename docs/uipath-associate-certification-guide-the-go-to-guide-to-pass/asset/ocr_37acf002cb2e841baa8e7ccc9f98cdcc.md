+   12. Drag and drop another **Build Data Table** activity below the previous activity.

+   13. Configure the activity as you did previously to build the following structure. Note that the `DepartmentID` column in this data table is **auto-incrementing**. Populate and configure the data table, as shown in the following figure:

![](bbox=[0.225, 0.195, 0.77, 0.63])

Figure 9.9 – Building the Department data table

+   14. Configure the **Output Data Table** property of the activity and assign it to a new data table variable named `DepartmentDT`. As of now, we have two data tables with data. Let's do some manipulations on the data.

+   15. Let's remove some unwanted columns from the **DepartmentDT** data table. We have two ID columns. We will remove **DepartmentCode** from the data table.

+   16. Let's use the **Remove Data Column** activity below the two activities: