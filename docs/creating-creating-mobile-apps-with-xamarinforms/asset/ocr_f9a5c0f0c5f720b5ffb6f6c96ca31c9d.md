```
```csharp
public SimplestKeypadPage()
{
    // Create a vertical stack for the entire keypad.
    StackLayout mainStack = new StackLayout
    {
        VerticalOptions = LayoutOptions.Center,
        HorizontalOptions = LayoutOptions.Center
    };

    // First row is the Label.
    displayLabel = new Label
    {
        Font = Font.SystemFontOfSize(NamedSize.Large),
        VerticalOptions = LayoutOptions.Center,
        XAlign = TextAlignment.End
    };
    mainStack.Children.Add(displayLabel);

    // Second row is the backspace Button.
    backspaceButton = new Button
    {
        Text = "\\u21E6",
        Font = Font.SystemFontOfSize(NamedSize.Large),
        IsEnabled = false
    };
    backspaceButton.Clicked += OnBackspaceButtonClicked;
    mainStack.Children.Add(backspaceButton);

    // Now do the 10 number keys.
    StackLayout rowStack = null;

    for (int num = 1; num <= 10; num++)
    {
        if ((num - 1) % 3 == 0)
        {
            rowStack = new StackLayout
            {
                Orientation = StackOrientation.Horizontal
            };
            mainStack.Children.Add(rowStack);
        }

        Button digitButton = new Button
        {
            Text = (num % 10).ToString(),
            Font = Font.SystemFontOfSize(NamedSize.Large),
            StyleId = (num % 10).ToString()
        };
        digitButton.Clicked += OnDigitButtonClicked;

        // For the zero button, expand to fill horizontally.
        if (num == 10)
        {
            digitButton.HorizontalOptions = LayoutOptions.FillAndExpand;
        }
```
```