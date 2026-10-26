```
If imageColumn.CommandName = "Edit" Then
    Dim formatString As String = EditUrl("ItemId", "KEYFIELD", "Edit")
    formatString = formatString.Replace("KEYFIELD", "{0}")
    imageColumn.NavigateURLFormatString = formatString
End If

End If

Next

End Sub
```

26. Next, we need to connect the delete icon that appears in the Datagrid to a handler routine that will delete the employee record. All we need to do is extract the ItemId value from the event we receive, convert it to integer using Int32, and pass it to the DeleteEmployee routine created by the Starter Kit. Scroll down and put this procedure just after the Page_Load procedure:

```
Private Sub grdResults_DeleteCommand(ByVal source As Object, ByVal e As System.Web.UI.WebControls.DataGridCommandEventArgs) Handles grdResults.DeleteCommand
    Try
        Dim objEmployeeController As New EmployeeController
        objEmployeeController.DeleteEmployee(ModuleId, Int32.Parse(e.CommandArgument.ToString()))
        'redisplay results in the datagrid
        BindData()
    Catch exc As Exception
        ProcessModuleLoadException(Me, exc)
    End Try
End Sub
```

27. Lastly, we want to trap when the filter selections change. Add these two new procedures at the bottom of the Event Handlers region:

```
Private Sub ddlManagerNo_SelectedIndexChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles ddlManagerNo.SelectedIndexChanged
    BindData()
End Sub
```