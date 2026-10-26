6. Edit the Source line of the EmployeeKeysAdHoc query to use the ExcelFilePath query:

```
let
    Source = Excel.Workbook(File.Contents(ExcelFolder & "\" & ExcelFile),
null, true)
in
    Source
```

The path and filename of the Excel file are now controlled by the ExcelFolder and ExcelFile parameters.

# See also

+   - Using custom functions: https://bit.ly/48IMEzm

# Capturing User Selections with Parameter Tables

An alternative method for providing parameter functionality to users of Power BI reports is via dedicated parameter tables. In this approach, the parameter values of a table are either computed during the semantic model refresh process or are loaded as a one-time manual operation. DAX measures reference this parameter table and other tables and expressions of the model to enrich the self-service analysis experience and support Power BI report development. This approach is best suited for parameters that change infrequently since hardcoded parameter tables within Power BI mean that the semantic model owner must make such changes. If parameters need to change frequently, it may be better to have a database table or external file for the parameter table that is easier to edit and change.

The example in this recipe involves providing simple visibility to four alternative scenarios to the baseline annual sales plan—10 and 20 percent above and below the baseline plan. An inline set of scenario values is embedded in the semantic model, and DAX measures are used to capture filter context—such as business user selections—and compute the corresponding scenario logic.

# Getting ready

To prepare for this recipe, follow these steps:

+   1. Download Capturing user selections with parameter tables Start.pbix from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition.
+   2. Open the .pbix file and choose Transform data from the ribbon of the Home tab to open Power Query Editor.
+   3. Edit the AdventureWorks query to point to your copy of the AdventureWorksDW2022 database.

# How to capture user selection with parameter tables

To implement this recipe, perform the following steps:

+   1. In Power BI Desktop, while in the Report view, click on New Table in the ribbon of the Modeling tab.