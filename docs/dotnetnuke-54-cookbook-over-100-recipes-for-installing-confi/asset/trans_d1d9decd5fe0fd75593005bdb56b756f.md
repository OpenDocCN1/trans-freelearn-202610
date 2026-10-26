

+   10. 向下滚动到 BindData 过程，并修改对 GetEmployeesByFilter 的调用以包含当前区域设置：

```
' use the filter procedure to retrieve the records
colEmployees =
    objEmployees.GetEmployeesByFilter(ModuleId,
    ddlManagerNo.SelectedItem.Value,
    ddlSalaryRange.SelectedItem.Value,
Type(Page, DotNetNuke.Framework.CDefault).PageCulture.Name,
    CurrentPage, PageSize, TotalRecords)

If colEmployees.Count > 0 Then
```

+   11. 从“文件”菜单中选择“全部保存”。

+   12. 要检查结果，请部署更新的存储过程代码，然后生成并将模块部署到 DNN 站点。

+   13. 当使用语言选择器更改当前区域设置时，网格控件将查询数据库，薪资值将使用新的汇率计算从美元 (USD) 更改为加拿大元 (CAN)：

| 名 | 姓 | 经理 # | 雇用日期 | 薪资 |
| Henry1 | Smith1 | 0 | 10/10/2001 | 70000 |
| Thomas | Brown | 0 | 9/9/2000 | 60000 |
| Katie | Smith | 11 | 8/8/2001 | 35000 |
| James | Watson | 0 | 7/7/1992 | 95000 |
| Debby | Smith | 4 | 6/6/1996 | 45000 |
| Prénom | Nom de famille | Gestionnaire # | Date d'embauche | Salaire |
| Henry1 | Smith1 | 0 | 2001-10-10 | 71849,4 |
| Thomas | Brown | 0 | 2000-09-09 | 61585,2 |
| Katie | Smith | 11 | 2001-08-08 | 35924,7 |
| James | Watson | 0 | 1992-07-07 | 97509,9 |
| Debby | Smith | 4 | 1996-06-06 | 46188,9 |

## 工作原理...

这只是一个简单的示例，展示了模块如何生成本地化输出。为简化起见，我们对汇率进行了硬编码，但真正本地化的存储过程会查找正确的每日汇率，并在公式中使用该汇率。

## 本地化 DataGrid 控件

DataGrid 控件是模块显示内容的常见方式。DNN 提供了一些对 DataGrid 本地化的支持，在本配方中，我们将了解它的工作原理以及如何提供 DataGrid 所需的翻译字符串。

## 准备工作

要继续操作，您必须已完成以下配方：

-   向 DataGrid 添加分页控件
-   本地化标签、标题、面板和链接

## 操作步骤

+   1. 启动开发工具并加载 Employee 项目。
+   2. 打开 ViewEmployee.ascx.vb 文件。
+   3. 向下滚动到 Page_Load 过程。
+   4. 在该过程中，找到对绑定例程的调用，并添加以下代码以触发 DataGrid 控件的本地化：

```
If Not Page.IsPostBack Then

    If Not Request.QueryString("PageRecords") Is Nothing Then
    ddlRecordsPerPage.SelectedValue =
                    Request.QueryString("PageRecords")

    End If

    BindManagerDropDown()
    BindSalaryDropDown()

    ' have grid get column titles from resource file
    Localization.LocalizeDataGrid(grdResults, LocalResourceFile)

    BindData()

End If
```

+   5. 接下来，我们需要将标题的英文文本添加到本地资源文件中。打开 ViewEmployee.ascx.resx 文件。

6. 添加以下条目：

-   First.Header: First
-   Last.Header: Last
-   Hire Date.Header: Hire Date
-   Manager #.Header: Manager #
-   Salary.Header: Salary

7. 该文件现在应如下所示：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_307_0.png)

8. 生成并将模块部署到 DNN 站点。

9. 在测试结果之前，我们需要做的最后一件事是为新的网格标题创建法语翻译。按照“本地化标签、标题、面板和链接”配方中的步骤，确保以下法语翻译出现在 ViewEmploye.ascx 的语言编辑器中：

-   First.Header: Prénom
-   Hire Date.Header: Date d'embauche
-   Last.Header: Nom de famille
-   Manager #.Header: Gestionnaire #
-   Salary.Header: Salaire

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_307_1.png)

