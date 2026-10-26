# Data Entry Tricks

+   11. Finally, scroll down to the bottom of the Event Handlers region and add this code to trap when the drop-down list is changed:
+   
+   ```vb
+   Private Sub ddlRecordsPerPage_SelectedIndexChanged
+           (ByVal sender As System.Object,
+            ByVal e As System.EventArgs) Handles
+                    ddlRecordsPerPage.SelectedIndexChanged
+       PageIndex = 1
+       BindData()
+   End Sub
+   ```

+   12. Double-click to open the `DataProvider.vb` file.

+   13. Replace `GetEmployeesByFilter` with this new version that includes paging arguments:
+   
+   ```vb
+   Public MustOverride Function GetEmployeesByFilter
+           (ByVal ModuleId As Integer,
+            ByVal MgrFilter As Integer,
+            ByVal SalaryFilter As Integer,
+            ByVal PageIndex As Integer,
+            ByVal PageSize As Integer,
+                    As IDataReader
+   ```

+   14. Double-click to open the `SqlDataProvider.vb` file.

+   15. Replace `GetEmployeesByFilter` with this new version:
+   
+   ```vb
+   Public Overrides Function GetEmployeesByFilter
+           (ByVal ModuleId As Integer,
+            ByVal MgrFilter As Integer,
+            ByVal SalaryFilter As Integer,
+            ByVal PageIndex As Integer,
+            ByVal PageSize As Integer)
+                    As IDataReader
+       Return CType(SqlHelper.ExecuteReader
+           (ConnectionString, GetFullyQualifiedName
+           ("GetEmployeesByFilter"), ModuleId,
+            MgrFilter, SalaryFilter, PageIndex,
+            PageSize), IDataReader)
+   End Function
+   ```

+   16. Double-click to open the `EmployeeController.vb` file.

+   17. As we have added new arguments for the `GetEmployeesByFilter` routine, change the code as follows:
+   
+   ```vb
+   Public Function GetEmployeesByFilter(ByVal ModuleId As Integer,
+                                         ByVal MgrFilter As Integer,
+                                         ByVal SalaryFilter As Integer,
+                                         ByVal PageIndex As Integer,
+                                         ByVal PageSize As Integer,
+                                         ByRef TotalRecords As Integer) As ArrayList
+   ```