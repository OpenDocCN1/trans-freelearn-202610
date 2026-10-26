

## 数据录入技巧

3. 通过点击 Add List 创建一个新列表。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_176_0.png)

4. 输入我们第一个条目的详细信息：

   - List Name: Salary Range
   - Parent List: None Specified
   - Parent Entry: blank
   - Entry Text: $0 or more
   - Entry Value: 0
   - Enable Sort Order: Checked

5. 点击 Save。

6. 点击 Add Entry 并创建第二个列表条目：

   - Entry Text: $30,000 or more
   - Entry Value: 30000

7. 点击 Save。

8. 点击 Add Entry 并创建第三个列表条目：

   - Entry Text: $60,000 or more
   - Entry Value: 60000

9. 点击 Save。

10. 点击 Add Entry 并创建第四个列表条目：

    - Entry Text: $100,000 or more
    - Entry Value: 100000

11. 点击 **Save**。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_177_0.png)

12. 现在我们有了 DNN 列表，我们可以在自定义模块中使用它。启动 Development Tool。

13. 在屏幕左侧查找 Recent Projects。点击 **Employee** 加载项目。

14. 双击打开 `ViewEmployee.ascx` 文件。

15. 查看文件顶部：

```aspx
<%@ Control language="vb"
    Inherits="ACME.Modules.Employee.ViewEmployee"
    AutoEventWireup="false" Explicit="True"
    Codebehind="ViewEmployee.ascx.vb" %>
<%@ Register TagPrefix="dnn" TagName="Audit"
    Src="~/controls/ModuleAuditControl.ascx" %>
<%@ Register TagPrefix="dnn" TagName="Label"
    Src="~/controls/LabelControl.ascx" %>

<asp:datalist id="lstContent" datakeyfield="ItemID"
    runat="server" cellpadding="4">
    <itemtemplate>
```

16. 在最后一个 register 指令之后、Datalist 之前添加以下 HTML 表格代码：

```aspx
<%@ Register TagPrefix="dnn" TagName="Label" Src="~/controls/LabelControl.ascx" %>

<table id="tblPanel" cellspacing="2" cellpadding="2" border="0"
       runat="server">
    <tr>
        <td width="20%" class="SubHead">
            [Put a label here]
        </td>
        <td width="25%">
            [Put a control here]
        </td>
        <td width="10%">
            &nbsp;
        </td>
        <td width="20%" class="SubHead">
            <dnn:Label ID="lblSalaryRange" runat="server"
                ControlName="ddlSalaryRange" Suffix=":">
            </dnn:Label>
        </td>
        <td width="25%">
            <asp:DropDownList ID="ddlSalaryRange"
                runat="server" DataTextField="Text"
                DataValueField="Value" AutoPostBack="true">
            </asp:DropDownList>
        </td>
    </tr>
</table>
```

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_178_0.png)

> 如果你遵循了之前的配方并且这里已经有一个 HTML 表格，只需将其替换为这个新的 HTML 表格。

17. 接下来，打开 `ViewEmployee.ascx.vb` 文件。

18. 第一步是导入 Lists 命名空间。向下滚动一点并添加 import 语句：

```vb
Imports System.Web.UI
Imports System.Collections.Generic
Imports System.Reflection

Imports DotNetNuke
Imports DotNetNuke.Services.Exceptions
Imports DotNetNuke.Services.Localization
Imports DotNetNuke.Common.Lists
```

### How it works...

我们可以直接将下拉列表控件绑定到 DNN 列表。这允许主机用户在无需重新部署模块的情况下向列表添加值。

### There's more...

由于我们在页面显示时绑定到列表，你可以向 DNN 列表添加更多值，并且它们会在页面刷新时自动出现在下拉列表中。

## 从存储过程填充下拉列表

在本配方中，我们将看到如何从存储过程填充下拉列表。这与绑定到 DNN 列表类似，但现在我们可以使用数据库中存储的任何数据作为列表。

### Getting ready

要跟随本配方，你必须已完成以下配方：

- *Creating stored procedures*（来自 *Chapter 4*）
- *Populating a drop-down list from a DNN list*

### How to do it...

1. 首先启动 Development Tool 并加载 Employee 项目。
2. 双击打开 `ViewEmployee.ascx` 文件。
3. 找到从上一个配方添加的表格和薪资控件。查找两个占位符并用以下标签和控件代码替换它们，使 HTML 表格现在看起来像这样：

