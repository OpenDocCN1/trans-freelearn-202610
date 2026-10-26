23. Next, scroll down to the Page_Load procedure and add the BindData() procedure call:

```
If Page.IsPostBack = False Then

    BindManagerDropDown()
    BindSalaryDropDown()

    BindData()

End If
```

24. The Page_Init routine occurs just before the Page_Load routine and is frequently used to initialize controls before the Page_Load. In this case, we are formatting the Datagrid before we populate it with data. We do this by creating a loop that checks each column of the grid and looks for two things:

+   - If the column is an image with the name of "Delete" then we pull a delete confirmation message from the system resource file.
+   - If the column is Edit (like the little edit pencil DNN shows next to a record) then we construct a link to the EditEmployee form of our module using the ItemID from the record.

25. Scroll up a little bit to the Event Handlers region and add this procedure at the top:

```
Private Sub Page_Init(ByVal sender As System.Object,
    ByVal e As System.EventArgs)
        Handles MyBase.Init

    ' Format the columns of the grid control

    For Each column As DataGridColumn In grdResults.Columns

        ' if the column is the edit or delete icon, build the URL
            for the icon
        If column.GetType Is GetType(ImageCommandColumn) Then
            'if it is the delete icon, load the confirmation
                message from the resource file
            Dim imageColumn As ImageCommandColumn =
                CType(column, ImageCommandColumn)
            If imageColumn.CommandName = "Delete" Then
                imageColumn.OnClickJS =
                    Localization.GetString("DeleteItem")
            End If

            'Create a URL to pass the selected ItemId to the
                EditEmployee form
```