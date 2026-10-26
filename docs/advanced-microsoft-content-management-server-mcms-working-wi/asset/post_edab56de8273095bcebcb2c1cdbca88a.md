
attachment placeholder control). Again, don't worry about adding the code to the class file for now, we will show you how the JavaScript gets injected later.

```
function WBC_setThinEditIEAttachment(strPhName, strURL, strDispText)
{
    var strMyDispText = strDispText;
    if (strMyDispText == "")
    {
        strMyDispText = strURL;
    }
    if(typeof document.all["NCPHRICH_" + strPhName] != "undefined")
    {
        document.all["NCPHRICH_" + strPhName].insertHtml("<a href=\"" + strURL + "\">" + strMyDispText + "</a>");
    }
}
```

### Passing Attachment Information to the Placeholder Control

Now, on to the second problem of passing information about the attachment back to the multiple attachment placeholder control. We will populate the hidden fields and textboxes with information about the attachment's icon, display text and URL.

To get this information, let's look at two input parameters passed into the WBC_setThinEditIEAttachment() method:

| Input Parameter | What it means |
|---|---|
| strURL | As the name suggests, this parameter contains the URL of the attachment. |
| strMyDispText | Stores the icon. The returned HTML is an image tag, which looks like this: <img alt="Plants are Green!" src="/NR/rdonlyres/670B7551-5A53-4B0A-99782B7AD3EE7BC8/0/MyAttachment.doc?thumbnail=true&label=Plants%20are%20Green%21" border="0"> |

So strURL contains the URL of the attachment and strMyDispText holds the icon as shown in the script below:

```
function WBC_setThinEditIEAttachment(strPhName, strURL, strDispText)
{
    var strMyDispText = strDispText;
    if (strMyDispText == "")
    {
        strMyDispText = strURL;
    }
    if(typeof document.all["NCPHRICH_" + strPhName] != "undefined")
    {
        document.all["NCPHRICH_" + strPhName].insertHtml("<a href=\"" + strURL + "\">" + strMyDispText + "</a>");
    }
    else
    {
        // Return the image of the icon
        if (typeof document.all["MAPH_AttIcon_" + MAPH_DestPH] != "undefined")
        {
```

### Generating the WBC_setThinEditIEAttachment() Method

As our custom placeholder control is constructed entirely from server-side code, we need to generate the client script above on the fly and register it with Page.RegisterStartUpScript(). To do so, we must add the helper function RenderResourceGalleryRelatedJavascript() shown below to the code:

```csharp
private void RenderResourceGalleryRelatedJavascript()
{
    StringBuilder script = new StringBuilder();

    // Start the script block
    script.Append("<SCRIPT language=\"javascript\">" + crlf);

    // Declare the MAPH_DestPH variable
    script.Append("var MAPH_DestPH = \"\";" + crlf);

    // Define WBC_setThinEditIEAttachment function to replace one defined by MCMS
    script.Append("function WBC_setThinEditIEAttachment(strPhName, strURL,"
        + "strDispText){" + crlf);

    script.Append("var strMyDispText = strDispText;" + crlf
        + "if (strMyDispText == \"\")"
        + "{"
        + "strMyDispText = strURL;"
        + "}" + crlf);

    script.Append("if(typeof document.all[\"NCPHRICH_\" + strPhName] != "
        + "\"undefined\")"
        + "{"
        + "document.all[\"NCPHRICH_\" + strPhName].insertHtml(\"<a href=\\\"\" + strURL + \"\\\">\" + strMyDispText + \"</a>\");"
        + "}" + crlf);

    script.Append("else"
        + "{"
        + "elements = strMyDispText.split(\"\"); " + crlf);

    script.Append("if(typeof document.all[\"MAPH_AttIcon_\" + MAPH_DestPH] != \"undefined\")"
        + "{"
        + "document.all[\"MAPH_AttIcon_\" + MAPH_DestPH].innerHTML=strMyDispText;"
        + "}" + crlf);

    script.Append("if(typeof document.all[\"MAPH_AttLink_\" + MAPH_DestPH] != \"undefined\")"
        + "{"
        + "document.all[\"MAPH_AttLink_\" + MAPH_DestPH].value = strURL;"
        + "}" + crlf);

    script.Append("if (typeof document.all[\"MAPH_AttIconUrl_\"+MAPH_DestPH]!=\"undefined\")"
        + "{"
        + "document.all[\"MAPH_AttIconUrl_\" + MAPH_DestPH].value=strMyDispText;"
        + "}" + crlf);

    script.Append("if (typeof "
        + "document.all[\"MAPH_AttName_\"+MAPH_DestPH]!=\"undefined\")"
        + "{"
        + "document.all[\"MAPH_AttName_\""
```

