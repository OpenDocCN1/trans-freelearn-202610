
## Bootstrap tables

Bootstrap provides a default styling for HTML tables with a few options to customize their layout and behaviors. The default ASP.NET MVC scaffolding automatically adds the table class names to the table element when generating a list view.

To see this in action, complete the following steps:

- 1. Create a new view model class in our project's Models folder called `ProductViewModel.cs`. This class will contain six properties and its code is listed as follows:

```csharp
public class ProductViewModel
{
    public int ProductID { get; set; }
    public string ProductName { get; set; }
    public decimal? UnitPrice { get; set; }
    public int? UnitsInStock { get; set; }
    public bool Discontinued { get; set; }
    public string Status { get; set; }
}
```

- 2. Next, we'll add a new empty ProductsController class and only add one action result called Index to it:

```csharp
public class ProductsController : Controller
{
    private readonly ApplicationDbContext _context;
    private readonly ICurrentUser _currentUser;

    public ProductsController(ApplicationDbContext context,
    ICurrentUser currentUser)
    {
        _context = context;
        _currentUser = currentUser;
    }

    public ActionResult Index()
    {
        var models = _context.Products.Project().
        To<ProductViewModel>().ToArray();
        return View(models);
    }
}
```

The preceding code uses a dependency injection to get a reference to the Entity Framework `DbContext` object. The Index method then retrieves a collection of products from the database and maps it to the `ProductViewModel` object using AutoMapper. The list is then passed to the view.

> We're using `StructureMap` to perform our project's dependency injection and inversion of control. We're also using a tool called AutoMapper to automatically map our domain models to our project's view models. Both libraries are available via NuGet, and you can read more about each on their respective websites at `www.structuremap.net` and `www.automapper.org`.

Using AutoMapper, we create a conditional mapping to the Status property of the `ProductViewModel` class. If the product is discontinued, we'll set the Status property to danger; if the UnitPrice is more than 50, the Status property is set to info; and if the UnitsInStock property is less than 20, the Status property is set to warning. The code that performs this logic looks like the following:

```csharp
Mapper.CreateMap<Product, ProductViewModel>()
    .ForMember(dest => dest.Status,
        opt => opt.MapFrom
            (src => src.Discontinued ? "danger" : src.UnitPrice > 50 ?
                "info" : src.UnitsInStock < 20 ? "warning" : ""));
```

To generate a view that includes a Bootstrap table, complete the following steps:

- 1. Right-click inside the `Index` method and select **Add View...** from the context menu. In the **Add View** dialog window, select **List** from the **Template** drop-down menu and the `ProductViewModel` class from the **Model class** drop-down menu.

- 2. Click on the **Add** button to create the new view, as shown in the following screenshot:

![](img/371b9a67300a03b7b335eb8a6fe55d94_46_0.png)

- 3. The default ASP.NET MVC scaffolding will generate a basic `<table>` element with a class name, `table`. We'll change the default-generated markup by adding a table head element (`<thead>`), which will be used to define the column headings for the table.

- 4. This element will be followed by a table body element (`<tbody>`). The table body element will contain the actual row data. The resulting markup will look like the following code:

```html
<table class="table">
    <thead>
        <tr>
            <th>
                Product Name
            </th>
            <th>
                Unit Price
            </th>
            <th>
                Units In Stock
            </th>
            <th>
                Discontinued
            </th>
            <th></th>
        </tr>
    </thead>
    <tbody>
    @foreach (var item in Model)
    {
        <tr class="@item.Status">
            <td>
                @Html.DisplayFor(modelItem => item.ProductName)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.UnitPrice)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.UnitsInStock)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Discontinued)
            </td>
            <td>
                @Html.ActionLink("Edit", "Edit", new { /* id=item.PrimaryKey */ }) |
                @Html.ActionLink("Details", "Details", new { /* id=item.PrimaryKey */ }) |
                @Html.ActionLink("Delete", "Delete", new { /* id=item.PrimaryKey */ })
            </td>
        </tr>
    }
    </tbody>
</table>
```

In the preceding markup, note that the `<table>` element's class name is set to table. When you run the project and open this view, your table should be styled using the Bootstrap table styles as illustrated in the following screenshot:

![](img/371b9a67300a03b7b335eb8a6fe55d94_48_0.png)

### Styling Bootstrap tables

Bootstrap provides additional classes with which you can style your tables even more. To create a bordered table, add `table-bordered` to its class name as follows:

```html
<table class="table table-bordered">
```

To create a table where each odd row is highlighted with another color than the base color, change the table's class name to `table table-striped` as follows:

```html
<table class="table table-striped">
```

Lastly, Bootstrap also gives you the option to enable the hover state on a table. This means the row that the user hovers their cursor over will be highlighted. To accomplish this, change the table class to `table table-hover` as follows:

```html
<table class="table table-hover">
```

All the different class names can be combined to create a zebra-striped, bordered table with hovering, as illustrated in the following markup:

```html
<table class="table table-striped table-bordered table-hover">
```

The result will look similar to the following screenshot in your browser:

![](img/371b9a67300a03b7b335eb8a6fe55d94_49_0.png)

### Bootstrap contextual table classes

Bootstrap provides additional classes with which you can style either your table's rows or cells. Adding one of the following classes to either the `<td>` or `<tr>` element of your HTML table will highlight it in either gray, green, blue, orange, or red, which respectively represents the following:

- Active
- Success
- Info
- Warning
- Danger

Of course, we can also apply these styles dynamically in our ASP.NET MVC views. In our ProductViewModel view model, we have a Status property, which can be one of the five contextual Bootstrap classes. By setting the `<tr>` element's class to this property, we can dynamically change the color of the rows in the table, based on their data as illustrated in the following markup:

```cshtml
<tbody>
    @foreach (var item in Model)
    {
        <tr class="@item.Status">
            <td>
                @Html.DisplayFor(modelItem => item.ProductName)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.UnitPrice)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.UnitsInStock)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Discontinued)
            </td>
            <td>
                @Html.ActionLink("Edit", "Edit", new { /* id=item.PrimaryKey */ })
                @Html.ActionLink("Details", "Details", new { /* id=item.PrimaryKey */ })
                @Html.ActionLink("Delete", "Delete", new { /* id=item.PrimaryKey */ })
            </td>
        </tr>
    }
</tbody>
```

In the preceding code, you will notice how the table row's class was set to `@item.Status`. This will cause the rows to be highlighted based on the mapping we've specified for the Status property, as illustrated in the following screenshot:
