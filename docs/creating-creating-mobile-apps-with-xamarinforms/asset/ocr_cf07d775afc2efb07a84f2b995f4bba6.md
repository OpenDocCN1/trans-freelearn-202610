GreetingsPage definition:

```
class GreetingsPage : ContentPage
{
    public GreetingsPage()
    {
        Label label = new Label
        {
            Text = "Greetings, Xamarin.Forms!"
        };
        this.Content = label;
    }
}
```

This style allows the Label instance to be set directly to the Content property directly, like so:

```
class GreetingsPage : ContentPage
{
    public GreetingsPage()
    {
        this.Content = new Label
        {
            Text = "Greetings, Xamarin.Forms!"
        };
    }
}
```

For more complex page layouts, this style of instantiation and initialization provides a better visual analogue of the organization of layouts and views on the page. However, it's not always as simple as this example might indicate if you need to call methods on these objects or set event handlers.

Whichever way you do it, if you can successfully run the program on the three platforms in either an emulator or a device, here's what you'll see: