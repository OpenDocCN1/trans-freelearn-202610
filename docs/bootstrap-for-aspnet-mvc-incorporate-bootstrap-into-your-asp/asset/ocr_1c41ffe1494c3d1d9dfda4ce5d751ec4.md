# Using Bootstrap Components

The following code inside the `_Layout.cshtml` file creates a fixed-top navigation bar, which is also responsive. When the site is opened on a device with a smaller screen size, it will display a button with three bars, and when you click on it, it will show the menu vertically:

```
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
```

Note that we are using two partial views to generate the rest of the navigation bar based on whether the current user is logged in or not. We'll add a global search functionality to the navigation bar when the user is logged in; this will return results based on the page the user is currently visiting.

First, add the following code to the `_BackendMenuPartial` view; this will generate a search box inside the navigation bar:

```
```cshtml
@using (Html.BeginForm("Index", "Search", FormMethod.Post, new {@class = "navbar-form navbar-left", role = "search"}))
{
    <div class="form-group">
        @Html.TextBox("searchquery", "", new { @id="searchquery", @class="form-control input-sm", placeholder="Search" })
        @Html.Hidden("fromcontroller", @ViewContext.RouteData.Values["controller"], new { @id = "fromcontroller" })
    </div>
    <button type="submit" class="btn btn-default btn-xs">GO</button>
}
```
```