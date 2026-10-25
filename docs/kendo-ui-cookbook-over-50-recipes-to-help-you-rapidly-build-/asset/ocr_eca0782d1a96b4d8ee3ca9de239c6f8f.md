# # Kendo UI Mobile Framework

```
```html
<footer data-role="footer">
  <div data-role="tabstrip">
    <a data-icon="about">About</a>
    <a data-icon="settings">Settings</a>
  </div>
</footer>
</div>
```
```

As mentioned earlier, a widget is created by assigning appropriate values to the `data-role` attribute. In this case, the `header`, `navbar`, `footer`, and `tabstrip` values are assigned to the `data-role` attribute of various elements.

Once a layout is added to the page, the next step would be to add a view. A view widget represents a single view that needs to be shown when the application is rendered. There can be multiple views in the page and only one view can be rendered at any given point, depending on the state of the application:

```
```html
<div data-role="view" data-layout="defaultLayout">
  Hello Kendo!!
</div>
```
```

Here, apart from the `data-role` attribute, the `div` element also has a `data-layout` attribute. The `data-layout` attribute is used to associate the view widget with a layout widget in the page. In this case, the value of the `data-layout` attribute is `defaultLayout`, which is the value of the `data-id` attribute of the `layout` widget. The `header` and `footer` widgets defined in `layout` are positioned accordingly, and the View widget is rendered in the available space inside the layout. The last step would be to initialize the application. To initialize it, include the following code snippet:

```
```html
<script>
var app = new kendo.mobile.Application();
</script>
```
```

The preceding code snippet will initialize all the widgets in the page and make associations between the layout and views. The Application object can then be used to route between multiple views in the page, and we will visit this scenario in the upcoming recipes.