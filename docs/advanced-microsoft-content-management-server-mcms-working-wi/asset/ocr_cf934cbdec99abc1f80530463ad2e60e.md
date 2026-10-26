```
this.basePresentationControl.ID = "PresentationControl";
// Add the base presentation control to
// the presentation container
presentationContainer.Controls.Add(this.basePresentationControl);
}
```

To load the RSS feed and output the items using the placeholder control, we will override the LoadPlaceholderContentForPresentation() method. The feed will not be rendered in "Presentation Unpublished" mode for authoring efficiency.

To minimize the hit to the feed provider, we will implement content caching for our placeholder control. This means even if several users request the posting containing the RssReaderPlaceholderControl within the configured timeframe, only one web request will hit the feed site. This minimizes traffic and processing overhead for the feed site and as well the website holding this placeholder control.

The cache duration is currently hardcoded to five minutes. If you need this to be more flexible, you can either store the cache duration in the Web.config or add a textbox to the authoring mode that captures the cache duration and saves it as additional configuration value for the XmlPlaceholder.

```
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