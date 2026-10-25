# # How it works...

When you render the page, you would see that `layout2` is used when rendering `view2`.

![](bbox=[0.357, 0.161, 0.664, 0.525])

If you specify the `data-layout` attribute for the view element, then the view would be rendered with that layout instead of the one mentioned when initializing the application.

# # There's more...

In the previous recipe, it was mentioned that the `data-platform` attribute is used to specify the platform for which the layout is built. In many scenarios, you might like the same look and feel to appear in all the platforms. For example, if you are using an iOS device and would like to force the look and feel of Android, then you can specify the attribute platform when initializing the application:

```
```html
<script>
var app = new kendo.mobile.Application(document.body, {
    layout: 'layout2',
    initial: 'view2',
});
</script>
```
```