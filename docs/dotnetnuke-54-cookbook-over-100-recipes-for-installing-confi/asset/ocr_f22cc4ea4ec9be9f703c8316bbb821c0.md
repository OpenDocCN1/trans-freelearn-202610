```
Dim dr As IDataReader =
    DataProvider.Instance.GetEmployeesByFilter
    (ModuleId, MgrFilter, SalaryFilter, PageIndex, PageSize)
Dim retValue As ArrayList = Nothing
Try
    While dr.Read
        TotalRecords = Convert.ToInt32(dr("TotalRecords"))
    End While
    dr.NextResult()
    retValue = CBO.FillCollection(dr, GetType(EmployeeInfo))
Finally
    If Not dr Is Nothing Then
        dr.Close()
    End If
End Try
Return retValue

End Function
```

18. We're using the Try/Finally structure to trap any errors that might occur from the stored procedure. If there's a problem, the Finally clause will close the data reader for a clean exit.

19. Next, double-click to open the 01.00.00.SqlDataProvider file.

> If you have already installed this module once, you can create a new file called 01.00.01.SqlDataProvider to hold the new procedure. For details see the recipe Deploying a new module version in Chapter 11.

20. To update the stored procedure we need to take the original GetEmployeesByFilter procedure from the last recipe and add the code that will provide the paging. It works by saving the filtered results into a temporary table, giving each record an index value, then only returning the records that fall in the range for the selected page. For example, if we are showing ten records per page (PageSize=10) and we are looking at page three (which has a PageIndex of 2), the procedure should return records 21-30. Look at the stored procedure code and add the lines highlighted below:

```
CREATE procedure {databaseOwner}
    {objectQualifier}ACME_GetEmployeesByFilter
    @ModuleID int,
    @MgrFilter int,
    @SalaryFilter int,
    @PageIndex int,
    @PageSize int
AS
```