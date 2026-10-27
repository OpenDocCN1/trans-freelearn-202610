

## 使用 Bootstrap 组件

> SignalR 是一个用于 ASP.NET 的库；这个库让向应用程序添加实时功能和反馈变得非常容易。你可以在 www.signalr.net 上了解更多信息。

2. 接下来，在你的项目中添加一个名为 `Hubs` 的新文件夹，并向该文件夹添加一个名为 `ProgressbarHub.cs` 的新类。将它的代码改为以下代码：

```csharp
using System.Threading;
using Microsoft.AspNet.SignalR;

namespace Northwind.Web.Hubs
{
    public class ProgressbarHub : Hub
    {
        public void SendProgress()
        {
            for (int i = 0; i <= 100; i++)
            {
                Thread.Sleep(50);
                Clients.Caller.sendMessage(i + "%");
            }
        }
    }
}
```

3. 在上述代码中，我们的类继承自 SignalR 的 Hub 类，并且只包含一个名为 `SendProgress` 的方法，该方法会递增一个变量，并通过 `sendMessage` 方法将值发送给任何 SignalR 客户端。

4. 接下来，我们添加对 SignalR 虚拟路径的引用。打开 `_Layout.cshtml` 文件，并在文件底部添加以下一行：

```html
<script src="~/signalr/hubs"></script>
```

5. 我们还要在现有的 bundle 中添加对 SignalR JavaScript 库的引用。打开位于 `App_Start` 文件夹中的 `BundleConfig.cs` 文件，并按如下方式修改 jquery bundle，使其包含 SignalR 库：

```csharp
bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
    "~/Scripts/jquery-{version}.js",
    "~/Scripts/jquery.signalR-2.0.3.js"));
```

6. 最后，在包含进度条的页面上，在 Scripts 节中添加以下脚本：

```csharp
@section scripts
{
    <script type="text/javascript" language="javascript">
        $("#start").click(function () {
            $(".progress-bar").width('0%');
            var progressNotifier = $.connection.progressbarHub;
            progressNotifier.client.sendMessage = function (message) {
                updateProgress(message);
            };

            $.connection.hub.start().done(function () {
                progressNotifier.server.sendProgress();
            });
        });

        function updateProgress(message) {
            $(".progress-bar").width(message);
            $("#progressbarTitle").html(message + ' Complete');
        }
    </script>
}
```

上述代码将在用户单击 ID 为 start 的按钮时执行。它首先会将页面上所有进度条的宽度设置为 0，并初始化与服务器的连接。然后我们设置服务器端要调用的函数名。在此场景中，JavaScript 函数名为 `updateProgress`。

最后，我们建立与服务器的连接，并启动服务器端操作。如你所见，我们调用了 `sendProgress` 函数——这是在 `ProgressbarHub` 类中声明的方法名。

执行该代码后，你会看到页面上所有进度条的值都会从 0 增加到 100。

## 总结

在本章中，我们探索了许多不同的 Bootstrap 组件，并学习了如何在你的 ASP.NET MVC 项目中使用它们。我们还了解了使用 NuGet 包来帮助你创建分页列表和动态更新进度条。

在下一章中，我们将更深入地研究 Bootstrap 组件，探讨如何使用 Bootstrap JavaScript 插件为你的网站添加更多交互性。

## 使用 Bootstrap JavaScript 插件

为了使用 Bootstrap 插件，我们需要在项目中包含 `bootstrap.js` 或 `bootstrap.min.js` 文件。该文件包含所有 Bootstrap 插件，但如果你不打算在项目中使用每一个插件，最好自定义 `bootstrap.js` 文件中要包含的组件。这将减小 Bootstrap 库的体积，并让网站加载得更快。要访问这些插件，请访问 http://getbootstrap.com/customize/ 并选择你将在项目中使用的插件。

### 数据属性与编程式 API

Bootstrap 让你可以完全通过 HTML 标记来使用其插件。这意味着，使用大多数插件时，你不需要编写一行 JavaScript。使用数据属性是推荐的方法，并且应该是你使用 Bootstrap 插件时的首选。

