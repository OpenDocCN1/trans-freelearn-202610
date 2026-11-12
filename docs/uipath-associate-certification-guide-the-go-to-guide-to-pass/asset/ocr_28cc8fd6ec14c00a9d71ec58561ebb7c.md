+   - **Filter Table**: Filters a table on an Excel spreadsheet based on the conditions provided. The **Filter Table** activity can be used with **Read Range** when you want to read filtered results. The filtering action is identical to the filter function in Excel. Once the filter is applied, the rows that do not qualify will be hidden in the spreadsheet.

+   - **Sort Table**: Sorts a specified table based on the values of a given column in ascending or descending order.

# ## Excel file activities

The file activities are mainly associated with the file itself:

+   - **Save Workbook**: Saves a specified workbook. The **Excel Application Scope** activity automatically saves the file before exiting if the **Save Changes** option is enabled in the **Properties** panel. However, this activity can also be used to save the file.

+   - **Close Workbook**: Enables the user to close a specific Excel spreadsheet.

# ## Cell color activities

Preparing Excel files often requires a little bit of coloring to highlight specific values. The activities under the cell color subcategory enable the user to perform coloring actions as follows:

+   - **Get Cell Color**: Returns the background color of a specific cell in an Excel file and stores it in a `Color` variable.

+   - **Set Range Color**: Enable the user to change the background of a specified cell range to the user-specified color. The input is a `Color` variable type.

# ## Sheet activities

The sheet activities provide the ability to perform different actions related to Excel worksheets, such as copying and retrieving the sheet information. The following activities enable you to work with Excel sheets:

+   - **Copy Sheet**: This enables the user to create a copy of a sheet in the same Excel file or a different file.

+   - **Get Workbook Sheet**: Retrieves the name of the sheet by its index.

+   - **Get Workbook Sheets**: Retrieves all the available sheets in a workbook file as a `String` array.