```
If colEmployees.Count > 0 Then
    ' add a node to the parent that was passed

    For intEmp = 0 To colEmployees.Count - 1
        objEmployeeInfo = CType(colEmployees(intEmp), EmployeeInfo)
        objChildNode = New TreeNode(objEmployeeInfo.EmpLastName +
                                    ", " + objEmployeeInfo.EmpFirstName)
        objChildNode.ImageIndex = 1 ' employee image
        objParentNode.TreeNodes.Add(objChildNode)
    Next intEmp

End If

End Sub

#End Region
```

+   9. What we are doing here is calling a new `GetManagersDR` function for the first level of the tree, then calling the `GetEmployeesByFilter` function from *Chapter 6* to get the list of employees for each manager.

+   10. Next, we need to call these new procedures when the page loads, so scroll down to the `Page_Load` procedure and add the following code inside the `IsPostBack` check:

```
If Page.IsPostBack = False Then

    If Not Request.QueryString("PageRecords") Is Nothing Then
        ddlRecordsPerPage.SelectedValue =
                                    Request.QueryString("PageRecords")
    End If

    BindManagerDropDown()
    BindSalaryDropDown()

    BindData()

    PopulateTreeControl()
End If
```

+   11. Next, open the `EmployeeController.vb` file.