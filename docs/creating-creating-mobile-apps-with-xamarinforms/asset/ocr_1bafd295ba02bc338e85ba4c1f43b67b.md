```
// Create Label views for displaying page width and height.
Label widthLabel = new Label
{
    Font = font
};

Label heightLabel = new Label
{
    Font = font
};

// Define bindings on Labels.
widthLabel.BindingContext = this;
widthLabel.SetBinding (Label.TextProperty,
    Binding.Create<ContentPage> (src => src.Width,
        BindingMode.OneWay,
        null, null, "{0:F0}"));

heightLabel.BindingContext = this;
heightLabel.SetBinding (Label.TextProperty,
    new Binding ("Height",
        BindingMode.OneWay,
        null, null, "{0:F0}"));

// Assemble a horizontal StackLayout with the three labels.
this.Content = new StackLayout
{
    Orientation = StackOrientation.Horizontal,
    Spacing = 0,
    HorizontalOptions = LayoutOptions.Center,
    VerticalOptions = LayoutOptions.Center,
    Children =
    {
        widthLabel,
        new Label
        {
            Text = " x ",
            Font = font
        },
        heightLabel
    }
};
}
```

Notice the use of a horizontal StackLayout with no spacing to visually concatenate three pieces of text so they appear to be a single line.

As you shift the phone between portrait and landscape, the value updates entirely through the data bindings.