
The data table variables are used quite often to handle manipulations on structured data.
By now, you are familiar with the functions that data tables support and the activities
available in UiPath to perform those functions. The knowledge gained on these functions
and activities comes in handy when working with almost all RPA projects. The following
section provides practical experience in using some of the activities discussed previously.

## Creating and customizing data table variables

Since we have covered the basics of data tables, let's now practice. The following steps
will guide you on how to use some of the activities we discussed earlier. Let's consider a
scenario where you have one data table that consists of employee information and another
with department information.

Let's build a workflow by completing the following steps:

- 1. Open UiPath Studio and create a new **Blank Process** project.

- 2. Open the **Main.xaml** file in the **Designer** panel.

- 3. Search for the **Build Data Table** activity from the **Activities** panel and drag it to the canvas.

- 4. Click on the **DataTable...** button on the activity to open the **Build Data Table** screen. This screen can be used to add columns, rows, and change column properties.

- 5. Remove all columns and rows by clicking on the X mark next to them.

- 6. Click on the + sign to add a new column named **EmployeeID** with the **Int32** data type and the **Auto-Increment** property set to **True**. Configure the **Allow Null** property to **False** and the **Unique** property to **True**, and then click on **OK** to create the new column. This configuration will enable the data table to automatically create a new unique ID for each new row added.

The following figure illustrates the configuration of the `EmployeeID` field in the **Build Data Table** activity:

![](img/84b06ad2832c53c5f677f8ce4c80f184_276_0.png)

Figure 9.6 – Creating columns in the Build Data Table activity

- 7. Create another column with the name `FirstName` with the **String** data type. The remaining properties don't need to be changed.

- 8. As with the `FirstName` column, create another two columns named `LastName` and `City` of the **String** data type, and another column called `DeptID` of the **Int32** data type.

- 9. Populate the table with rows, as shown in Figure 9.7:

![](img/84b06ad2832c53c5f677f8ce4c80f184_277_0.png)

Figure 9.7 – Building the Employee data table

- 10. Click OK and close the configuration window.

- 11. Click on the **Properties** panel and configure the **Output Data Table** argument of the activity by creating a new data table variable and assigning it. You can create the variable through the **Properties** panel by using the **Ctrl + K** shortcut keys. Name the new data table `EmployeeDT`. Once you have configured the property, your configuration should look similar to the one provided in the following figure:

![](img/84b06ad2832c53c5f677f8ce4c80f184_277_1.png)

Figure 9.8 – Configuring the Output property of the Build Data Table activity

- 12. Drag and drop another **Build Data Table** activity below the previous activity.

- 13. Configure the activity as you did previously to build the following structure. Note that the `DepartmentID` column in this data table is **auto-incrementing**. Populate and configure the data table, as shown in the following figure:

![](img/84b06ad2832c53c5f677f8ce4c80f184_278_0.png)

Figure 9.9 – Building the Department data table

- 14. Configure the **Output Data Table** property of the activity and assign it to a new data table variable named `DepartmentDT`. As of now, we have two data tables with data. Let's do some manipulations on the data.

- 15. Let's remove some unwanted columns from the **DepartmentDT** data table. We have two ID columns. We will remove **DepartmentCode** from the data table.

- 16. Let's use the **Remove Data Column** activity below the two activities:

![](img/84b06ad2832c53c5f677f8ce4c80f184_279_0.png)

Figure 9.10 – Adding the Remove Data Column activity

- 17. Configure the DataTable property of the Remove Data Column activity by specifying the DepartmentDT variable. This property specifies which data table is affected by the activity.

- 18. Configure the Column Name property of the activity by specifying the "DepartmentCode" column, which we need to delete:

![](img/84b06ad2832c53c5f677f8ce4c80f184_279_1.png)

Figure 9.11 – Configuring the Remove Data Column activity

- 19. Now, let's add a new row to the Employee table. Use the Add Data Row activity and place it below the last activity.

- 20. Configure the DataTable property of the Add Data Row activity by specifying EmployeeDT, as that is the data table that we need to add the new row to.

