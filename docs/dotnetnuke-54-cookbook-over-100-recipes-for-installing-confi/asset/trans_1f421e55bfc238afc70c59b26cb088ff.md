

## 酷炫的 Web 控件

在本章中，我们将介绍以下主题：

- 将 Web 控件添加到工具箱
- 在 DataGrid 中显示电子邮件链接
- 在 DataGrid 中显示复选框
- 在 DataGrid 中显示缩略图
- 创建可编辑的标签
- 在输入时提供文本建议
- 在树形视图中显示数据
- 使用标签条分隔内容
- 使用 CAPTCHA 控件保障安全
- 创建多状态复选框

### 简介

DNN 的强大功能之一，就是提供了多种灵活且可复用的控件，用于自定义模块开发。其中包括核心 DNN 页面上的许多 Web 控件，例如可编辑标签、树形视图、CAPTCHA 控件和 Tab 控件。在本章中，我们将了解如何将这些 Web 控件添加到自定义模块，并将它们与数据库中的表关联起来。

通常，使用这些控件只需四个简单步骤：

- 将控件代码添加到 View 或 Edit 的 `.ascx` 文件中
- 向 info 对象添加一个新属性，用于为控件提供值
- 将控件绑定到这些值
- 从控件获取值并保存到数据库（如果是 Edit 页面）

由于本章的实例主要关注 Web 控件和数据库中的值，因此你需要先构建并部署*第6章*中的示例员工模块。我们还将扩展*第6章*中的 DataGrid，并展示它可以显示的不同类型的列。

### 将 Web 控件添加到工具箱

如果你经常使用开发工具中的可视化编辑器来布局页面，那么这个简短实例将向你展示如何将 DNN Web 控件添加到工具箱中。

### 操作步骤...

1. 启动开发工具。
2. 将编辑器切换到设计模式。
3. 确保工具箱已显示。
4. 右键单击工具箱，然后选择 **选择项...**。
5. 单击 **浏览** 按钮。
6. 在 DNN 源代码中导航到 `/bin` 文件夹（`DNNSource/website/bin`）。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_206_0.png)

7. 选择 DotNetNuke.WebControls.dll，并单击 **打开**。
8. 确保 DNN 控件已勾选，并单击 **确定**。
9. 现在，当你编辑代码时，这些 Web 控件将出现在工具箱的 **常规** 部分下。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_207_0.png)

10. 接下来，我们需要添加对 DotNetNuke.WebUtility.dll 的引用。在解决方案资源管理器中右键单击 Employee 项目，然后选择 **添加引用...**。
11. 在弹出的对话框中，单击 **浏览** 选项卡，然后导航到存放 DNN 源文件的文件夹（例如，我的文档\DNNSource\Website\bin）。
12. 选择 DotNetNuke.WebUtility.dll 文件，并单击 **确定**。

### 在 DataGrid 中显示电子邮件链接

正如我们在上一章中所看到的，DataGrid 控件非常适合以格式整齐的表格显示数据库中的记录。但 DataGrid 还可以显示许多其他类型的信息，在本实例中，我们将了解如何在一列中显示电子邮件超链接。

### 准备工作

在本实例中，我们将扩展第6章末尾创建的 DataGrid。要继续操作，你需要先成功完成并部署*使用筛选控件显示 DataGrid* 项目。

> 在本实例中，我们使用一个函数来为示例生成电子邮件地址。这使实例保持简单，但并不太实用。在真实的生产环境中，你会将其作为 Employee 表的一部分存储在数据库中。

### 操作步骤...

1. 启动开发工具并加载 **Employee** 项目。
2. 双击打开 `ViewEmployee.ascx` 文件。
3. 在代码中找到 DataGrid，并在 **Salary** 列之后添加一个新列：

