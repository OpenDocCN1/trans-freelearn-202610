8. To manage the storage and retrieval of the data for this placeholder, we will create the two methods LoadSettingsFromXml() and GetSettingsAsXml():

```
#region Load/Set Settings

/// <summary>
/// Load the settings of the placeholder from an XmlString
/// </summary>
/// <param name="strXml"></param>
public void LoadSettingsFromXml(string strXml)
{
    // Create a new xml Document
    XmlDocument _doc = new XmlDocument();

    // Load the xml into the xml document
    _doc.LoadXml(strXml);

    // Set the variables from the XML
    this.RssUrl = _doc.SelectSingleNode("/Content/RssUrl").InnerText;
    this.ItemsToDisplay = System.Convert.ToInt32(
        _doc.SelectSingleNode("/Content/ItemsToDisplay").InnerText);
    this.ShowOpenArticleLink = Boolean.Parse(
        _doc.SelectSingleNode("/Content/ShowOpenArticleLink").InnerText);
    this.ShowDescription = Boolean.Parse(
        _doc.SelectSingleNode("/Content/ShowDescription").InnerText);
    this.ShowDate = Boolean.Parse(
        _doc.SelectSingleNode("/Content/ShowDate").InnerText);
    this.ShowAuthor = Boolean.Parse(
        _doc.SelectSingleNode("/Content/ShowAuthor").InnerText);
}
```

9. To create the XML string in the GetSettingsAsXml() method, we will use a StringBuilder class to build the XML string. We could equally use an XmlTextWriter object.

```
/// <summary>
/// Get the current set of settings as an Xml string
/// </summary>
/// <returns></returns>
public string GetSettingsAsXml()
{
    // Create a new string builder
    StringBuilder sb = new StringBuilder();

    // Create the Content XML string and populate elements
    sb.Append("<Content>");
    sb.Append("  <RssUrl>" + HttpUtility.HtmlEncode(this.txtRSSUrl.Text)
        + "</RssUrl>");
    sb.Append("<ItemsToDisplay>"
        + this.ddlItemsToDisplay.Items[ddlItemsToDisplay.SelectedIndex].Value
        + "</ItemsToDisplay>");
    sb.Append("<ShowOpenArticleLink>"
        + this.chkShowOpenArticleLink.Checked.ToString()
        + "</ShowOpenArticleLink>");
    sb.Append("<ShowDescription>"
        + this.chkShowDescription.Checked.ToString()
        + "</ShowDescription>");
    sb.Append("<ShowDate>" + this.chkShowDate.Checked.ToString()
        + "</ShowDate>");
    sb.Append("<ShowAuthor>" + this.chkShowAuthor.Checked.ToString()
        + "</ShowAuthor>");
    sb.Append("</Content>");
```