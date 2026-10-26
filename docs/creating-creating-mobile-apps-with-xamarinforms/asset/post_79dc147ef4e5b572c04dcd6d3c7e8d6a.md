
```csharp
};

// Get access to the text resource.
Assembly assembly = this.GetType().GetTypeInfo().Assembly;
string resource = "BlackCat.Texts.TheBlackCat.txt";

using (Stream stream = assembly.GetManifestResourceStream (resource))
{
    using (StreamReader reader = new StreamReader (stream))
    {
        bool gotTitle = false;
        string line;

        // Read in a line (which is actually a paragraph).
        while (null != (line = reader.ReadLine()))
        {
            Label label = new Label
            {
                Text = line,

                // Black text for ebooks!
                TextColor = Color.Black
            };

            if (!gotTitle)
            {
                // Add first label (the title) to mainStack.
                label.HorizontalOptions = LayoutOptions.Center;
                label.Font = Font.SystemFontOfSize(NamedSize.Medium,
                    FontAttributes.Bold);

                mainStack.Children.Add(label);
                gotTitle = true;
            }
            else
            {
                // Add subsequent labels to textStack.
                textStack.Children.Add(label);
            }
        }
    }
}

// Put the textStack in a ScrollView with FillAndExpand.
ScrollView scrollView = new ScrollView
{
    Content = textStack,
    VerticalOptions = LayoutOptions.FillAndExpand,
    Padding = new Thickness(5, 0)
};

// Add the ScrollView as a second child of mainStack.
mainStack.Children.Add(scrollView);

// Set page content to mainStack.
```

```csharp
Children =
{
    button,
    new ScrollView
    {
        VerticalOptions = LayoutOptions.FillAndExpand,
        Content = loggerLayout
    }
};
}

void OnButtonClicked(object sender, EventArgs args)
{
    // Add Label to scrollable StackLayout.
    loggerLayout.Children.Add(new Label
    {
        Text = "Button clicked at " + DateTime.Now.ToString("T")
    });
}
```

In the programs in this book, event handlers are given names beginning with the word On, followed by some kind of identification of the view firing the event (sometimes just the view type), followed by the event name. The resultant name in this case is OnButtonClicked.

The Clicked handler is attached to the Button right after the Button is created:

```csharp
button.Clicked += OnButtonClicked;
```

Then the page is assembled. Notice that the ScrollView has its VerticalOptions set to FillAndExpand so that it can share the StackLayout with the Button and still be visible and scrollable.

Here’s the display after a few clicks:

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
    {
        // Remove topmost Label from StackLayout
        loggerLayout.Children.RemoveAt(0);
    }
    // Enable "Remove" button only if children are present.
    removeButton.IsEnabled = loggerLayout.Children.Count > 0;
}
```

The two buttons are given HorizontalOptions values of CenterAndExpand so that they can be nicely arranged on the screen using a horizontal StackLayout.

Notice that when the Clicked handler detects removeButton, it simply calls the RemoveAt method on the Children property. What happens if there are no children?

It can’t happen in this program! When the application begins, the IsEnabled property of the removeButton is initialized to false. When a button is disabled in this way, it appears to be non-functional, and it actually is non-functional. It does not fire Clicked events. Towards the end of the Clicked handler, the IsEnabled property on removeButton is set to true only if the loggerLayout has at least one child.

This illustrates a good general rule: If your code needs to determine if a button Clicked event is valid, it’s likely much better to prevent invalid button clicks entirely by setting IsEnabled to false.

## Anonymous event handlers

Many C# programmers these days like to define small event handlers as anonymous lambda functions. This allows the event handling code to be very close to the instantiation and initialization of the object firing the event instead of somewhere else in the file. It also allows referencing objects within the event handler without storing those objects as fields.

Here’s a program named ButtonLambdas that has a Label displaying a number and two buttons. One button doubles the number, and the other halves the number. Normally the number and label variables would be saved as fields. But because the anonymous event handlers are defined right in the constructor after these variables are defined, the event handlers have access to them:

```csharp
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

![](img/c2d61d0459c51981af78607f1eef3d1c_85_0.png)

The disadvantage of defining event handlers as anonymous lambda functions is that they can’t be shared among multiple views.

## Distinguishing views with IDs

In the **TwoButtons** program you saw a technique for sharing an event handler that distinguishes views by comparing objects. This works fine when there aren’t very many views to distinguish, but it would be a terrible approach for a calculator program.

The `Element` class defines a `StyleId` of type `string` specifically for the purpose of identifying views. Set it to whatever is convenient for the application. You can test the values using `if` and `else` statements, or in a `switch` and `case`, or you can use a `Parse` method to convert the strings into numbers or enumeration members.

The following program isn’t a calculator, but it is a numeric keypad, which is certainly part of a calculator. The program is called **SimplestKeypad** and uses a `StackLayout` for organizing the rows and columns of keys. (One of the intents of this program is to demonstrate that `StackLayout` is not quite the right tool for this job!)

The program creates a total of four `StackLayout` instances. The `mainStack` is vertically oriented; three horizontal `StackLayout` objects arrange the nine non-zero digit buttons. To keep things simple, the keypad is arranged with telephone ordering rather than calculator ordering:

```csharp
class SimplestKeypadPage : ContentPage
{
    Label displayLabel;
    Button backspaceButton;
```
