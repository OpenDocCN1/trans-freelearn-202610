Label also allows specifying color, and a little experimentation with color will demonstrate the profound difference between HorizontalOptions and VerticalOptions, and XAlign and YAlign.

# Text and background colors

As you've seen, the Label view displays text in a color appropriate for the device. You can override that behavior by setting two properties named TextColor and BackgroundColor. Label itself defines TextColor but inherits BackgroundColor from VisualElement, which means that Page and Layout also have a BackgroundColor property.

You set TextColor and BackgroundColor to a value of type Color, which is a structure that defines 16 static fields for obtaining common colors. Here are two of them used in conjunction with XAlign and YAlign to center the text:

```
class GreetingsPage : ContentPage
{
    public GreetingsPage()
    {
        this.Content = new Label
        {
            Text = "Greetings, Xamarin.Forms!",
            XAlign = TextAlignment.Center,
            YAlign = TextAlignment.Center,
            BackgroundColor = Color.Yellow,
            TextColor = Color.Blue
        };
    }
}
```

The result might surprise you. As these screenshots illustrate, the Label actually occupies the entire area of the page (including underneath the iOS status bar), and the XAlign and YAlign properties position the text within that entire area: