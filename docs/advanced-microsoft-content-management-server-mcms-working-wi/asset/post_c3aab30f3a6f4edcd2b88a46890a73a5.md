
## Rendering the RSS Feed in Presentation Mode

We will now create the interface for presenting the RSS feed. In unpublished mode, the RssReader placeholder control will show "Feed will be shown in published mode", and when the placeholder is viewed in published mode, LoadAndRenderRssFeed() will be executed to retrieve the feed and its output will be bound to the basePresentationControl.

First we override CreatePresentationChildControls() to create the presentation child controls:

```csharp
#region Presentation

/// <summary>
/// Create the Presentation Controls
/// </summary>
/// <param name="presentationContainer"></param>
protected override void CreatePresentationChildControls(
    BaseModeContainer presentationContainer)
{
    // Instantiate a literal control and
    // set the base presentation control
    this.basePresentationControl = new LiteralControl();

    // Set the ID of the base presentation control
    this.basePresentationControl.ID = "PresentationControl";
    // Add the base presentation control to
    // the presentation container
    presentationContainer.Controls.Add(this.basePresentationControl);
}
```

```csharp
// Date?
if (this.ShowDate)
{
    // Append the Date
    sb.Append(PubDate);
}

sb.Append("</div>\n");

// Are we showing the Open Article Link?
if (this.ShowOpenArticleLink)
{
    sb.Append(" <div>");
    sb.Append("<a target=\"_blank\" href=\"" + Link
        + "\">[Open Article]</a>");
    sb.Append("</div>\n");
}

sb.Append("</div>\n");
rsReader.Read();
break;
}
default:
{
    rsReader.Read();
    break;
}
}
}

// return the string builder contents
return(sb.ToString());
}

#endregion
```

To load the RSS feed and output the items using the placeholder control, we will override the LoadPlaceholderContentForPresentation() method. The feed will not be rendered in "Presentation Unpublished" mode for authoring efficiency.

To minimize the hit to the feed provider, we will implement content caching for our placeholder control. This means even if several users request the posting containing the RssReaderPlaceholderControl within the configured timeframe, only one web request will hit the feed site. This minimizes traffic and processing overhead for the feed site as well as the website holding this placeholder control.

The cache duration is currently hardcoded to five minutes. If you need this to be more flexible, you can either store the cache duration in the Web.config or add a textbox to the authoring mode that captures the cache duration and saves it as additional configuration value for the XmlPlaceholder.

```csharp
/// <summary>
/// Load and parse the RSS feed. Output the feed
/// items to the basePresentationControl
/// </summary>
/// <param name="e"></param>
protected override void
LoadPlaceholderContentForPresentation(PlaceholderControlEventArgs e)
{
    // Verify the child controls have been created
    EnsureChildControls();

    try
    {
        // Check the current mode
        if (WebAuthorContext.Current.Mode ==
            WebAuthorContextMode.PresentationUnpublished)
        {
            this.basePresentationControl.Text =
                "Feed will be shown in published mode";
        }
        else
        {
            // Load the settings from the XML
            this.LoadSettingsFromXml(this.BoundXmlPlaceholder.XmlAsString);

            // Get the current Cache object from HttpContext
            System.Web.Caching.Cache cache = HttpContext.Current.Cache;

            // we will vary the cache entry by the content of the
            // current placeholder object. This will ensure that different
            // feeds can be served from the same website
            string CacheKey = this.BoundXmlPlaceholder.XmlAsString;

            // check if content is already in Cache
            string cacheXml = cache["RssReader." + CacheKey] as string;
            if (cacheXml == null)
            {
```

## Enhancements to the RSS Feed Generator

- Author name—The solution could be enhanced by integration with Active Directory to retrieve the author's full name.
- Channel-based feed generation—set up channel properties as configuration parameters for the feed and generate it from a posting or channel rendering script.
- Storing history—Organizations may have a requirement to keep a copy of all historical content that has been on their website. The RSS feed currently potentially changes every 5 minutes (when the cache expires). To make the RSS feed act as content that appears in the page revision history, the request to the feed could be made on save and the generated HTML could be saved into an HtmlPlaceholder. An auto-updater tool could then be written to update the aggregated feed at a scheduled time, say every 60 minutes.

## Enhancements to the RSS Aggregator Placeholder Control

- Design—The RSS aggregation control only has a rudimentary look and feel that could easily be enhanced by adding class attributes to the div elements to apply styles defined in the page's CSS stylesheet.

## Summary

We have now given the Tropical Green website the ability to syndicate and aggregate content via RSS. Content authors will have the ability to specify RSS feeds to aggregate and display on specific postings. End users will now have the chance to subscribe to an RSS feed on our MCMS website so they can be automatically notified when content changes happen without having to browse to the website. In addition, an MCMS website can now aggregate content from RSS feeds coming from other servers on the Internet and display some of that website's own content.

Below the Current Version header, we will add the table that contains the hyperlink to the working version of the posting. Toggle to Design view. Select Table | Insert | Table... from the menu bar and insert a table with one row and two columns. In the table cells on the left-hand column, add the word Name. Drag and drop a HyperLink control (from the Web Forms section of the Toolbox) into the cell on the right-hand side. Give the HyperLink the ID aName (note that we have prefixed it with the letter 'a'), and set its Text property to be an empty string.

Next, drag a DataGrid and drop it below the Available Revisions header. The DataGrid will list all revisions of the posting. Set the ID of the DataGrid to dgPageRevisionsList and the AutoGenerateColumns property to false. Switch to HTML view and add four columns to the grid. The first column displays the name of the posting for that particular version, the second shows the revision date, the third is a hyperlink that links to a preview page, and the fourth is a button that triggers the page reversion process.

```xml
<asp:DataGrid ID="dgPageRevisionsList" Runat="server"
AutoGenerateColumns="False">
<Columns>
<asp:BoundColumn DataField="Name" HeaderText="Name"></asp:BoundColumn>
<asp:BoundColumn DataField="RevisionDate" HeaderText="Revision Date">
</asp:BoundColumn>
<asp:HyperLinkColumn Text="Preview" Target="CMSPageRevertPreviewWindow" DataNavigateUrlField="Url">
</asp:HyperLinkColumn>
<asp:ButtonColumn Text="Revert" CommandName="RevertTo">
</asp:ButtonColumn>
</Columns>
</asp:DataGrid>
```

Toggle to Design view. Drag two CheckBox controls and drop them above the DataGrid. Set the ID of the first CheckBox to chkRevertContent and its Text property to Revert Placeholder Content. Give the second CheckBox an ID of chkRevertCustomProperties and a Text value of Revert Custom Property Values. Set the Checked property of both CheckBoxes to true.

Drag a Label control and drop it below the DataGrid. Set the Label's ID to lblMessage and its Text property to an empty string. We will display messages (such as errors) from the application here.

Here's what you should see:
