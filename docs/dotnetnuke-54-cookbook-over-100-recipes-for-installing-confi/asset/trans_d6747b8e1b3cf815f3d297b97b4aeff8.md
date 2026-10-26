

23. 接下来，滚动到 `Page_Load` 过程并添加 `BindData()` 过程调用：

```vb
If Page.IsPostBack = False Then

    BindManagerDropDown()
    BindSalaryDropDown()

    BindData()

End If
```

24. `Page_Init` 例程在 `Page_Load` 例程之前发生，并且经常用于在 `Page_Load` 之前初始化控件。在本例中，我们在用数据填充之前先格式化 DataGrid。我们通过创建一个循环来检查网格的每一列并查找两件事来实现：

- 如果列是名为 "Delete" 的图像，则我们从系统资源文件中提取删除确认消息。
- 如果列是 Edit（像 DNN 在记录旁边显示的小编辑铅笔）则我们使用记录中的 `ItemID` 构造一个指向模块的 `EditEmployee` 表单的链接。

25. 稍微向上滚动到 Event Handlers 区域并在顶部添加此过程：

```vb
Private Sub Page_Init(ByVal sender As System.Object,
    ByVal e As System.EventArgs) Handles MyBase.Init

    ' Format the columns of the grid control

    For Each column As DataGridColumn In grdResults.Columns

        ' if the column is the edit or delete icon, build the URL for the icon
        If column.GetType Is GetType(ImageCommandColumn) Then
            'if it is the delete icon, load the confirmation message from the resource file
            Dim imageColumn As ImageCommandColumn = CType(column, ImageCommandColumn)
            If imageColumn.CommandName = "Delete" Then
                imageColumn.OnClickJS = Localization.GetString("DeleteItem")
            End If

            'Create a URL to pass the selected ItemId to the EditEmployee form
            If imageColumn.CommandName = "Edit" Then
                Dim formatString As String = EditUrl("ItemId", "KEYFIELD", "Edit")
                formatString = formatString.Replace("KEYFIELD", "{0}")
                imageColumn.NavigateURLFormatString = formatString
            End If

        End If

    Next

End Sub
```

26. 接下来，我们需要将出现在 DataGrid 中的删除图标连接到将删除员工记录的处理程序例程。我们所需要做的就是从收到的事件中提取 `ItemId` 值，使用 `Int32` 将其转换为整数，并将其传递给 Starter Kit 创建的 `DeleteEmployee` 例程。向下滚动并将此过程放在 `Page_Load` 过程之后：

```vb
Private Sub grdResults_DeleteCommand(ByVal source As Object, ByVal e As System.Web.UI.WebControls.DataGridCommandEventArgs) Handles grdResults.DeleteCommand
    Try
        Dim objEmployeeController As New EmployeeController
        objEmployeeController.DeleteEmployee(ModuleId, Int32.Parse(e.CommandArgument.ToString()))
        'redisplay results in the DataGrid
        BindData()
    Catch exc As Exception
        ProcessModuleLoadException(Me, exc)
    End Try
End Sub
```

27. 最后，我们想要捕获筛选选择更改时。 在 Event Handlers 区域底部添加这两个新过程：

```vb
Private Sub ddlManagerNo_SelectedIndexChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles ddlManagerNo.SelectedIndexChanged
    BindData()
End Sub

Private Sub ddlSalaryRange_SelectedIndexChanged(ByVal sender As System.Object,
                                               ByVal e As System.EventArgs) Handles ddlSalaryRange.SelectedIndexChanged
    BindData()
End Sub
```

28. 从 **File** 菜单中选择 **Save All**。

29. 要检查结果，请将模块构建并部署到您的 DNN 站点（有关说明，请参阅第 5 章中的配方 *Deploying a module as a standalone package in Chapter 5*）。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_195_0.png)

### How it works...

在本配方中，我们采用了 Starter Kit 中的基本模块并以三种方式扩展了它：

- 我们添加了来自上一个配方的筛选控件
- 我们用 DataGrid 控件替换了 DataList 控件
- 我们创建了一个新的数据库过程，用筛选器扩展了基本的 `GetEmployees` 过程

### There's more...

如果您查看本配方中的存储过程，您可能会觉得它在筛选 Employee 记录方面做了不必要的工作（创建临时表等）。存储过程中额外的代码是为了支持分页，我们将在下一个配方中看到。

### See also

在下一个配方中，我们将通过添加分页控件进一步扩展 DataGrid，以便我们可以将 DataGrid 结果限制为可管理的大小。

## Adding a paging control to a DataGrid

