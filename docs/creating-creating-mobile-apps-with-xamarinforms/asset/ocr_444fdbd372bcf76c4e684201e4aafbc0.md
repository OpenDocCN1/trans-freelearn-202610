```
{ // Remove topmost Label from StackLayout loggerLayout.Children.RemoveAt(0); } // Enable "Remove" button only if children are present. removeButton.IsEnabled = loggerLayout.Children.Count > 0; }
```

The two buttons are given HorizontalOptions values of CenterAndExpand so that they can be nicely arranged on the screen using a horizontal StackLayout.

Notice that when the Clicked handler detects removeButton, it simply calls the RemoveAt method on the Children property. What happens if there are no children?

It can’t happen in this program! When the application begins, the IsEnabled property of the removeButton is initialized to false. When a button is disabled in this way, it appears to be non-functional, and it actually is non-functional. It does not fire Clicked events. Towards the end of the Clicked handler, the IsEnabled property on removeButton is set to true only if the loggerLayout has at least one child.

This illustrates a good general rule: If your code needs to determine if a button Clicked event is valid, it’s likely much better to prevent invalid button clicks entirely by setting IsEnabled to false.

# Anonymous event handlers

Many C# programmers these days like to define small event handlers as anonymous lambda functions. This allows the event handling code to be very close to the instantiation and initialization of the object firing the event instead of somewhere else in the file. It also allows referencing objects within the event handler without storing those objects as fields.

Here’s a program named ButtonLambdas that has a Label displaying a number and two buttons. One button doubles the number, and the other halves the number. Normally the number and label variables would be saved as fields. But because the anonymous event handlers are defined right in the constructor after these variables are defined, the event handlers have access to them:

```
class ButtonLambdasPage : ContentPage
{
    public ButtonLambdasPage()
    {
        // Number to manipulate.
        double number = 1;

        // Create the Label for display.
        Label label = new Label
        {
            Text = number.ToString(),
            Font = Font.SystemFontOfSize(NamedSize.Large),
            HorizontalOptions = LayoutOptions.Center,
```