例如，要让 `alert` 元素可以关闭，你需要将 `data-dismiss="alert"` 属性添加到 `button` 或 `anchor` 元素上，如下面的代码所示：

```html
<div class="alert alert-danger">
  <button data-dismiss="alert" class="close" type="button">×</button>
  <strong>Warning</strong> Shuttle launch in t-minus 10 seconds.
</div>
```

你也可以选择通过 JavaScript 使用编程式 API 执行相同的操作。下面的代码在用户单击按钮时使用 jQuery 关闭特定的 `alert` 元素：

```html
<button class="close" type="button" onclick="$('#myalert').alert('close')">×</button>
```

### 级联下拉菜单

你可以使用 dropdown 插件为几乎任何 Bootstrap 组件添加下拉菜单。级联下拉菜单是一种根据另一个下拉菜单中所选值来更新其数据的下拉菜单。要添加级联下拉菜单，请执行以下步骤：

1. 我们将为两个按钮添加下拉菜单，以列出从数据库中检索到的地区（Region）和区域（Territory）数据。当用户选择一个地区（Region）时，区域（Territory）下拉列表将会更新，仅显示与所选地区关联的那些区域（Territory）。下面的截图展示了最终结果：

![](img/371b9a67300a03b7b335eb8a6fe55d94_89_0.png)

2. 这两个下拉菜单将用于我们的 Edit Customer 视图。为了让区域（Territory）和地区（Region）列表可以在视图中访问，我们首先使用以下代码将它们添加到 ViewBag 对象：

```csharp
public ActionResult Edit(int id)
{
    var model = _context.Customers.First(c => c.CustomerId == id);
    ViewBag.RegionId = new SelectList(_context.Regions,
    "RegionId", "RegionDescription", model.RegionId);
    ViewBag.TerritoryId = new SelectList(_context.Territories.Where(t => t.RegionId == model.RegionId), "TerritoryId",
    "TerritoryDescription", model.TerritoryId);
    return View(model);
}
```

在这段代码中，我们首先从数据库中加载了所请求的客户，然后将数据库中的地区（Region）和区域（Territory）列表添加到 ViewBag 对象。你会注意到，我们只检索了所选客户所在地区（Region）内的区域（Territory）。

3. 接下来，我们向视图添加 Razor 标记，以创建两个带有下拉菜单的按钮。在下面的代码中，我们将创建两个新的 Bootstrap 表单组，其中包含地区（Region）和区域（Territory）属性的标签，以及用于携带所选地区 ID 和区域 ID 的隐藏元素。我们遍历通过 ViewBag 对象传递给视图的地区和区域，并构建菜单项。

```html
<div class="form-group">
    @Html.LabelFor(model => model.RegionId,
        new { @class = "control-label col-md-2" })
    @Html.HiddenFor(model => model.RegionId)
    <div class="col-md-10">
        <div id="regiondropdown" class="dropdown">
            <button id="regionbutton" class="btn btn-sm btn-info"
                    data-toggle="dropdown">@Model.Region.RegionDescription</button>
            <ul class="dropdown-menu">
                @foreach (var item in ViewBag.RegionId)
                {
                    <li><a href="#" tabindex="-1" data-value="@item.Value">@item.Text</a></li>
                }
            </ul>
        </div>
    </div>
</div>
<div class="form-group">
    @Html.LabelFor(model => model.TerritoryId, new { @class =
        "control-label col-md-2" })
    @Html.HiddenFor(model => model.TerritoryId)
    <div class="col-md-10">
        <div id="territorydropdown" class="dropdown">
            <button id="territorybutton" class="btn btn-sm btn-info" data-toggle="dropdown">@Model.Territory.TerritoryDescription</button>
            <ul id="territories" class="dropdown-menu">
                @foreach (var item in ViewBag.TerritoryId)
                {
                    <li><a href="#" tabindex="-1" data-value="@item.Value">@item.Text</a></li>
                }
            </ul>
        </div>
    </div>
</div>
```

