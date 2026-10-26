```
private static void ProcessPageAndGetAttachments(string encodingName,
    ref byte[] buffer)
{
    // ... code continues ...
    // Replace special characters to make it easier to find
    content = content.Replace("\t", " ").Replace("\n", " ").Replace("\r", " ");
    // Get attachments

    // Start searching for attachments!
    FindAttachment(ref content, "base", "href");
    FindAttachment(ref content, "a", "href");
    FindAttachment(ref content, "link", "href");
    FindAttachment(ref content, "script", "src");
    FindAttachment(ref content, "xml", "src");
    FindAttachment(ref content, "img", "src");
    FindAttachment(ref content, "frame", "src");
    FindAttachment(ref content, "iframe", "src");
    FindAttachment(ref content, "embed", "src");
    FindAttachment(ref content, "body", "background");
    FindAttachment(ref content, "td", "background");
    FindAttachment(ref content, "th", "background");
    FindAttachment(ref content, "table", "background");
    FindAttachment(ref content, "layer", "background");
}
```

Of course, depending on the tags used by authors, the list above may not be exhaustive. Feel free to add more tags and attributes to the list.

The FindAttachment() method accepts three input parameters:

+   - **input**: The HTML of the page being scanned
+   - **tagName**: The tag to look for (e.g. <img>)
+   - **attribute**: The attribute that stores the attachment's URL (e.g. src)

It looks for all instances of the tag in the content. For example, when it finds an <img> tag, it extracts the URL from its src attribute using the ExtractUrlFromTag() method (defined later).

If a URL has been successfully extracted, it is added to the list of URLs using the AddToUrlList() helper function (also defined later).

<base> tags are handled separately. Earlier, we discussed how <base> tags are injected into all postings by the RobotMetaTag. The browser interprets all relative links by pre-pending the value in its href attribute to them. If FindAttachment() sees a <base> tag, it calls the SetBaseUrl() routine, which stores the extracted URL in the m_LocalBaseUrl variable for use later when downloading relative links.

Add the FindAttachment() method to the class:

```
private static void FindAttachment(ref string input, string tagName,
    string attribute)
{
    // Pattern that extracts all tags with the specified tagName
    string pattern = @"<\s*" + tagName + @"\b[^>]*>";
    // The regular expression that finds all the tags
    Regex findTags = new Regex(pattern, RegexOptions.IgnoreCase);
    foreach (Match tag in findTags.Matches(input))
    {
        // Extract the URL from each tag based on the specified attribute
        string url = ExtractUrlFromTag(tag.Value, attribute);
        // We have successfully extracted a URL
```