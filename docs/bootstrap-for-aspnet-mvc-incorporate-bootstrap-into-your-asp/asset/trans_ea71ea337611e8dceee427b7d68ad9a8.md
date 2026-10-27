

## 使用 jQuery DataTables 插件与 Bootstrap

```html
$('.table').dataTable();
});
</script>
}
```
```

- 5. 在前一步中，我们添加了对 DataTables 样式表以及 JavaScript 文件的必要引用。我们创建了一个 jQuery 事件处理程序，当页面加载后，它将对所有类名为 `table` 的 HTML 元素启用 DataTable 功能。

当你运行项目并导航到 customers 视图时，你会看到客户列表会自动分页为每组十条，并且你可以搜索和排序表格内的数据，如下面的截图所示。Bootstrap 默认的表格样式也会被正确应用：

![](img/371b9a67300a03b7b335eb8a6fe55d94_164_0.png)

## DataTables 扩展

jQuery DataTables 插件提供了多种多样的扩展，可以显著增强插件的功能。

### ColReorder 扩展

ColReorder 扩展允许用户通过点击并将列标题拖拽到他们喜欢的位置来重新排列表列。要为你的 DataTables HTML 表格启用列重新排序，请完成以下步骤：

- 1. 打开 _Layout.cshtml 文件，并添加对 dataTables.colReorder.css 文件的引用：
- ```
<link rel="stylesheet" type="text/css" href="//cdn.datatables.net/colreorder/1.1.1/css/dataTables.colReorder.css">
```
- 2. 打开视图的 .cshtml 文件，并添加对 DataTables、DataTables Bootstrap 和 ColReorder 扩展 JavaScript 文件的引用：
- ```
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/colreorder/1.1.1/js/dataTables.colReorder.min.js"></script>
```
- 3. 最后，使用 jQuery 添加一个事件处理程序，在页面加载完成后初始化 DataTables 插件和 ColReorder 扩展：
- ```
<script type="text/javascript">
$(document).ready(function () {
$(".table").DataTable({
"dom": 'Rlfrtip'
});
});
</script>
```

4. 当导航到该页面时，你现在应该能够拖拽并重新排列表中的列。拖拽列时会显示一条蓝线，如下图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_166_0.png)

> 注意，我们从 DataTables CDN（可在 cdn.datatables.net 获取）引用了所有扩展所需的样式表和 JavaScript 文件。

### ColVis 扩展

ColVis 扩展会在 DataTable 顶部添加一个按钮，点击该按钮时，会显示表格中列名的列表，每列旁边有一个复选框。然后，用户可以取消选择他们不想在网格中看到的列名。

要启用列可见性扩展，请执行以下步骤：

- 1. 打开 _Layout.cshtml 文件，并添加对 dataTables.colVis.css 文件的引用：

```
<link rel="stylesheet" type="text/css" href="//cdn.datatables.net/colvis/1.1.0/css/dataTables.colVis.css">
```

- 2. 打开视图的 .cshtml 文件，并添加对 DataTables、DataTables Bootstrap 和 ColVis 扩展 JavaScript 文件的引用：

```
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/colvis/1.1.0/js/dataTables.colVis.min.js"></script>
```

- 3. 最后，使用 jQuery 添加一个事件处理程序，在页面加载完成后初始化 DataTables 插件和 ColVis 扩展：

```
<script type="text/javascript">
    $(document).ready(function () {
        $('table').DataTable({
            "dom": 'C<"clear">lfrtip'
        });
    });