```html
<table id="tblPanel" cellspacing="2" cellpadding="2" border="0"
       runat="server" >
    <tr>
        <td width="20%" class="SubHead">
            <dnn:Label ID="lblManagerNo" runat="server"
                      ControlName="ddlManagerNo" Suffix=":" />
        </td>
        <td width="25%">
            <asp:DropDownList ID="ddlManagerNo" runat="server"
                DataTextField="ManagerName"
                DataValueField="ItemId" AutoPostBack="true">
            </asp:DropDownList>
        </td>
        <td width="10%">
            &nbsp;
        </td>
        <td width="20%" class="SubHead">
            <dnn:Label ID="lblSalaryRange" runat="server" ControlName="ddlSalaryRange" Suffix=":">
            </dnn:Label>
        </td>
        <td width="25%">
            <asp:DropDownList ID="ddlSalaryRange" runat="server" DataTextField="Text" DataValueField="Value" AutoPostBack="true">
            </asp:DropDownList>
        </td>
    </tr>
</table>
```

4. 现在我们有了下拉控件，下一步是添加我们需要例程来从数据库中的存储过程填充下拉控件。让我们先创建存储过程。

5. 双击打开 01.00.00.SqlDataProvider 文件。

> 如果你已经安装过一次此模块，你可以创建一个名为 01.00.01.SqlDataProvider 的新文件来保存新过程。详情请参阅第 11 章中的配方 Deploying a new module version。

6. 我们将向脚本添加一个新过程，它与 Starter Kit 创建的 GetEmployees 过程非常相似，但稍有调整。在示例 Employee 表中，有一个名为 ManagerNo 的字段保存员工经理的 ID。如果 ManagerNo 为 0，则表示该员工是经理。因此，为了获取下拉控件的所有经理列表，存储过程需要选择所有 ManagerNo 为 0 的员工。过程的代码如下：

```sql
create procedure {databaseOwner}{objectQualifier}ACME_GetManagers

    @ModuleId int

as

select Emp.ModuleId,
Emp.ItemId,
Emp.FirstName,
Emp.LastName,
ManagerNo,
HireDate,
Salary,
CreatedByUser,
Emp.CreatedDate,
'CreatedByUserName' = Usr.FirstName + ' ' + Usr.LastName
from {objectQualifier}ACME_Employee As Emp
inner join {objectQualifier}Users As Usr on Emp.CreatedByUser = Usr.UserId
where ModuleId = @ModuleId And ManagerNo = 0

GO
```

7. 接下来我们需要创建将员工控制器连接到此存储过程的例程。双击打开 DataProvider.vb 文件。

8. 滚动到文件底部并查找名为 Abstract methods 的代码区域。添加以下函数定义：

```vb
Public MustOverride Function GetManagers(ByVal ModuleId As Integer) As IDataReader
```

9. 现在，打开 SqlDataProvider.vb 并滚动到文件底部 Public methods 区域。添加新函数：

```vb
Public Overrides Function GetManagers(ByVal ModuleId As Integer) As IDataReader
    Return CType(SqlHelper.ExecuteReader(ConnectionString,
        GetFullyQualifiedName("GetManagers"),
        ModuleId), IDataReader)
End Function
```

10. 接下来我们需要更改 EmployeeController 文件。双击 EmployeeController.vb 并滚动到 Public methods 区域底部。添加以下代码：

```vb
Public Function GetManagers(ByVal ModuleId As Integer) As List(Of EmployeeInfo)

    Return CBO.FillCollection(Of EmployeeInfo)(DataProvider.Instance().GetManagers(ModuleId))

End Function
```

11. 在这一点上我们有了填充下拉列表所需的例程，但最后一步是在 EmployeeInfo 对象中提供一个新属性，以便在下拉控件中显示格式良好的经理姓名。双击 EmployeeInfo.vb 并向下滚动一点以添加以下代码：

```vb
' public properties

Public ReadOnly Property ManagerName() As String
    Get
        Return _EmpLastName + ", " + _EmpFirstName
    End Get
End Property
```

12. 现在，为了将所有内容连接起来，我们需要进入 ViewEmployee 代码隐藏并将下拉列表绑定到我们刚刚创建的例程。双击打开 ViewEmployee.ascx.vb 文件。

13. 滚动到 Page_Load 过程并找到来自上一个配方的 BindSalaryDropDown 过程调用。在它之前添加 BindManagerDropDown 调用：

```vb
If Page.IsPostBack = False Then

    BindManagerDropDown()
    BindSalaryDropDown()

End If
```

14. 滚动到 Private methods 区域并在 BindSalaryDropDown 之前添加此新过程：

```vb
Private Sub BindManagerDropDown()

    Dim objEmployeeController As New EmployeeController

    ddlManagerNo.DataSource =
        objEmployeeController.GetManagers(ModuleId)
    ddlManagerNo.DataTextField = "ManagerName"
    ddlManagerNo.DataValueField = "ItemId"

    Try
    ddlManagerNo.DataBind()

    ' add an extra value to always show a prompt in the drop down

    ddlManagerNo.Items.Insert(0, New ListItem
        (Localization.GetString("ManagerPrompt",
            LocalResourceFile), "-1"))
```

