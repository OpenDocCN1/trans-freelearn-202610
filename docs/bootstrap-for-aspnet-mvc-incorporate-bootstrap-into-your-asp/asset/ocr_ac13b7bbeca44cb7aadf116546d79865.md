*Using Bootstrap Components*

This markup will generate the following screenshot in your browser:

![](bbox=[0.257, 0.176, 0.744, 0.231])

# # Breadcrumb

Breadcrumb is a common metaphor used in web design to indicate to the user what their current location is inside a navigation tree. It is similar to a file path in Windows Explorer. Breadcrumb is ideal for a site with many subnavigation levels and allows the user to navigate between the varied parent and child pages.

In the following markup, we'll use a combination of Razor and HTML to build a breadcrumb component with which the user can navigate back to the home page or the Manage page:

```
```html
<ol class="breadcrumb">
    <li>@Html.ActionLink("Home", "Index", "Home")</li>
    <li>@Html.ActionLink("Manage", "Index", "Manage")</li>
    <li class="active">Categories</li>
</ol>
```
```

The preceding markup contains an ordered list <ol> element with a class name of breadcrumb. Each child element of the breadcrumb is added as a list item <li> element. To indicate to the user that the last level of the breadcrumb is the active page, we set its <li> element's class name to active. The result of the previous code will look like the following screenshot when visiting the page:

![](bbox=[0.178, 0.604, 0.824, 0.65])

# # Pagination

Pagination is used to divide content, usually lists, into separate pages. For example, when scaffolding a list view, the default scaffolding template generates a table that contains a row for each item in the collection you pass into the view. This is fine for small amounts of data, but if the list contains hundreds of items, your page will take a very long time to load. Ideally, we would like to split our list view into a manageable view that has five items per page. In the following screenshot, we've changed the **Products** page's table to use pagination: