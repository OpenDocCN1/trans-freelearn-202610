```
BindableProperty.Create<RadioButton, Font>
    (radio => radio.Font,                 // property
        Font.SystemFontOfSize(NamedSize.Large));   // initial value

public Font Font
{
    set { SetValue(FontProperty, value); }
    get { return (Font)GetValue(FontProperty); }
}
…
}
```

The Create method uses reflection to obtain the string rendition of the Font property name. The Font property is initialized to the Large size because the Medium size doesn’t provide an adequate touch target.

Now the RadioButton has Text and Font properties but the values of these properties must be set on the two Label views the RadioButton has created in its constructor. This could happen in property-changed handlers for the Text and Font properties just defined, but data bindings provide a simpler approach. In the following data bindings defined in the RadioButton constructor, the targets are the Label views, and the source is the RadioButton itself:

```
class RadioButton : ContentView
{
    …
    public RadioButton()
    {
        …
        // Set the bindings.
        checkLabel.BindingContext = this;
        checkLabel.SetBinding(Label.FontProperty, "Font");

        textLabel.BindingContext = this;
        textLabel.SetBinding(Label.TextProperty, "Text");
        textLabel.SetBinding(Label.FontProperty, "Font");
        …
    }
    …
}
```

The RadioButton also requires a Toggled event and an IsToggled property backed by an IsToggledProperty bindable property. This definition of the BindableProperty object contains a reference to a method called when the property changes:

```
class RadioButton : ContentView
{
    …
    // Define the Toggled event, IsToggled bindable property and property.
    public event EventHandler<ToggledEventArgs> Toggled;

    public static readonly BindableProperty IsToggledProperty =
        BindableProperty.Create("IsToggled",
```