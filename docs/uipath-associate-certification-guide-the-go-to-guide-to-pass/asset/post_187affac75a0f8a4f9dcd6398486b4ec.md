
## Interacting with Excel files

Many business users use Excel to carry out their daily activities, such as performing financial calculations and maintaining expense records. Excel is one of the most widely used applications in all the departments of any organization. UiPath offers a fantastic set of activities that enables seamless integration and interaction with Excel files with or without Excel installed on your computer. The Excel dependency is added to the new solutions you create in Studio by default. However, if you specifically need to add it to your solution, you can look for UiPath.Excel.Activities from the Dependency Manager.

UiPath offers two different ways of interacting with Excel files:

- File access level: We call these **workbook activities**, which are part of Excel activities. Workbook activities do not require the Microsoft Office package to be installed on your computer. These activities work in the background and are faster compared to actual Excel integration. These activities only work for .XLSX files.

- Excel app integration: We call these **Excel activities**, which allow us to directly integrate with excel. Unlike the workbook activities, Excel activities require the Microsoft Office package to be installed on a computer. The use of these activities will open the Excel application, just as a human user would do before interacting with it. Excel activities enable the user to interact with CSV, XLS, XLSB, and XLSM files. Furthermore, all Excel activities can be configured to be visible or hidden when executing.

Regardless of the method used to interact with Excel files (the access level), both approaches share common actions. However, though the action is common, there are different activities to perform for each type. For example, you can find activities that can read an excel file under both Excel and Workbook activities. These are located in two separate sections in the **Activities** panel, as shown in *Figure 9.12*.

The following screenshot shows the **Read Range** activity, which is used to read data from Excel files. As shown in Figure 9.15, all CSV and Excel activities are located in the **App Integration** category, whereas workbook activities are located in the **System** -> **File** -> **Workbook** category. The main difference between the two types is that activities under the **App Integration** category require the **Excel Application Scope** activity to hold all the other activities, whereas the others do not. The following figure shows the two activities located separately under the **Workbook** and **Excel** categories:

![](img/84b06ad2832c53c5f677f8ce4c80f184_284_0.png)

Figure 9.15 – The Excel and workbook activities location

Let's explore the common activities that are available for both methods:

- **Read Range**: Reads data from a specified range in an Excel file into a data table variable. Using the **Read Range** activity with **Excel Application Scope** enables the user to read the data based on the filters applied in the Excel file. Alternatively, the workbook activities read all the data available in the sheet irrespective of filters.

- **Write Range**: Provides the ability to write data in a data table variable to a spreadsheet. The writing starts from the cell indicated by the user.

- **Read Cell**: Reads the content of a specified cell into a string variable.

- **Write Cell**: Writes to the content of a variable into a specified cell in the specified worksheet.

- **Read Cell Formula**: Enables the user to read the available formula in a specified cell and store it in a string variable.

- **Read Column**: Enables the user to read the content of a specified column in the Excel file. The extracted content is stored in an `IEnumerable<object>` type variable.

- Read Row: Similar to **Read Column**, but this activity reads the content of a row, starting with the cell identifier provided by the user. The extracted data of the row is stored in an `IEnumerable<object>` type variable.

- Append Range: Enables the easy addition of content in a data table variable to the end of a specified spreadsheet without overwriting data. Furthermore, if the specified sheet is not available in the Excel file, the sheet is created automatically.

- Get Table Range: Works especially with the tables we have created in the Excel file. This activity enables the user to locate the specified table in a selected spreadsheet and extract the range of the table. This activity’s output can be fed into other activities such as **Read Range** to extract the data from the range returned from the **Get Table Range** activity.

Apart from the common activities described, there are many other activities that UiPath offers to interact with Excel files through **Excel Application Scope**.

## Excel Application Scope

**Excel Application Scope** is a container that holds all the Excel app integration category activities. The **Excel Application Scope** activity triggers the Excel file to open to perform the actions specified within the activity scope. You now have a general idea about the commonly available activities for both categories. However, Excel App Integration offers more functionalities apart from what we discussed earlier. There are many other functions that we can perform using the app integration approach. These actions can be categorized based on the different aspects of Excel functions.

## CSV activities

Excel app integration offers the capability to interact with CSV files using data table variables easily. Although CSV activities are placed under the Excel app integration category, the main difference between CSV and Excel app integration activities is that they can still work without **Excel Application Scope**. UiPath offers three unique activities to interact with CSV files:

- Read CSV: Reads all the records from a CSV file and stores them in a data table variable.

- Write CSV: Writes the content of a data table variable into a CSV file. The activity is also capable of creating the CSV file if the file is not available.

- Append to CSV: Like **Append Range**, the **Append to CSV** activity adds the content of a data table variable at the end of an existing CSV file without overwriting the data. However, the activity also can create the file if it does not exist.

## Range activities

The activities that fall under this subcategory can easily interact with specified ranges in Excel. These activities are similar to the activities we came across when learning data table activities. However, these activities directly perform the same function in the Excel file itself:

- Select Range: Enables the user to select a specific range in an Excel file. This action is similar to the function that the user performs to highlight a particular range in an Excel spreadsheet.

- Delete Range: Enables the user to delete a specific range in an Excel spreadsheet.

- Get Selected Range: Returns the given range as a string that can be used in other activities that work with ranges.

- Insert Column: Like Add Data Column, this activity directly adds a new table column at a particular position in the Excel file itself.

- Delete Column: Removes the specified Excel table column from the spreadsheet. The deletion takes place based on the column name.

- Insert/Delete Columns: This activity can either add blank columns or remove existing columns from a spreadsheet.

- Insert/Delete Rows: This activity can either add blank rows or remove existing rows, based on the action type the user selects.

- Auto Fill Range: Like auto-population in Excel, this activity will apply the given formula over a specified range in a spreadsheet.

- Copy Paste Range: Enables the user to copy and paste an entire range from one spreadsheet to another. The copied content can include values, formatting, and formulas.

- Lookup Range: Similar to the Lookup function in data tables and Excel, the Lookup Range activity can search for a value in all cells in a given range.

- Remove Duplicate Range: Deletes all duplicate rows in the specified range.

## Excel table activities

The table activities interact with the tables you have created in Excel spreadsheets. The activities allow you to perform the basic functions related to sorting, filtering, and creating tables. The following activities are available to complete the previously mentioned functions on tables:

- Create Table: Creates the specified table in the specified range.