## Useful Placeholder Controls

```
for (int i = 0; i < m_maxAttachments; i++)
{
    if (Page.Request.Form["MAPH_AttLink_" + sPhName + "_" + i.ToString()] != "")
    {
        XmlNode xAttachment = xd.CreateElement("Attachment");

        // Store the attachment's display text
        XmlNode xDisplayName = xd.CreateElement("DisplayText");
        xDisplayName.InnerText = Page.Request.Form["MAPH_AttName_" + sPhName
                                    + "_" + i.ToString()];
        xAttachment.AppendChild(xDisplayName);

        // Store the attachment's URL
        XmlNode xUrl = xd.CreateElement("Url");
        xUrl.InnerText = Page.Request.Form["MAPH_AttLink_" + sPhName + "_"
                                  + i.ToString()];
        xAttachment.AppendChild(xUrl);

        // Store the icon
        XmlNode xIcon = xd.CreateElement("Icon");
        xIcon.InnerText = Page.Request.Form["MAPH_AttIconUrl_" + sPhName + "_"
                                   + i.ToString()];
        xAttachment.AppendChild(xIcon);

        // Add the node to the XML Doc
        xnRoot.AppendChild(xAttachment);
    }
}
xph.XmlAsString = xnRoot.OuterXml;
}
```

### Retrieving Saved Content

To display the stored content, we first have to retrieve it. Here's the plan:

- First prepare three arrays for storing the attachment URLs, icons, and display text.
- Next, retrieve the saved XML from the underlying `XmlPlaceholder`.
- Once we have the XML, we extract information about the attachment from the tags and store them in the arrays that we have prepared.
- Finally, we load the content back into the table of all uploaded attachments.

### Preparing Arrays for Storing Information about the Attachments

Let's prepare three string arrays for storing a list of attachment URLs, icons, and display text. Add the following code to the start of the LoadPlaceholderContentForAuthoring() method:

```
// Stores the URL of the attachment
private string[] attUrl = new string[m_maxAttachments];

// Stores the icon
private string[] attIcon = new string[m_maxAttachments];

// Stores the display text
private string[] attName = new string[m_maxAttachments];

protected override void LoadPlaceholderContentForAuthoring
                               (PlaceholderControlEventArgs e)
{
    - - - code continues - - -
}
```

Let's start with the icons. The attachment's icon is displayed in the first cell of each row. We will get the icon stored in the attIcon[] array and add it to the cell. Add the code highlighted below:

```
protected override void LoadPlaceholderContentForAuthoring(
    PlaceholderControlEventArgs e)
{
    . . . code continues . . .
    // Add the cell for the attachment's icon
    htmlCode.Append("<td align=center id=\"MAPH_AttIcon_" + sPhName
        + "_" + i.ToString() + "\">" + attIcon[i] + "</td>" + crlf);
    . . . code continues . . .
}
```

The icon is also stored in a hidden field in the second column of the table. The hidden field has a name starting with MAPH_AttIcon and, like the cell in the first column, will be populated with the contents of the attIcon[] array.

```
protected override void
LoadPlaceholderContentForAuthoring(PlaceholderControlEventArgs e)
{
    . . . code continues . . .
    // Add a hidden form field to store the icon
    htmlCode.Append("<input type=\"hidden\" name=\"MAPH_AttIconUrl_" + sPhName
        + "_"+i.ToString() + "\" value=\"" + attIcon[i] + "\">");
    . . . code continues . . .
}
```

The display text of the attachment is shown in a textbox with a name that starts with MAPH_AttName. We will set it to hold the contents of the attName[] array.

```
protected override void
LoadPlaceholderContentForAuthoring(PlaceholderControlEventArgs e)
{
    . . . code continues . . .
    // Add the Text box to display the attachment's display text
    htmlCode.Append("<input type=\"text\" name=\"MAPH_AttName_" + sPhName
        + "_" + i.ToString() + "\" value=\"" + attName[i] + "\">");
    . . . code continues . . .
}
```

The last item to fill in is the URL of the attachment. These are displayed in the textboxes that begin with MAPH_AttLink. The contents of the attUrl[] array will be used to fill in their values.

```
protected override void LoadPlaceholderContentForAuthoring(
    PlaceholderControlEventArgs e)
{
    . . . code continues . . .
    // Add the read-only Text box to display the attachment's URL
    htmlCode.Append("<input readonly type=\"text\" name=\"MAPH_AttLink_"
        + sPhName + "_" + i.ToString()+"\" value=\""+attUrl[i]+"\">");
    . . . code continues . . .
}
```