10. 单击“保存资源文件”（位于最底部）。

11. 转到 Employee 模块。

12. 使用语言选择器控件在语言之间切换。列标题将显示正确翻译的值。注意分页控件也已被本地化：

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_308_0.png)

## 工作原理...

对 DataGrid 标题进行本地化需要使用 DotNetNuke.Services.Localization 命名空间中的 LocalizeDataGrid API 调用。它要求我们在本地资源文件中提供字符串翻译。

## 另请参阅

LocalizeDataGrid 例程将本地化 DataGrid 标题，但要使查询结果发生变化，需要将当前区域设置传递给从数据库提取结果的后端存储过程。要了解如何实现这一点，请参阅上一个配方“本地化存储过程”。

# 10

# 现有模块的高级技巧

-   将 HTML 模块与 jQuery 配合使用
-   将 HTML 模块与替换令牌配合使用
-   将 HTML 模块与 Flash 配合使用
-   显示 XML 源
-   控制友好 URL 重写
-   使用 Google Analytics 跟踪站点
-   将日历事件发布为 RSS 源
-   设计自己的表单
-   样式化自己的表单

## 简介

在前面的章节中，我们看到了如何配置和使用 DotNetNuke 核心模块的配方。随着您越来越熟悉管理 DNN 门户和开发自己的自定义模块，您会希望更深入地挖掘，看看这些模块还提供哪些功能。

在本章中，我们将介绍一些新模块，并探索 DNN 中可用的其他功能。我们将了解 HTML 模块是多么灵活，一些底层处理是如何发生的，以及如何使用自己的自定义表单扩展 DNN 数据库。

## 将 HTML 模块与 jQuery 配合使用

jQuery 是一个简洁的 JavaScript 库，专注于 JavaScript 与 HTML 之间的交互。从版本 5 开始，DNN 支持在 DNN 页面中使用 jQuery 代码。

在本配方中，我们将演示一个小型 jQuery 程序，该程序扩展了之前的“图像条”示例，添加了悬停在当前所选图像上的缩略图。

## 准备工作

要继续操作本配方，您必须已完成第 3 章中的配方：

-   *使用 HTML 模块*

## 操作步骤

+   1. 在使用 jQuery 编码之前，检查 DNN 安装所支持的版本很重要。要查看版本，请先以超级用户身份登录（在我们的示例中为 `sys...`）。
+   2. 从 **主机** 菜单中选择 **主机设置**。
+   3. 向下滚动越过 **高级选项**，直到看到 **jQuery 设置**。

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_310_0.png)

+   4. 在 **jQuery 设置** 下，您将看到安装支持的版本。除了 jQuery 版本，您还可以看到可用选项。以下是一个简要说明：
+      - 取消选择 **使用 jQuery 调试版本** 将使 DNN 使用更小、更紧凑的库，但提供的调试信息更少。
+      - **使用托管 jQuery 版本** 意味着 DNN 将使用给定 URL 处的库。如果未选中，将使用库的本地副本。
+   5. 要了解如何在 HTML 模块中使用 jQuery，请转到 *使用 HTML 模块* 配方中创建的页面。

```css
border: 1px solid #ddd;
}
ul.image {
    float: left;
    list-style: none;
    margin: 0; padding: 0;
}
ul.image li {
    margin: 0; padding: 5px;
    float: left;
    position: relative;
    width: 120px;
    height: 60px;
}
ul.image li img {
    width: 120px; height: 60px;
    border: 1px solid #ddd;
    padding: 2px;
    background: #f0f0f0;
    position: absolute;
    left: 0; top: 0;
    -ms-interpolation-mode: bicubic;
}
#main_view {
    float: left;
    padding: 2px 0;
}
</style></p>
<script type="text/javascript">
$(document).ready(function(){

$("ul.image li").hover(function() {
    $(this).css({'z-index' : '10'});
    $(this).find('img').addClass("hover").stop()
        .animate({
            marginTop: '-100px',
            marginLeft: '-120px',
            top: '50%',
            left: '50%',
            width: '194px',
            height: '134px',
            padding: '20px'
        }, 400);

    }, function() {
        $(this).css({'z-index' : '0'});
        $(this).find('img').removeClass("hover").stop()
            .animate({

