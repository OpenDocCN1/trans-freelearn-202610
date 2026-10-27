
## Using the jQuery DataTables Plugin with Bootstrap

```html
$('.table').dataTable();
});
</script>
}
```
```

- 5. In the preceding step, we've added the required references to the DataTables style sheets as well as the JavaScript files. We created a jQuery event handler, which will enable the DataTable functionality on all HTML elements with a class name of `table` as soon as the page loads.

When you run your project and navigate to the customers view, you'll see that the list of customers are automatically paginated into groups of ten, and you are able to search and sort the data inside the table, as shown in the following screenshot. The default Bootstrap styles for tables are also correctly applied:

![](img/371b9a67300a03b7b335eb8a6fe55d94_164_0.png)

## DataTables extensions

The jQuery DataTables plugin provides a wide variety of extensions, which can enhance the functionality of the plugin dramatically.

### The ColReorder extension

The ColReorder extension allows users to reorder table columns by clicking-and-dragging the column header to the location they prefer. To enable column reordering for your DataTables HTML table, complete the following steps:

- 1. Open the _Layout.cshtml file and add a reference to the dataTables.colReorder.css file:
- ```
<link rel="stylesheet" type="text/css" href="//cdn.datatables.net/colreorder/1.1.1/css/dataTables.colReorder.css">
```
- 2. Open the view .cshtml file and add a reference to the DataTables, DataTables Bootstrap, and ColReorder extension JavaScript files:
- ```
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/colreorder/1.1.1/js/dataTables.colReorder.min.js"></script>
```
- 3. Lastly, using jQuery, add an event handler to initialize the DataTables plugin and the ColReorder extension after the page has loaded:
- ```
<script type="text/javascript">
$(document).ready(function () {
$(".table").DataTable({
"dom": 'Rlfrtip'
});
});
</script>
```

4. When navigating to the page, you should now be able to drag and reorder the columns in the table. A blue line will be shown when dragging a column, as illustrated in the following screenshot:

![](img/371b9a67300a03b7b335eb8a6fe55d94_166_0.png)

> Notice that we're referencing all the style sheets and JavaScript files for the extensions from the DataTables CDN that are available at cdn.datatables.net.

### The ColVis extension

The ColVis extension adds a button to the top of DataTable, which when clicked on, displays a list of column names in the table with a checkbox next to it. The user can then deselect the column names they do not wish to see in the grid.

To enable the column visibility extension, perform the following steps:

- 1. Open the _Layout.cshtml file and add a reference to the dataTables.colVis.css file:

```
<link rel="stylesheet" type="text/css" href="//cdn.datatables.net/colvis/1.1.0/css/dataTables.colVis.css">
```

- 2. Open the view .cshtml file and add a reference to the DataTables, DataTables Bootstrap, and ColVis extension JavaScript files:

```
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/colvis/1.1.0/js/dataTables.colVis.min.js"></script>
```

- 3. Lastly, using jQuery, add an event handler to initialize the DataTables plugin and the ColVis extension after the page has loaded:

```
<script type="text/javascript">
    $(document).ready(function () {
        $('table').DataTable({
            "dom": 'C<"clear">lfrtip'
        });
    });
</script>
```

- 4. On navigating to the Customers page, you should see a button next to the search box with which you can show or hide columns in the table, as shown in the following screenshot:

![](img/371b9a67300a03b7b335eb8a6fe55d94_167_0.png)

### The TableTools extension

The TableTools extension for jQuery DataTables adds a toolbar at the top of the table with which the user can copy to clipboard, export to CSV, and print to PDF the data inside the DataTable. It is a really simple way to give your users the functionality to export their data.

To use the TableTools extension, perform the following steps:

- 1. Create a new folder in the root of your project called `swf`.
- 2. Add the `copy_csv_xls_pdf.swf` file to the `swf` folder. The `.swf` file is included in the DataTables plugin download.
- 3. Open the `_Layout.cshtml` file and add a reference to the `dataTables.tableTools.css` file:
- ```html
<link rel="stylesheet" type="text/css" href="//cdn.datatables.net/tabletools/2.2.1/css/dataTables.tableTools.css">
```
- 4. Open the view `.cshtml` file and add a reference to the DataTables, DataTables Bootstrap, and TableTools extension JavaScript files:

```html
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/1.10.0/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/tabletools/2.2.1/js/dataTables.tableTools.min.js"></script>
<script type="text/javascript" language="javascript" src="//cdn.datatables.net/plug-ins/be7019ee387/integration/bootstrap/3/dataTables.bootstrap.js"></script>
```

- 5. Lastly, using jQuery, add an event handler to initialize the DataTables plugin and the TableTools extension after the page has loaded:

```javascript
<script type="text/javascript">
    $(document).ready(function () {
        var table = $('.table').dataTable();
        var tt = new $.fn.dataTable.TableTools(table);
        $(tt.fnContainer()).insertBefore('div.dataTables_wrapper');
    });
</script>
```

6. The preceding highlighted code is used to initialize the TableTools extension and to apply Bootstrap styling to the TableTools toolbar buttons. When opening the view with DataTable, you should see a TableTools button toolbar above the DataTable, as shown in the following screenshot:

![](img/371b9a67300a03b7b335eb8a6fe55d94_169_0.png)

## Summary

In this chapter, we've implemented a sortable, searchable, and extensible HTML table using the jQuery DataTables plugin. We also explored how to specify that the plugin should use the default Bootstrap styles. This chapter should also have given you the confidence to explore other open source plugins and incorporate them in your own ASP.NET MVC projects.

In the next and final chapter, we'll take a look at the `TwitterBootstrapMVC` library and how to use it in your own project.

The fluent implementation also provides IntelliSense inside Visual Studio, enabling developers to easily discover the Bootstrap configuration options. The library is no longer open source or free, although you can still view the original project on GitHub at `https://github.com/DmitryEfimenko/TwitterBootstrapMvc`.

The commercialized version of the library is available at `https://www.twitterbootstrapmvc.com/`.

## Including TwitterBootstrapMVC in your project

You can include the `TwitterBootstrapMVC` library in your project in one of the two ways. The first and preferred option is to use the NuGet package, and the second option is to download and manually add a reference to the `.dll` file.

### Adding TwitterBootstrapMVC using NuGet

To include the `TwitterBootstrapMVC` library in your project using NuGet, perform the following steps:

- 1. In Visual Studio, open the **Package Manager Console** window, via the **Tools | Library Package Manager | Package Manager Console** menu.
- 2. Inside the **Package Manager Console** window, type the following command and hit *Enter*:
- ```
Install-Package TwitterBootstrapMVC5
```
- 3. Next, navigate to `https://www.twitterbootstrapmvc.com/Download` and download a trial or purchase a personal or commercial license.
- 4. Download the license file, it should be called `TwitterBootstrapMvcLicense.lic` and add it to the root of your project in Visual Studio.
- 5. Open the `Web.config` file located inside the Views folder and add the following code to the `<namespaces>` element:
- ```xml
<add namespace="TwitterBootstrapMVC" />
<add namespace="TwitterBootstrap3" />
```
- 6. Finally, open the `Global.asax` file in the root of your project and add the following line of code to the `Application_Start` method:
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
