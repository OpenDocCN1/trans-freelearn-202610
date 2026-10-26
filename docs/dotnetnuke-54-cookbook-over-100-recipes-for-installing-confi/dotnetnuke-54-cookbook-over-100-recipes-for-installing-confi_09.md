# 创建自定义模块操作

在许多教程中，我们使用了出现在模块标题旁边的模块操作菜单，用于选择其他模块功能。继承自 `PortalModuleBase` 类的模块会在菜单中收到几个默认操作（如导入/导出内容）。在本教程中，我们将展示如何使用 `iActionable` 接口在模块菜单中添加您自己的操作。

定义我们自己的模块操作有三个基本任务：

- 定义自定义操作菜单项
- 创建子程序来处理 `Click` 事件
- 将 `Click` 事件处理程序连接到菜单项

## 准备工作

要继续本教程，您必须已完成来自 *第5章，构建、调试和部署模块* 中的以下教程：

- *将模块部署为独立包*

在本教程中，我们将通过创建一个简单的 HTML 统计表来演示操作菜单处理，该统计表位于查看员工表单上，在单击 **显示统计信息** 操作菜单项之前是隐藏的。

## 如何操作...

1. 首先启动开发工具并加载 Employee 项目。
2. 双击打开 `ViewEmployee.ascx` 文件。向下滚动到文件底部，并添加这个新的隐藏 HTML 表格：

```html
<table id="tblStats" cellspacing="2" cellpadding="2" border="0"
       runat="server" style="width: 100%" visible="false">
  <tr>
    <td width="25%" class="SubHead">
      Total Company Sales:
    </td>
    <td width="75%">
      $1,234,567.00
    </td>
  </tr>
  <tr>
    <td width="25%" class="SubHead">
      Total Company Expenses:
    </td>
    <td width="75%">
      $234,567.00
    </td>
  </tr>
</table>
```

3. 接下来，双击打开 `ViewEmployee.ascx.vb` 文件。
4. 在文件顶部添加必要的导入指令：

```vb
Imports DotNetNuke.Entities.Modules.Actions
```

5. 向下滚动到 **可选接口** 区域，并查找 `ModuleActions` 属性。

```
Public ReadOnly Property ModuleActions() As Entities.Modules.
Actions.ModuleActionCollection Implements Entities.Modules.
IActionable.ModuleActions
    Get
        Dim Actions As New Entities.Modules.Actions.
        ModuleActionCollection
        Actions.Add(GetNextActionID, Localization.GetString(Entities.
        Modules.Actions.ModuleActionType.AddContent, LocalResourceFile),
        Entities.Modules.Actions.ModuleActionType.AddContent, "", "",
        EditUrl(), False, Security.SecurityAccessLevel.Edit, True,
        False)
    End Get
```

```
Return Actions
    End Get
End Property
```

6. 如果您查看 Starter Kit 最初创建的代码，会发现已经定义了一个操作。这就是我们用来添加新员工记录的“添加内容”操作。

7. 既然我们看到了操作的形式，让我们创建第二个操作，使新的 HTML 表格可见：

```
Public ReadOnly Property ModuleActions() As
    Entities.Modules.Actions.ModuleActionCollection Implements
    Entities.Modules.IActionable.ModuleActions
    Get
Dim Actions As New
    Entities.Modules.Actions.ModuleActionCollection
Actions.Add(GetNextActionID,
    Localization.GetString(Entities.Modules.Actions.
    ModuleActionType.AddContent, LocalResourceFile),
    Entities.Modules.Actions.ModuleActionType.AddContent, "", "",
    EditUrl(), False, Security.SecurityAccessLevel.Edit, True,
    False)

Actions.Add(GetNextActionID, "Show Statistics",
    Entities.Modules.Actions.ModuleActionType.AddContent,
    "ShowStats", "view.gif", "", False,
    Security.SecurityAccessLevel.View, True, False)

Return Actions
    End Get
End Property
```

8. 这将在模块操作菜单中创建一个名为“显示统计信息”的新项。下一步是创建一个子程序，在选中菜单项时执行操作。向下滚动到“可选接口”区域的底部。

9. 添加以下子程序：

```
Private Sub ModuleAction_Click(ByVal sender As Object,
    ByVal e As ActionEventArgs)
    Select Case e.Action.CommandArgument
    Case "ShowStats"
        tblStats.Visible = True
    End Select
End Sub
```

10. 该例程将在选中操作时使 HTML 可见。最后一步是将操作菜单项连接到这个点击处理程序。滚动到 Page_Load 例程，并在检查回发之前添加以下代码：

```
Private Sub Page_Load(ByVal sender As System.Object,
    ByVal e As System.EventArgs) Handles MyBase.Load
    Try

    'Add an Action Event Handler to the Skin
    AddActionHandler(AddressOf ModuleAction_Click)

    If Not Page.IsPostBack Then
```

11. 从“文件”菜单中选择“全部保存”以保存更改。

12. 重新生成并重新部署模块。

13. 转到包含 Employee 模块的页面。当您第一次看到该页面时，统计表应该是不可见的。

14. 查看模块操作菜单下方。您应该看到新的菜单选项“显示统计信息”。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_351_0.png)

15. 从菜单中选择“显示统计信息”，统计表应显示在页面底部。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_351_1.png)

## 工作原理...

调用 Actions.Add() 定义新操作时，传入了九个关键参数。在下表中，您可以看到这些参数以及它们在此示例中的使用方式：

| 参数 | 原始模板操作 | 新操作 |
|---|---|---|
| 标题 | 从资源文件提取 | 硬编码“显示统计信息” |
| 命令名/操作 | AddContent | AddContent |
| 可选参数 | 无 | "ShowStats" |
| 显示图标 | 无 | "view.gif" |
| 可选 URL | EditUrl() | 无 |
| 操作事件（T/F） | False | False |
| 所需安全级别 | Edit | View |
| 可见（T/F） | True | True |
| 新窗口（T/F） | False | False |

## 更多内容...

在本示例中，我们对菜单项文本“显示统计信息”和统计数据进行了硬编码。这样做是为了使示例更易于阅读。在实际模块中，您应该通过使用资源文件来避免硬编码文本（例如 Localization.GetString("StatisticsPrompt", LocalResourceFile)）。
