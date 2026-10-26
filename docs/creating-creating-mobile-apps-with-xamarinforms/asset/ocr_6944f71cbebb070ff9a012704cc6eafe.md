```
VerticalOptions = LayoutOptions.CenterAndExpand
};

// Create the first Button and attach Clicked handler.
Button timesButton = new Button
{
    Text = "Double",
    Font = Font.SystemFontOfSize(NamedSize.Large),
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
timesButton.Clicked += (sender, args) =>
{
    number *= 2;
    label.Text = number.ToString();
};

// Create the second Button and attach Clicked handler.
Button divideButton = new Button
{
    Text = "Half",
    Font = Font.SystemFontOfSize(NamedSize.Large),
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
divideButton.Clicked += (sender, args) =>
{
    number /= 2;
    label.Text = number.ToString();
};

// Assemble the page.
this.Content = new StackLayout
{
    Children =
    {
        label,
        new StackLayout
        {
            Orientation = StackOrientation.Horizontal,
            VerticalOptions = LayoutOptions.CenterAndExpand,
            Children =
            {
                timesButton,
                divideButton
            }
        }
    }
};
}
}
```

Like the previous program, the two buttons share a horizontal `StackLayout`: