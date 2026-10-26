7. Add an expression that invokes the EmployeeDetailFunction function in a Table.AddColumn function, and then expand the column to expose the three columns returned from the EmployeeDetailFunction function using the Table.ExpandTableColumn function. The following is the complete EmployeeIDLookup query:

```
let
    Source = EmployeeKeysAdHoc,
    CallFunction =
        Table.AddColumn(
            Source,
            "FunctionTbl", each
            EmployeeDetailFunction([EmployeeAlternateKey])
        ),
    Expand =
        Table.ExpandTableColumn(
            CallFunction,
            "FunctionTbl",
            {"EmployeeName", "DepartmentName", "EmailAddress"},
            {"EmployeeName", "DepartmentName", "EmailAddress"}
        )
    in
    Expand
```

8. If prompted about data privacy, click the Continue button, set both data sources to have a privacy level of Organizational, and then click the Save button.

9. Click on Close & Apply in the ribbon of the Home tab and save your work.

# How it works

In this example, a business team maintains a list of employee IDs in an Excel workbook and wants the ability to access several columns from the employee dimension table in the semantic model related to these IDs. In the EmployeeKeysAdHoc query, three M transformation functions are applied to protect the integration process: a data type conversion to text, the removal of any null values, and the removal of any duplicates. Data cleansing operations are always recommended when importing from files and unstructured data sources.

In the EmployeeDetailFunction function, the EmployeeID parameter is first defined as a required text-type input parameter. The parameter is then used in the EmployeeDimFilter expression as part of a Table.SelectRows filtering function. Given that the Employees table has type 2 slowly changing dimension logic applied, with multiple rows possible per employee, it is necessary to filter for the current employee row according to the EmployeeDimFilter variable expression ([End Date = null]). Setting this filter condition ensures that only the current or “active” row (no end date) for the employee is returned.