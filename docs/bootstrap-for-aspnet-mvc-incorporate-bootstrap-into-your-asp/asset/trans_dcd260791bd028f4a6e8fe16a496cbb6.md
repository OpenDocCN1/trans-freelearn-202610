

```cshtml
<script src="@Url.Content("~/js/plugins/morris/morris.js")"></script>
<script src="@Url.Content("~/js/dashboard-demo.js")"></script>
}
```

12. 最后，打开 _Layout.cshtml 文件并找到以下行：

    ```html
    <a href="index.html"><i class="fa fa-dashboard fa-fw"></i> Dashboard</a>
    ```

13. 将找到的行更改为以下行：

    ```cshtml
    <a href="@Url.Action("Index","Home")"><i class="fa fa-dashboard fa-fw"></i> Dashboard</a>
    ```

14. 保存并关闭所有文件并运行你的项目。

15. 当你导航到 **Dashboard** 页面时，你应该会看到一个图表渲染，类似于以下截图。

![](img/371b9a67300a03b7b335eb8a6fe55d94_155_0.png)

## Summary

在本章中，你学习了如何将预设计的 HTML 模板转换为可用的 ASP.NET MVC 项目。本章中展示的技术可以应用于几乎任何 HTML 模板，让你能够在无需自行设计布局的情况下构建专业设计的 Web 应用程序。

在下一章中，我们将探讨如何在自己的 ASP.NET MVC 项目中包含和使用 jQuery DataTables 插件。

## Using the jQuery DataTables Plugin with Bootstrap

jQuery DataTables 插件允许开发者为任何 HTML 表格添加创新交互控件。

jQuery DataTables 插件支持多种选项和丰富的扩展。ASP.NET MVC 开发者也可以将其包含在自己的项目中。本章的目的不仅是向你展示如何使用 DataTables 插件，还说明了如何将几乎任何开源 JavaScript 和 CSS 插件或框架与 ASP.NET MVC 一起使用。

在本章中，我们将涵盖以下主题：

- An overview of jQuery DataTables
- Including the jQuery DataTables plugin in your ASP.NET MVC project
- Loading and displaying data with jQuery DataTables and ASP.NET MVC
- Using some of the extensions

### jQuery DataTables

DataTables 是一个免费的、开源的 jQuery JavaScript 库插件，由一家名为 SpryMedia Ltd 的公司设计和创建。该插件使为任何标准 HTML 表格添加排序、过滤、分页和搜索等功能变得非常容易实现。它还提供各种扩展，可实现类似 Excel 的功能、内联编辑和固定列等。DataTables 网站提供有良好文档的示例、博客和论坛，你可以在 www.datatables.net 上找到。

jQuery DataTables 插件可以通过两种方式添加到你的 ASP.NET MVC 项目中，DataTables NuGet 包或 **Content Delivery Network (CDN)**。

### Adding DataTables to your ASP.NET MVC project

要添加 DataTables 插件的基本功能，需要以下两个文件：

- 第一个是 `jquery.dataTables.css`，它包含表格的默认 CSS 样式
- 第二个是 `jquery.dataTables.js`，它包含用于渲染 DataTables 插件并添加必要功能的 JavaScript 逻辑

这两个文件都可在 DataTables CDN 上获得，链接如下：

- //cdn.datatables.net/1.10.0/css/jquery.dataTables.css
- //cdn.datatables.net/1.10.0/js/jquery.dataTables.js

### Using the DataTables NuGet package

你也可以使用 NuGet 添加使用 jQuery DataTables 所需的所有 CSS 和 JavaScript 文件以及扩展的所有 CSS 和 JavaScript 文件。按照以下步骤使用 NuGet 添加 jQuery DataTables：

1. 在 Visual Studio 中，通过导航到 **Tools | Library Package Manager | Package Manager Console** 打开 **Package Manager Console** 窗口。
2. 在 **Package Manager Console** 窗口中，输入以下命令：`Install-Package jquery.datatables`
3. NuGet 包将在 `Content` 文件夹内添加一个 `DataTables-1.10.0` 文件夹，其中包含 DataTables 插件所需的 `css`、`images` 和 `swf` 文件。
4. 它还将在你的项目中向 `Scripts` 文件夹添加一个 `DataTables-1.10.0` 文件夹。该文件夹将包含 DataTables 插件的 JavaScript 文件以及所有扩展的 JavaScript 文件。

### Using the CDN

你可以从上述位置保存文件或将它们添加到项目中，或者更推荐的做法是添加对托管在 CDN 上的文件的引用，这将有助于提高你网站的性能。

要从 CDN 引用，请完成以下步骤：

1. 在 Visual Studio 中，打开本书附带的示例项目并打开位于 `Views\Shared` 文件夹中的 `_Layout.cshtml` 文件。

2. 在 `_Layout.cshtml` 文件的 `<head>` 元素内，通过插入以下标记行添加对 jQuery DataTables 样式表的引用：

    ```html
    <link rel="stylesheet" type="text/css" href="//cdn.datatables.net/1.10.0/css/jquery.dataTables.css">
    ```

3. 打开你需要 DataTables 功能的视图，并在视图底部添加对 JavaScript 库的引用，添加以下代码：

    ```cshtml
    @section scripts{
        <script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
    }
    ```

### Adding Bootstrap styling to DataTables

前一节中提到的步骤将向视图和布局文件添加生成 jQuery DataTables 基础样式和功能所需的最小文件。然而，默认的 DataTables CSS 样式在 Bootstrap 网站中看起来可能有些格格不入。

幸运的是，DataTables 项目团队创建了专门针对 Bootstrap 的 CSS 样式和 JavaScript 库，以匹配你网站的外观和感觉。这两个文件也可在 DataTables CDN 上获得：

- `//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.css`
- `//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js`

这两个文件以与普通 DataTables CSS 和 JavaScript 文件相同的方式添加。请记住，当在视图中包含特定于 Bootstrap 的 DataTables JavaScript 文件时，你需要同时引用默认的 DataTables JavaScript 文件以及特定于 Bootstrap 的文件，如以下标记所示：

```html
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js"></script>
```

### Loading and displaying data in jQuery DataTables

为了实现 jQuery DataTables 插件，我们首先需要创建一个新的视图，用于在 HTML 表格中列出数据。对于此示例，我们将创建一个列出 Northwind Traders 数据库中客户的视图。为此，完成以下步骤：

1. 在 Visual Studio 中，添加一个名为 CustomersController.cs 的新控制器类。

2. 我们在项目中实现了依赖注入，因此我们将向该类添加一个构造函数，该构造函数将自动使用以下代码设置对数据库上下文的引用：

    ```csharp
    public CustomersController(ApplicationDbContext context)
    {
        _context = context;
    }
    ```

3. _context 对象是在类内部声明的本地字段，如下所示：

    ```csharp
    private readonly ApplicationDbContext _context;
    ```

4. 向控制器添加一个名为 Index 的新方法。此方法向控制器返回一个 ActionResult 对象。向其中添加以下代码，该代码将客户对象列表传递给视图：

    ```csharp
    public ActionResult Index()
    {
        var model = _context.Customers;
        return View(model);
    }
    ```

```cshtml
<h1>Customers <small>Our Customers</small></h1>
</div>

<ol class="breadcrumb">
    <li>@Html.ActionLink("Home", "Index", "Home")</li>
    <li class="active">Customers</li>
</ol>
<table class="table table-striped table-hover">
    <thead>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.
                CustomerCode)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.
                CompanyName)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.
                ContactName)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.City)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Country)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Phone)
            </th>
        </tr>
    </thead>
    <tbody>
        @foreach (var item in Model)
        {
            <tr>
                <td>
                    @Html.ActionLink(item.CustomerCode,
                    "Edit", new { id = item.CustomerId })
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.
                    CompanyName)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.
                    ContactName)
                </td>
                <td>
```

