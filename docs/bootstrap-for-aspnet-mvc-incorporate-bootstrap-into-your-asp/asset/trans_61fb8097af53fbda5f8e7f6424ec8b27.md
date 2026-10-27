

## Bootstrap 图片类

可以通过将 `class` 属性设置为 `img-responsive` 来使图片响应式。这样会根据其父元素按比例缩放图片，将其最大 `width` 设置为 100%，`height` 设置为 auto。

你也可以通过圆角、圆形或外边框来塑造图片。这是通过将 `<img>` 元素的 class 设置为以下 Bootstrap 类之一来实现的：

- `img-rounded`
- `img-circle`
- `img-thumbnail`

在下面的屏幕截图中，我们展示了一份员工及其照片的列表。员工列表从数据库中检索并传递给视图：

![](img/371b9a67300a03b7b335eb8a6fe55d94_62_0.png)

实现上述结果的代码如下（可在随附的示例项目中查看）：

```cshtml
@model Northwind.Web.Models.EmployeeViewModel[]
<div class="container">
    <div class="row">
        <h2>About Us.</h2>
        <p>Northwind Traders are a premier supplier of various food products,</p>
    </div>
    <div class="row">
        <h3>Our Team</h3>
        @foreach (var item in Model)
        {
            <div class="col-md-4">
                <img src="@Url.Content("~/Images/employees/" + item.EmployeeID + ".png")" alt="@item.FirstName @item.LastName" class="img-circle img-responsive">
                <h3>
                    @item.FirstName @item.LastName <small>@item.Title</small>
                </h3>
                <p>@item.Notes</p>
            </div>
        }
    </div>
</div>
```

在这段代码中，我们遍历模型中的每个员工项并使用 `EmployeeID` 属性作为文件名渲染一个 `<img>` 元素。每个 `<img>` 元素的 class 属性都设置为 `img-circle`，这将使图片以圆形显示。

## 摘要

在本章中，我们探讨了如何使用 Bootstrap 网格和表单类来布局各种元素。我们还解决了使用标准 Bootstrap 验证类而不是 jQuery 验证类进行表单验证的问题，以及使用 ASP.NET 的编辑器模板简化表单元素的创建。

在下一章中，我们将探讨各种 Bootstrap 组件，包括 Bootstrap 导航栏、输入组、进度条和警告，以及如何在 ASP.NET MVC 项目中实现它们。

## 使用 Bootstrap 组件

Bootstrap 提供了十多个组件来提供输入组、下拉菜单、导航、警告以及图标。使用这些组件在你的 Web 应用程序中，你可以为用户提供一致且易于使用的界面。

Bootstrap 组件本质上是通过组合各种现有的 Bootstrap 元素、添加一些唯一的类名，并表示许多网站上常用的隐喻而构成的。

在本章中，我们将涵盖以下主题：

- 使用 Bootstrap 导航栏
- 实现上下文敏感的搜索栏
- 使用列表组、徽章和媒体对象
- 实现页面标题和面包屑
- 使用 PagedList 库创建分页列表
- 如何使用警告、输入组和按钮下拉菜单
- 实现一个使用 SignalR 报告实时进度的进度条

### Bootstrap 导航栏

Bootstrap 导航栏是使用 Bootstrap 框架的大多数网站上使用的组件之一。在 Visual Studio 中创建 ASP.NET MVC 网站时，默认模板会生成标准导航栏所需的所有 HTML 标记。Bootstrap 导航栏如下图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_65_0.png)

以下 `_Layout.cshtml` 文件中的代码创建了一个固定在顶部的响应式导航栏。当网站在屏幕尺寸较小的设备上打开时，它会显示一个带有三条杠的按钮，点击后会垂直显示菜单：

```cshtml
<div class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            @Html.ActionLink("Northwind Traders", "Index", "Home", null, new { @class = "navbar-brand" })
        </div>
        <div class="navbar-collapse collapse">
            @Html.Partial("_BackendMenuPartial")
            @Html.Partial("_LoginPartial")
        </div>
    </div>
</div>
```

请注意，我们使用两个局部视图根据当前用户是否已登录来生成其余的导航栏。当用户登录时，我们将向导航栏添加全局搜索功能；它将根据用户当前访问的页面返回结果。

首先，将以下代码添加到 `_BackendMenuPartial` 视图；这将在导航栏内生成一个搜索框：

```cshtml
@using (Html.BeginForm("Index", "Search", FormMethod.Post, new { @class = "navbar-form navbar-left", role = "search" }))
{
    <div class="form-group">
        @Html.TextBox("searchquery", "", new { @id = "searchquery", @class = "form-control input-sm", placeholder = "Search" })
        @Html.Hidden("fromcontroller", ViewContext.RouteData.Values["controller"], new { @id = "fromcontroller" })
    </div>
    <button type="submit" class="btn btn-default btn-xs">GO</button>
}
```

你会注意到在上面的代码中，我们有一个名为 `fromcontroller` 的隐藏字段，其中包含当前控制器的名称。我们从 `ViewContext` 上的 `RouteData` 对象获取此信息。此信息将用于根据当前页面返回搜索结果。

表单将以 HTTP `POST` 方法向 `Search` 控制器的 `Index` 动作发送请求；这反过来会检查请求来自哪个控制器名称并返回适当的数据和视图。执行搜索的代码如下：

```csharp
[HttpPost]
public ActionResult Index(string searchquery, string fromcontroller)
{
    switch (fromcontroller)
    {
        case "Products":
            return RedirectToAction("SearchProductsResult", new {
                query = searchquery });

        case "Customers":
            return RedirectToAction("SearchCustomersResult", new {
                query = searchquery });

        case "Employees":
            return RedirectToAction("SearchEmployeesResult", new {
                query = searchquery });
    }
    return View();
}

public ActionResult SearchCustomersResult(string query)
{
    ViewBag.SearchQuery = query;
    var results = _context.Customers.Where(p => p.CompanyName.Contains(query)
        || p.ContactName.Contains(query)
        || p.City.Contains(query)
        || p.Country.Contains(query)).ToList();
    return View(results);
}
```

### 列表组

列表组是灵活的组件，可以显示简单的元素列表，也可以与其他元素组合以创建具有自定义内容的复杂列表。我们的搜索将在用户搜索产品时将我们重定向到 SearchProductsResult 视图。此视图使用列表组来显示找到的产品项。检索数据库中产品的操作方法如下：

```csharp
public ActionResult SearchProductsResult(string query)
{
    ViewBag.SearchQuery = query;
    var results = _context.Products.Where(p => p.ProductName.Contains(query)).ToList();
    return View(results);
}
```

视图的 Razor 标记表示如下：

```cshtml
@model IEnumerable<Northwind.Data.Models.Product>
@{
    ViewBag.Title = "Product Search Results";
    Layout = "~/Views/Shared/_Layout.cshtml";
}
<div class="container">
    <div class="page-header">
        <h1>Product Results <small>Results for your search term: "@ViewBag.SearchQuery"</small></h1>
    </div>
    <ul class="list-group">
        @foreach (var item in Model)
        {
            <a href="@Url.Action("Edit", "Products", new { id = item.ProductID })" class="list-group-item">@item.ProductName <span class="badge">@item.UnitsInStock</span></a>
        }
    </ul>
</div>
```

在此标记中，产品项被加载到无序列表 `<ul>` 元素中作为锚点 `<a>` 元素。每个锚点元素的类名应设置为 `list-group-item`。视图在浏览器中应如下图所示：

