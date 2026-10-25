The following highlighted sections in Views\Shared\_Layout.cshtml should be modified with the desired application name:

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,
    initial-scale=1.0" />
<title>@ViewData["Title"] - MasteringEF Core.Web</title>
...
</head>
<body>
<nav class="navbar navbar-inverse navbar-fixed-top">
<div class="container">
<div class="navbar-header">
...
<a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">MasteringEF Core.Web</a>
...
<div class="container body-content">
...
<footer>
<p>&copy; 2017 - MasteringEF Core.Web</p>
</footer>
...
</body>
```

We have created a .NET Core web application with no authentication and explored the structure of the project, which might help us understand MVC in .NET Core. If we expand the dependencies, it is evident that we don't have built-in support for Entity Framework (EF) Core. We will look at the different ways of identifying and installing the packages.