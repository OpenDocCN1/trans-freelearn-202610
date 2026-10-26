control, and to override the SavePlaceholderContent() method, which saves the configuration information in the underlying XmlPlaceholder object.

The LoadPlaceholderContentForAuthoring() method will get the information about the configured SharePoint Event list from the bound XmlPlaceholder object, extract the SharePoint List Service Web Service URL and the name of the Event list, and call our DisplaySharePointEventListItems() method to display the content of the list:

```
protected override void LoadPlaceholderContentForAuthoring(
    PlaceholderControlEventArgs e)
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
        .Attributes["sharePointUrl"].Value;
    spListName = placeholderContents.DocumentElement.ChildNodes[0]
        .Attributes["listName"].Value;

    // bind the config info to the text boxes
    txbSharePointUrl.Text = sharePointUrl;
    txbListName.Text = spListName;

    // show the items
    DisplaySharePointEventListItems(sharePointUrl, spListName);
}
```

The SavePlaceholderContent() method will create a small XML string containing the Web Service URL and list name and insert it into the bound placeholder:

```
protected override void SavePlaceholderContent(PlaceholderControlSaveEventArgs e)
{
    string config = string.Empty;

    // make sure enough information is provided in the input boxes
    if ((txbSharePointUrl.Text == string.Empty)
        || (txbListName.Text == string.Empty))
        return;

    // save the config information
    config = string.Format("<config><sharepointlist sharePointUrl=\"{0}\""
        + " listname=\"{1}\" /></config>",
        txbSharePointUrl.Text, txbListName.Text);

    ((XmlPlaceholder)this.BoundPlaceholder).XmlAsString = config;
}
```

Let's see how our control looks in authoring mode. Build the TropicalGreenControlLib project, open a browser, and navigate to http://www.tropicalgreen.net/TropicalGreen/Events.htm. If you hadn't logged in already, go ahead and do so. Once you're logged in, click the Switch To Edit Site link in the Web Author Console, and then click Edit to switch into authoring mode for the posting. Enter the following into the two input boxes and click Save and Exit in the Web Author Console: