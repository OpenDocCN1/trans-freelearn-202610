
The first code block is `<#= #>` and is used to execute the code within the tag and will return a text result. For example, in the following markup, the code will return the value in the `ViewName` variable inside the T4 template:

```
<#= ViewName #>
```

The `<# #>` code block executes the code inside the tag and returns a void result. For example, the following markup loops through every item in a collection and produces the item's `Total` property followed by a carriage return:

```
<#foreach(var item in Orders) { #>
    <#= item.Total + Environment.NewLine #>
<# } #>
```

The `<#+ #>` code block is used to define reusable methods, which can be called from inside the T4 template. For example, the following markup defines a method called `TypeName` and returns the full name of the object:

```
<#+ public string TypeName(object obj) { return obj.GetType().FullName; } #>
```

To use the method, you simply need to call it inside your template file, as illustrated in the following code:

```
<#= TypeName(myObj) #>
```

## Customizing the generated code for controllers

When adding a new controller class to your ASP.NET MVC project, the Add New Scaffolded Item dialog is shown, and you have a choice between three different types of controllers to add:

- MVC 5 Controller – Empty
- MVC 5 Controller with read/write actions
- MVC 5 Controller with views, using Entity Framework

Each of these items generates boilerplate code, which you can customize afterwards. However, let's assume that all our projects' controller classes need a constructor that accepts a parameter. Using the standard items, we would have to manually add the constructor for each controller in our project.

Fortunately, we are able to override the generated output for the controllers, because in essence, they are nothing more than T4 templates.

To customize the output of the MVC 5 Controller – Empty item, perform the following steps:

1. Right-click on your ASP.NET MVC project's name inside Visual Studio, and navigate to Add | New Item.
2. Navigate to Visual C# | Web | SideWaffle and select the ASP.NET Scaffolding T4 files item template and click on Add:

    ![](img/371b9a67300a03b7b335eb8a6fe55d94_123_0.png)

## Creating T4 Templates to Scaffold Bootstrap Views

3. The item template will add a folder called `CodeTemplates` inside your project. Inside this folder, it will create 13 subfolders that contain `.t4` files, as shown in the following screenshot:

    ![](img/371b9a67300a03b7b335eb8a6fe55d94_124_0.png)

4. Double-click on the `Controller.cs.t4` file inside the `MvcControllerEmpty` folder.

5. The T4 template for the empty MVC 5 controller will open inside Visual Studio. You’ll notice that it contains a combination of C# code and T4 markup.

6. Keep the markup in the template as is, but add the following code above the `Index` method:

    ```
    private readonly ApplicationDbContext _context;
    private readonly ICurrentUser _currentUser;

    public <#= ControllerName #>(ApplicationDbContext context,
    ICurrentUser currentUser)
    {
        _context = context;
        _currentUser = currentUser;
    }
    ```

```
private readonly ICurrentUser _currentUser;

public SuppliersController(ApplicationDbContext context, ICurrentUser currentUser)
{
    _context = context;
    _currentUser = currentUser;
}

//
// GET: /Suppliers/
public ActionResult Index()
{
    return View();
}
}
```

## Customizing the generated code for views

As with controllers, the scaffolding for views can also be customized. In our example project used throughout this book, our views followed a consistent look. Each page has a page header followed by a breadcrumb component to indicate to the user which page is currently being viewed.

We always want to follow this design when adding a new page to our site, and one method of enforcing this is to either override the default scaffolder for views or add our own. We'll create a T4 template that will generate a standard, vertical Bootstrap form that already contains a page header and a breadcrumb component. To accomplish this, perform the following steps:

1. Open the `MvcView` folder inside the `CodeTemplates` folder.
2. Right-click on the `Create.cs.t4` file and select Copy.
3. Right-click on the `MvcView` folder and select Paste.
4. The file will be copied as `Create.cs - Copy.t4`; rename it to `VerticalForm-Bootstrap.cs.t4`.
5. Double-click on `VerticalForm-Bootstrap.cs.t4` to open it.
6. Add the following HTML markup before the `@using (Html.BeginForm())` line:

    ```
    <div class="container">
        <div class="page-header">
            <h1><#= ViewDataTypeShortName #> </h1>
        </div>
    <ol class="breadcrumb">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li class="active"><#= ViewDataTypeShortName #></li>
    </ol>

    <div class="row clearfix">
        <div class="col-md-12">
    ```

7. The preceding markup will render a page header and a breadcrumb component above the form. The form will reside within a grid column, inside a Bootstrap row's `<div>` element. Note that we use the `ViewDataTypeShortName` variable, which contains the short name of the object used as the model for the view.
8. Next, remove the `<div class="form-horizontal">` line as well as the `<h4>` and `<hr>` elements in the line below `<div>`.
9. Remove all `<div class="form-group">` and `<div class="col-md-10">` elements from the template. Make sure that you also remove all their closing `</div>` tags.
10. Remove the `new { @class = "control-label col-md-2" }` HTML attribute settings from all the `@Html.LabelFor` methods.
11. Lastly, we need to close all the `<div>` elements that were added to the top of the template. Add three closing `</div>` tags after the last `@Html.ActionLink` line.

With the template markup in place, we can create a new scaffolded view based on this by completing the following steps:

1. Open a controller class. In this example, we'll use the `SuppliersController.cs` file.
2. Right-click inside the Index method and select Add View...:

    ![](img/371b9a67300a03b7b335eb8a6fe55d94_127_0.png)

3. This will display the **Add View** dialog.

4. Enter a view name and select the **VerticalForm-Bootstrap** template from the list of templates in the **Template** combobox. Any custom T4 template that you'll create will automatically be listed inside the templates list.

5. Select your model class. In this example, we'll use the **Supplier** model.

6. The **Data context class** field should be automatically selected; if not, select it from the list of data context classes. Click on **Add**:

    ![](img/371b9a67300a03b7b335eb8a6fe55d94_128_0.png)

7. Visual Studio will use our custom template to scaffold a view, which will contain all the markup we've added earlier.

8. The result should be a vertical or standard Bootstrap view, as illustrated in the following screenshot:

    ![](img/371b9a67300a03b7b335eb8a6fe55d94_129_0.png)

## Creating a custom scaffolder extension

If you need more control on how the code for your views or controllers is scaffolded, you need to create a new scaffolding extension for Visual Studio. This extensibility enables developers to add their own scaffolding items to the Add Scaffold dialog window, as illustrated in the following screenshot:

![](img/371b9a67300a03b7b335eb8a6fe55d94_130_0.png)

To create such a custom scaffolder, we'll create a new basic scaffolder project. This project template is installed by the SideWaffle extension and adds all the files necessary to create a custom scaffolder.

Perform the following steps to create your own custom scaffolder project:

1. Create a new project inside Visual Studio.
2. Select the Basic Scaffolder project template by navigating to Extensibility | SideWaffle, as shown in the following screenshot:

    ![](img/371b9a67300a03b7b335eb8a6fe55d94_131_0.png)

3. The project template will create two new projects: BootstrapBasicForm and BootstrapBasicFormExtension. One contains the code for the extension and another contains the .vsixmanifest file for the project:

    ![](img/371b9a67300a03b7b335eb8a6fe55d94_131_1.png)
