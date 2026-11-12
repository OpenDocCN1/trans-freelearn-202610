+   Read Row: Similar to **Read Column**, but this activity reads the content of a row, starting with the cell identifier provided by the user. The extracted data of the row is stored in an `IEnumerable<object >` type variable.

+   Append Range: Enables the easy addition of content in a data table variable to the end of a specified spreadsheet without overwriting data. Furthermore, if the specified sheet is not available in the Excel file, the sheet is created automatically.

+   Get Table Range: Works especially with the tables we have created in the Excel file. This activity enables the user to locate the specified table in a selected spreadsheet and extract the range of the table. This activity’s output can be fed into other activities such as **Read Range** to extract the data from the range returned from the **Get Table Range** activity.

Apart from the common activities described, there are many other activities that UiPath offers to interact with Excel files through **Excel Application Scope**.

# Excel Application Scope

**Excel Application Scope** is a container that holds all the Excel app integration category activities. The **Excel Application Scope** activity triggers the Excel file to open to perform the actions specified within the activity scope. You now have a general idea about the commonly available activities for both categories. However, Excel App Integration offers more functionalities apart from what we discussed earlier. There are many other functions that we can perform using the app integration approach. These actions can be categorized based on the different aspects of Excel functions.

# CSV activities

Excel app integration offers the capability to interact with CSV files using data table variables easily. Although CSV activities are placed under the Excel app integration category, the main difference between CSV and Excel app integration activities is that they can still work without **Excel Application Scope**. UiPath offers three unique activities to interact with CSV files:

+   Read CSV: Reads all the records from a CSV file and stores them in a data table variable.

+   Write CSV: Writes the content of a data table variable into a CSV file. The activity is also capable of creating the CSV file if the file is not available.

+   Append to CSV: Like **Append Range**, the **Append to CSV** activity adds the content of a data table variable at the end of an existing CSV file without overwriting the data. However, the activity also can create the file if it does not exist.