</script>
```

- 4. 导航到 Customers 页面时，你应该会在搜索框旁边看到一个按钮，通过该按钮可以显示或隐藏表格中的列，如下面的截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_167_0.png)

### TableTools 扩展

jQuery DataTables 的 TableTools 扩展在表格顶部添加了一个工具栏，用户可以通过它复制到剪贴板、导出为 CSV 以及将 DataTable 中的数据打印为 PDF。这是为你的用户提供数据导出功能的一种非常简单的方法。

要使用 TableTools 扩展，请执行以下步骤：

- 1. 在项目根目录下创建一个名为 `swf` 的新文件夹。
- 2. 将 `copy_csv_xls_pdf.swf` 文件添加到 `swf` 文件夹中。该 `.swf` 文件包含在 DataTables 插件下载中。
- 3. 打开 `_Layout.cshtml` 文件，并添加对 `dataTables.tableTools.css` 文件的引用：
- ```html
<link rel="stylesheet" type="text/css" href="//cdn.datatables.net/tabletools/2.2.1/css/dataTables.tableTools.css">
```
- 4. 打开视图的 `.cshtml` 文件，并添加对 DataTables、DataTables Bootstrap 和 TableTools 扩展 JavaScript 文件的引用：

```html
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/tabletools/2.2.1/js/dataTables.tableTools.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js"></script>
```

- 5. 最后，使用 jQuery 添加一个事件处理程序，在页面加载完成后初始化 DataTables 插件和 TableTools 扩展：

```javascript
<script type="text/javascript">
    $(document).ready(function () {
        var table = $('.table').dataTable();
        var tt = new $.fn.dataTable.TableTools(table);
        $(tt.fnContainer()).insertBefore('div.dataTables_wrapper');
    });
</script>
```

6. 上面突出显示的代码用于初始化 TableTools 扩展，并将 Bootstrap 样式应用于 TableTools 工具栏按钮。当打开带有 DataTable 的视图时，你应该会在 DataTable 上方看到 TableTools 按钮工具栏，如下面的截图所示：

![](img/371b9a67300a03b7b335eb8a6fe55d94_169_0.png)

## 总结

在本章中，我们使用 jQuery DataTables 插件实现了一个可排序、可搜索且可扩展的 HTML 表格。我们还探讨了如何指定插件使用默认的 Bootstrap 样式。本章还应让你有信心探索其他开源插件，并将它们整合到你自己的 ASP.NET MVC 项目中。

在下一章也是最后一章中，我们将了解 `TwitterBootstrapMVC` 库，以及如何在你自己项目中使用它。

流式实现还在 Visual Studio 中提供了 IntelliSense，使开发人员能够轻松发现 Bootstrap 配置选项。该库已不再是开源或免费的，但你仍然可以在 GitHub 上查看原始项目：`https://github.com/DmitryEfimenko/TwitterBootstrapMvc`。

该库的商业版本可在 `https://www.twitterbootstrapmvc.com/` 获取。

## 在项目中引入 TwitterBootstrapMVC

你可以通过以下两种方式之一在项目中引入 `TwitterBootstrapMVC` 库。第一种也是推荐的方式是使用 NuGet 包，第二种方式是下载 `.dll` 文件并手动添加引用。

### 使用 NuGet 添加 TwitterBootstrapMVC

要使用 NuGet 在项目中引入 `TwitterBootstrapMVC` 库，请执行以下步骤：

- 1. 在 Visual Studio 中，通过**工具 | 库程序包管理器 | 程序包管理器控制台**菜单打开**程序包管理器控制台**窗口。
- 2. 在**程序包管理器控制台**窗口中，输入以下命令并按 *回车*：
- ```
Install-Package TwitterBootstrapMVC5
```
- 3. 接下来，导航到 `https://www.twitterbootstrapmvc.com/Download`，下载试用版或购买个人或商业许可证。
- 4. 下载许可证文件，该文件应名为 `TwitterBootstrapMvcLicense.lic`，并将其添加到 Visual Studio 中项目的根目录。
- 5. 打开 Views 文件夹中的 `Web.config` 文件，并将以下代码添加到 `<namespaces>` 元素中：
- ```xml
<add namespace="TwitterBootstrapMVC" />
<add namespace="TwitterBootstrap3" />
```
- 6. 最后，打开项目根目录中的 `Global.asax` 文件，并将以下代码行添加到 `Application_Start` 方法中：
- ```csharp
Bootstrap.Configure();
```

```
Install-Package TwitterBootstrapMVC5
```

```
<add namespace="TwitterBootstrapMVC" />
<add namespace="TwitterBootstrap3" />
```

```
Bootstrap.Configure();
```

