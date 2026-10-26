```
```csharp
addButton = new Button
{
    Text = "Add",
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
addButton.Clicked += OnButtonClicked;

removeButton = new Button
{
    Text = "Remove",
    HorizontalOptions = LayoutOptions.CenterAndExpand,
    IsEnabled = false
};
removeButton.Clicked += OnButtonClicked;

this.Padding = new Thickness(5, Device.OnPlatform(20, 0, 0), 5, 0);

// Assemble the page.
this.Content = new StackLayout
{
    Children =
    {
        new StackLayout
        {
            Orientation = StackOrientation.Horizontal,
            Children =
            {
                addButton,
                removeButton
            }
        },

        new ScrollView
        {
            VerticalOptions = LayoutOptions.FillAndExpand,
            Content = loggerLayout
        }
    }
};
}

void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;

    if (button == addButton)
    {
        // Add Label to scrollable StackLayout.
        loggerLayout.Children.Add(new Label
        {
            Text = "Button clicked at " + DateTime.Now.ToString("T")
        });
    }
    else
```
```