# # Buttons and links

The `TwitterBootstrapMVC` library provides fluent helpers for Bootstrap buttons as well as links. Both buttons and links helpers share the following extension methods:

| Method name | Description |
|---|---|
| `.AppendIcon()` | This appends the specified icon to the button or link |
| `.PrependIcon()` | This prepends the specified icon to the button or link |
| `.Disabled()` | This adds the `disabled` class to the button or link |

In the following code example, we'll create a simple submit button that has an icon at the back and at the front of it. Note the use of the `glyphicon` class names in order to specify which icons to use.

```
```csharp
@Html.Bootstrap().Button().AppendIcon("glyphicon glyphicon-pencil").
    PrependIcon("glyphicon glyphicon-music")
```
```

The preceding markup will render the following screenshot in your browser:

![](bbox=[0.38, 0.441, 0.621, 0.515])

The `ActionLinkButton`, `Button`, and `SubmitButton` helpers share the following extension methods:

| Method name | Description |
|---|---|
| `.Text()` | This sets the caption/text of the button. |
| `.Name()` | This specifies the HTML name attribute. |
| `.Value()` | This sets the value HTML attribute. |
| `.Size()` | This specifies the size of the button. This also accepts the `ButtonSize` enum as a parameter. |
| `.Style()` | This sets the style of the button. This also accepts the `ButtonStyle` enum as a parameter. |
| `.ButtonBlock()` | This expands the button to the full width of the container. |