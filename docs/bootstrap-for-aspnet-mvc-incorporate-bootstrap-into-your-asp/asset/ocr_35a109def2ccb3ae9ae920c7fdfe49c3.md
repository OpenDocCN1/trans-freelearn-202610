+   5. Add a reference to the metisMenu plugin by adding the following line just above the </body> closing element:
+   <script src="@Url.Content("~/js/plugins/metisMenu/jquery.metisMenu.js")"></script>
+   6. When running your project, the left-hand side menu should now display correctly.

# Adding different page views

The SB Admin template comes with a variety of different page styles, which can be used to build our own views. We'll create a custom view that displays two panels, one with a simple Bootstrap form and another one that displays an image, by completing the following steps:

+   1. Add a new empty MVC 5 controller called FormsController to your project.
+   2. Add a new empty view for the Index method of the controller and select the layout page we've created earlier as its layout page.
+   3. Open the Index.cshtml file inside the Views\Forms folder.
+   4. Add the following markup to the view:

```
@{
    ViewBag.Title = "Index";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<div class="row">
    <div class="col-lg-12">
        <h1 class="page-header">Forms</h1>
    </div>
</div>
<div class="row">
    <div class="col-lg-6">
        <div class="panel panel-primary">
            <div class="panel-heading">
                Firs Panel with simple form
            </div>
            <div class="panel-body">
                <div class="row">
                    <div class="col-lg-6">
                        <form role="form">
```