- 21. Configure the Array Row property of the activity with the following code:

```
{ Nothing, "Amy", "Woods", "New York", Nothing }
```

The array we define here has to pass values to all the columns. However,
we do not need to assign a value to the **EmployeeID** field because it is an
auto-incrementing field. In that case, we will be passing an empty field defined
by the keyword Nothing. In the same command, we will also be giving a blank
value for **DepartmentID** as well.

- 22. Add a Filter Data Table activity below the **Add Data Row** activity. Let’s filter the
**EmployeeDT** data table by removing employees that do not have a valid **City**:

- 23. Configure the **DataTable** and the **Filtered DataTable** properties with the same
**EmployeeDT** variable. This configuration will overwrite the data in **EmployeeDT**
with the filtered results.

- 24. Click the Configure Filter button on the **Filter Data Table** activity to open
**Filter Wizard**.

- 25. Configure **Filter Wizard**, as shown in the following figure:

![](img/84b06ad2832c53c5f677f8ce4c80f184_280_0.png)

Figure 9.12 – Filtering the data table

- 26. Before we join the two data tables, print the values in the data tables and see
how they look. Use an **Output Data Table** activity, and configure the **DataTable**
property with **EmployeeDT** and the output **Text** property to a new string variable.
Use a **Write Line** activity and configure it with the new string variable you created.

- 27. Run the workflow and check the output of EmployeeDT. You will see the final outcome of the manipulations we performed on the data table. Feel free to use the combination of Output Data Table and Write Line anywhere in the workflow to explore the output after each manipulation action.

- 28. Use a Join Data Tables activity to join the two data tables using the common Department ID. Configure the Join Data Tables activity, as shown in the following figure. Note that the Join Type option is set to an Inner join:

![](img/84b06ad2832c53c5f677f8ce4c80f184_281_0.png)

Figure 9.13 – Configuring the Join Data Tables activity

- 29. Use another combination of the Output Data Table and Write Line activities pointing to JoinDT (the data table that contains the result after joining) and examine the output.

- 30. The following is the output we get for the preceding workflow with the join type as an Inner join:

| EmployeeID | FirstName | LastName | City | DeptID | DepartmentID | DepartmentName |
|------------|-----------|----------|------|--------|--------------|----------------|
| 0 | John | Doe | New York | 1 | 1 | Information Technology |
| 1 | Jack | Robins | Florida | 2 | 2 | Human Resources |
| 2 | Mark | Milos | Texas | 2 | 2 | Human Resources |

Figure 9.14 – The data table output with an inner join

- 31. Configure the join type as Left and Full and explore how the output differs from the one previously.

In addition to the points covered, explore the different operation types available in the Join Wizard. We used the operation = for our matching because we wanted an exact match. However, there are many operations available that may fit into different types of joins based on the scenario. Alternatively, sometimes, you may receive data tables without a proper column name. In scenarios where you do not know the exact column names, you can always refer to the columns using the column index. Similar to the Collection type variables, DataTable also has zero-based indexes for rows and columns. In other words, if you want to access the first row in a data table using an index, it would always be zero. The same concept is applied to accessing columns.

Imagine we want to use the index to configure our Join Wizard instead of the column names. In such a case, we need to use the following approach.

The first data table (employee) has five fields, and Department ID is the last. Considering the zero-based index, Department ID will hold index 4. Regarding the Department data table, you need to use index 0 because the Department ID field is the first column. If you are to configure the Join Wizard with the identified indexes, you use index 4 for the **Column Table 1** field and **index 0** for the **Column Table 2** field.

By now, you should have an understanding of the basic concepts of a data table and how to use it with our activities in UiPath. As you have already seen, there is a wide range of activities in UiPath to perform different manipulations with data tables. Try exploring the other activities on the same example and see what other things you can do. You can expect the same functionality when working with Excel files; therefore, data in Excel files is easy to export into data tables. The next section explains how to interact with Excel files. There are many automation solutions built around Excel data manipulation. It is essential to understand the different methods available to interact with Excel files.
