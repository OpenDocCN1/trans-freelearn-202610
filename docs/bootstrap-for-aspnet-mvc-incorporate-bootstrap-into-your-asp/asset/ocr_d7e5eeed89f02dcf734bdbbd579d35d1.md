5. In the **Add View** dialog, leave all the fields at their default values and select the layout page we've added earlier. Click on **Add** to continue, as shown in the following screenshot:

![](bbox=[0.23, 0.207, 0.77, 0.486])

6. An empty view will be generated and you'll notice the `ViewBag.Title` property as well as the layout page to use for this view have been added at the top of the view. Consider the following example:

```
```cshtml
@{
    ViewBag.Title = "Index";
    Layout = "~/Views/Shared/_Layout.cshtml";
}
```
```

There will also be a single `<h2>` element in the view that you can leave as is. If you run your project, you should see the view displayed with the default Bootstrap styling applied to the `<h2>` element.