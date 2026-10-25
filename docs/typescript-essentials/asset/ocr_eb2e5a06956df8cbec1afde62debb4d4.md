displayed by the button. The IColorSelectionControl interface extends IUserControl in the same way that the IToolSelectionControl interface does, but the additional properties here are very different. This interface defines a change method that will be called when the color input change event fires. The final property of this interface is very similar to the observable property on the IToolSelectionControl interface. The primary difference between the two is that they store different types; in this case, we store the string value that represents the current color. Due to the close relationship between these two interfaces, we abstracted the IUpdateObservable interface out and then provided a more specific type definition experience within the more finely-grained interfaces.

Now that we have our interfaces defined, we can build our HTML templates to represent the different controls we wish to create. The first template will contain only a button element that has bindings of its own applied to it. The second template is the color picker with a different set of bindings that is unique to this element type. The final thing we must do is modify the area of our HTML that currently holds our controls. As you can see, the resulting HTML has no direct controls defined, but as controls are added to an observable list called userControls, they will appear in our application:

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<title>TypeScript HTML App</title>
<link rel="stylesheet" href="app.css" type="text/css" />
<link rel="stylesheet" href="DrawingApplication.css" type="text/css" />
<script src="Scripts/jquery-2.1.1.js"></script>
<script src="Scripts/knockout-3.1.0.js"></script>
<script data-main="app" src="Scripts/require.js"></script>
</head>
<body>
<h1>Drawing Application</h1>
<div id="content">
<div data-bind="foreach: userControls">
<span data-bind="attr: { id: id }, template: { name: templateName }"></span>
</div>
<canvas id="drawingCanvas" height="400" width="570" />
</div>
</body>
</html>
<script type="text/html" id="ButtonTemplate">
<button class="tool-select-button" data-bind="text: buttonText, click: click, css: { 'selected': isSelected }" />
</script>
<script type="text/html" id="ColorPickerTemplate">
<input type="color" data-bind="event: { change: change }" />
</script>
```

As you can see, we use the foreach binding to create a new control for each object in the UserControls list that our HTML will be bound to. Inside this, there is a span element that is bound to the IUserControl interface. Our canvas object will remain unchanged for now, but there are some new elements at the bottom of the page. These new script tags will contain our HTML templates that the user controls will bind to. The button template