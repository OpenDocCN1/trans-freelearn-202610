```
{
    Text = "Greetings, Xamarin.Forms!",
    XAlign = TextAlignment.Center,
    YAlign = TextAlignment.Center
};
}
```

Visually, the result with this single line of text is the same as setting HorizontalOptions and VerticalOptions to center, and you can also use various combinations of these properties to position the text in one of nine different locations around the page.

However, these two techniques to center the text are actually quite different, as you’ll soon see.

# Wrapping paragraphs

Displaying a paragraph of text is as easy as displaying a single line of text:

```
class BaskervillesPage : ContentPage
{
    public BaskervillesPage()
    {
        this.Content = new Label
        {
            VerticalOptions = LayoutOptions.Center,
            Text =
                "Mr. Sherlock Holmes, who was usually very late in " +
                "the mornings, save upon those not infrequent " +
                "occasions when he was up all night, was seated at " +
                "the breakfast table. I stood upon the hearth-rug " +
                "and picked up the stick which our visitor had left " +
                "behind him the night before. It was a fine, thick " +
                "piece of wood, bulbous-headed, of the sort which " +
                "is known as a \u201CPenang lawyer.\u201D Just " +
                "under the head was a broad silver band, nearly an " +
                "inch across, \u201CTo James Mortimer, M.R.C.S., " +
                "from his friends of the C.C.H.,\u201D was engraved " +
                "upon it, with the date \u201C1884.\u201D It was " +
                "just such a stick as the old-fashioned family " +
                "practitioner used to carry\u2014dignified, solid, " +
                "and reassuring."
        };

        this.Padding = new Thickness(5, Device.OnPlatform(20, 5, 5), 5, 5);
    }
}
```

Notice the use of embedded Unicode codes for opened and closed "smart quotes" (\u201C and \u201D) and the em-dash (\u2014). Padding has been set for 5 units around the page to avoid the text butting up against the edges of the screen, but the VerticalOptions property has been used