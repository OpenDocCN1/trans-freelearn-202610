Now we get a BoxView with that specific size:

![](bbox=[0.12, 0.14, 0.85, 0.48])

Let’s use both Frame and BoxView in an enhanced color list. The ColorBlock program has a page constructor that is virtually identical to the one in ReflectedColors, except that it calls a method named CreateColorView rather than CreateColorLabel. Here’s that method:

```
class ColorBlockPage : ContentPage
{
    ...

    View CreateColorView(Color color, string name)
    {
        return new Frame
        {
            Content = new StackLayout
            {
                Orientation = StackOrientation.Horizontal,
                Spacing = 15,
                Children =
                {
                    new BoxView
                    {
                        Color = color
                    },
                    new Label
                    {
                        Text = name,
                        Font = Font.BoldSystemFontOfSize(NamedSize.Medium),
                    }
                }
            }
        }
    }
```