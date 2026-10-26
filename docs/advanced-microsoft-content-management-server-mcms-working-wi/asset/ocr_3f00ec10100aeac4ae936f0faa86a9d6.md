| Name | Value |
|---|---|
| SharePoint Portal Site URL | http://portal.tropicalgreen.net |
| SharePoint Event List Name | Events |

Now all we need is to wire up the two additional methods to create and populate the presentation controls for our placeholder control using `CreatePresentationChildControls()`, get the information about the configured SharePoint Event list from the bound placeholder, and call `DisplaySharePointEventListItems()` to display the items using the method called `LoadPlaceholderContentForPresentation()`:

```
protected override void CreatePresentationChildControls
    (BaseModeContainer presentationContainer)
{
    litEventItemsLiteral = new LiteralControl();
    presentationContainer.Controls.Add(litEventItemsLiteral);
}

protected override void LoadPlaceholderContentForPresentation
    (PlaceholderControlEventArgs e)
{
    EnsureChildControls();

    XmlDocument placeholderContents = new XmlDocument();
    string sharePointUrl = string.Empty;
    string spListName = string.Empty;

    // load the saved config from the posting
    placeholderContents.LoadXml(((XmlPlaceholder)this.BoundPlaceholder)
        .XmlAsString);

    // extract the necessary information
    sharePointUrl = placeholderContents.DocumentElement.ChildNodes[0]
        .Attributes["sharePortalUrl"].Value;
    spListName = placeholderContents.DocumentElement.ChildNodes[0]
        .Attributes["listname"].Value;

    // show the items
    DisplaySharePointEventListItems(sharePointUrl, spListName);
}
```

That should do it… build the TropicalGreenControlLib project one last time and navigate back to the Events page at http://www.tropicalgreen.net/TropicalGreen/Events.htm.