在本配方中，我们将通过向 DataGrid 和筛选器添加分页控件来扩展上一个配方。DNN 提供了一个非常好的分页控件，它会根据我们选择的页面大小将筛选后的搜索结果拆分为多个页面。

在本配方中，我们将在 `ViewEmployee` 表单上添加一个新的下拉列表来控制页面的大小。然后我们将修改我们的筛选过程以接受并使用分页变量。

### Getting ready

要跟随本配方，您必须已完成以下配方：

- Displaying a DataGrid with filter controls

### How to do it...

1. 首先启动 Development Tool 并加载 Employee 项目。
2. 双击打开 `ViewEmployee.ascx` 文件。
3. 文件顶部有一个 HTML 表格，用于保存我们在前几章中创建的筛选器。我们需要添加另一个下拉控件来保存页面大小的选项。找到上一个配方中的 HTML 表格并按如下方式更改代码：

```aspx
<table id="tblPanel" cellspacing="2" cellpadding="2" border="0"
      runat="server">
    <tr>
        <td width="12%" class="SubHead">
            <dnn:Label ID="lblManagerNo" runat="server"
                      ControlName="ddlManagerNo" Suffix=":" >
            </dnn:Label>
        </td>
        <td width="18%">
            <asp:DropDownList ID="ddlManagerNo" runat="server"
                      DataTextField="ManagerName"
                      DataValueField="ManagerNo"
                      AutoPostBack="true">
            </asp:DropDownList>
        </td>
        <td width="5%">
            &nbsp;
        </td>
        <td width="12%" class="SubHead">
            <dnn:Label ID="lblSalaryRange" runat="server"
                       ControlName="ddlSalaryRange" Suffix=":">
            </dnn:Label>
        </td>
        <td width="18%">
            <asp:DropDownList ID="ddlSalaryRange" runat="server"
                              DataTextField="Text" DataValueField="Value"
                              AutoPostBack="true">
            </asp:DropDownList>
        </td>
        <td width="5%">
            &nbsp;
        </td>
        <td width="12%" class="SubHead">
            <dnn:Label ID="lblRecordsPage" runat="server"
                       CssClass="SubHead"
                       ResourceKey="RecordsPerPage" Suffix=":">
            </dnn:Label>
        </td>

        <td width="18%">
            <asp:DropDownList ID="ddlRecordsPerPage"
                              runat="server" AutoPostBack="True">
                <asp:ListItem Value="10">10</asp:ListItem>
                <asp:ListItem Value="25">25</asp:ListItem>
                <asp:ListItem Value="50">50</asp:ListItem>
                <asp:ListItem Value="100">100</asp:ListItem>
                <asp:ListItem Value="250">250</asp:ListItem>
            </asp:DropDownList>
        </td>

    </tr>
</table>
```

4. 接下来，滚动到文件底部并在 DataGrid 之后添加此分页控件代码：

```aspx
<br />
<dnn:PagingControl ID="ctlPagingControl" runat="server">
</dnn:PagingControl>
<br />
```

5. 双击打开 `ViewEmployee.ascx.vb` 文件。

6. 为了使分页工作，我们需要记住当前选择的页码。为此我们创建一个私有变量来保存 Page Index。索引从 0 开始（第一页），我们将其传递给存储过程，以便它可以计算哪些记录应出现在页面上。查看文件顶部并添加 Private Members 区域，其中包含单个变量声明。将其放在 Private Methods 区域之前：

```vb
#Region "Private Members"

    Dim PageIndex As Integer = 1

#End Region

#Region "Private Methods"

    Private Sub BindData()
```

7. 稍微向下滚动到 `BindData()` 过程并进行以下更改：

```vb
Private Sub BindData()
    Dim objEmployees As New EmployeeController
    Dim colEmployees As ArrayList

    Dim TotalRecords As Integer
    Dim PageSize As Integer = Convert.ToInt32(ddlRecordsPerPage.SelectedValue)

    Dim CurrentPage As Integer = PageIndex
    If CurrentPage > 0 Then CurrentPage = CurrentPage - 1

    ' use the filter procedure to retrieve the records
    colEmployees = objEmployees.GetEmployeesByFilter(ModuleId, ddlManagerNo.SelectedItem.Value,
                    ddlSalaryRange.SelectedItem.Value,
                    CurrentPage, PageSize, TotalRecords)

    If colEmployees.Count > 0 Then
        grdResults.Visible = True
        grdResults.DataSource = colEmployees
        grdResults.DataBind()

        ctlPagingControl.Visible = (PageSize < TotalRecords)
        InitializePaging(ctlPagingControl, TotalRecords, PageSize)
    Else
        ' hide the controls that need data
        grdResults.Visible = False
        ctlPagingControl.Visible = False
    End If

End Sub
```