```aspx
<dnn:textcolumn datafield="Salary" headertext="Salary"/>

<asp:TemplateColumn HeaderText="Email Contact">
    <ItemTemplate>
        <asp:HyperLink id="hlEmail"
                       NavigateUrl='<%# "mailto:" & DataBinder.Eval(Container.DataItem,"ContactEmail") %>'
                       Text='<%# DataBinder.Eval(Container.DataItem,"ContactEmail") %>'
                       Target="_new"
                       runat="server" />
    </ItemTemplate>
</asp:TemplateColumn>

</Columns>
</asp:datagrid>
```

4. 接下来，打开 `EmployeeInfo.vb` 文件。
5. 找到“公共属性”部分，并添加只读属性 `EmailAddress`，该属性根据员工姓名构造一个电子邮件地址：

```vb
' public properties

Public ReadOnly Property EmailAddress() As String
    Get
        Return _EmpFirstName.Substring(0, 1) + _EmpLastName + "@yourcompany.com"
    End Get
End Property
```

6. 从 **文件** 菜单中选择 **全部保存**。
7. 要检查结果，请构建模块并将其部署到开发门户进行测试（相关说明请参阅第5章中的*将模块作为独立包部署*实例）。
8. 转到 ACME Employee 页面查看员工列表。新的电子邮件超链接将出现在右侧。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_209_0.png)

### 工作原理...

在本实例中，我们完成了在 DataGrid 控件中显示电子邮件超链接的任务：

- 我们采用了上一实例中的 DataGrid 控件，并添加了一个包含电子邮件超链接控件的新模板列。
- 我们向 EmployeeInfo 对象添加了一个新属性，用于为 DataGrid 提供电子邮件地址。

### 在 DataGrid 中显示复选框

DataGrid 中一个有用的显示元素是类似复选框的图像，用于指示数据库记录的状态。这些并不是真正可用的复选框，而是一种视觉指示器，用于显示数据为 true 或 false。

该控件通过两张图像工作：一张带有对勾，当值为 true 时显示；另一张为未勾选图像，当值为 false 时显示。

> 本实例适用于任何表示 true 或 false 的图像。类似复选框的图像在其他 DNN 模块中也有使用，因此用户会很熟悉，但你也可以尝试使用自己的图像。

本实例有两个基本步骤：

- 我们将创建 EmployeeInfo 对象的一个新属性，名为 `NewHire`。该属性检查数据库中的入职日期，如果员工的入职时间在 30 天以内，则返回 true。
- 我们将向 DataGrid 添加一个新列，该列会计算 `NewHire` 属性，如果 `NewHire` 为 true 则显示一张图像，如果 `NewHire` 为 false 则显示另一张图像。

### 准备工作

在本实例中，我们将扩展在*第6章*中创建的 DataGrid。要继续操作，你需要先成功完成并部署*使用筛选控件显示 DataGrid* 项目。

### 操作步骤...

1. 启动开发工具并加载 **Employee** 项目。
2. 双击打开 `ViewEmployee.ascx` 文件。
3. 第一步是向 DataGrid 添加一个新列，用于显示复选框图像。我们将使用 `Eval` 函数来检查 `NewHire` 属性。找到 DataGrid，并在 `Salary` 列之后添加一个新列：

```aspx
<dnn:textcolumn datafield="Salary" headertext="Salary"/>

<asp:TemplateColumn HeaderText="New Hire">
    <ItemTemplate>
        <asp:Image Runat="server" ID="imgApproved"
                   ImageUrl="~/images/checked.gif" Visible='<%# DataBinder.Eval(Container.DataItem,"NewHire") = "1" %>'/>
        <asp:Image Runat="server" ID="imgNotApproved"
                   ImageUrl="~/images/unchecked.gif" Visible='<%# DataBinder.Eval(Container.DataItem,"NewHire") = "0" %>'/>
    </ItemTemplate>
</asp:TemplateColumn>

</Columns>
</asp:datagrid>
```

4. 接下来，打开 `EmployeeInfo.vb` 文件。
5. 找到 `Public Properties` 部分，并添加只读属性 `NewHire`。如果员工在过去 30 天内入职，该属性返回 true 或 false：

```vb
' public properties

Public ReadOnly Property NewHire() As Boolean
    Get
        Return (Today - _HireDate).Days < 30
    End Get
End Property
```

