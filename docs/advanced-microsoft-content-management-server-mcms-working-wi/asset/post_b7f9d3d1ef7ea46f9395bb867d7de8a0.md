
# Useful Placeholder Controls

```csharp
protected override void LoadPlaceholderContentForPresentation(
                                      PlaceholderControlEventArgs e)
{
    EnsureChildControls();

    // Get the current CmsHttpContext
    CmsHttpContext cmsContext = CmsHttpContext.Current;

    // Get an instance of the selected resource gallery
    string xml = ((XmlPlaceholder)this.BoundPlaceholder).XmlAsString;
    m_resourceGalleryPath = GetResourceGalleryPath(xml);

    ResourceGallery rg = cmsContext.Searches.GetByPath(m_resourceGalleryPath)
                             as ResourceGallery;
}
```

Once we have an instance of the resource gallery, a random number is generated. Again, we will write a helper function, RandomNumberGenerator(), to pick a number from zero to one less than the number of resources in the gallery:

```csharp
private int RandomNumberGenerator(int max)
{
    // generate a random number
    int seed;
    int index = 0;
    seed = (int)System.DateTime.Now.Ticks % System.Int32.MaxValue;
    System.Random rand = new System.Random(seed);
    index = rand.Next(0,max);

    return index;
}
```

Next, we get the resource that corresponds to the randomly chosen index. Modify the LoadPlaceholderContentForPresentation() method as shown below:

```csharp
protected override void LoadPlaceholderContentForPresentation(
                                      PlaceholderControlEventArgs e)
{
    . . . code continues . . .

    if (rg != null)
    {
        // Randomly select a resource from the gallery
        int indexOfResource = RandomNumberGenerator(rg.Resources.Count);
        Resource r = rg.Resources[indexOfResource];
    }
}
```

To determine the file's type, we check its MIME type (Multipurpose Internet Mail Extensions type). MIME types are used by the browser to determine how content should be displayed. For example, a file with the MIME type `applications/x-shockwave-flash` will be played within a Flash player instead of being displayed as an image or text. The exact value returned by the Resource.MimeType property depends on the server's registry.

```
```html
<TD>Greet the world: </TD>
<TD></TD>
</TR>
</TBODY>
</TABLE>
```
```

What happened? The HtmlPlaceholder object was designed to store only specific tags such as the `<table>`, `<tr>`, and `<td>` tags (note that these table tags are only accepted when full formatting is allowed). The full list can be obtained from the documentation.

## Where can I find a full list of tags accepted by the HtmlPlaceholder?

The full list of allowed tags is documented in the MCMS manual, in the chapter *Mapping HTML Placeholder Definition Properties to Content Authoring Behavior*. The online version can be found at `http://msdn.microsoft.com/library/en-us/sitedevl/htm/cms_sd_dev_vsnetintegration_lcua.asp`.

Such restrictions are placed to prevent potentially dangerous tags from being entered and saved in placeholders. Such code could potentially cause infinite loops that open new browser windows, steal cookies, and even launch malicious applets or other controls.

Nevertheless, there may be times when your authors need to contribute content that contains these prohibited tags. Authors may need to embed client-side script, IFRAMES, applets, or ActiveX controls within placeholder content. Usually, these objects are defined within the template file, but doing so requires the developer to insert the code. Tech-savvy authors may wish to manage such content themselves, bypassing potential workflow bottlenecks caused by busy developers.

The default HtmlPlaceholder placeholder object does not allow all tags. To get around this, we will build a custom placeholder control. Let's call it the AllTagsHtmlPlaceholderControl. The control will have the same look and feel as the HtmlPlaceholderControl but it will modify the content so that the underlying HtmlPlaceholder no longer detects the disallowed tags when the posting is saved.

Here's the completed AllTagsHtmlPlaceholderControl in authoring view. It looks exactly like the HtmlPlaceholderControl. Authors won't even know the difference.

![](img/442e26bf28f2df81fbf8dff3478f88e3_285_0.png)

To fool the placeholder into accepting every tag, regardless of whether it's a simple bold tag <b> or a script tag <script>, we will replace all less-than (<) and greater-than (>) characters with the strings "~LT~" and "~GT~" respectively. As a result, what gets saved to the placeholder is just funny looking text like this:

```text
~LT~script~GT~
function Greet()
{
    alert(document.all.MyTextBox.value);
}
~LT~/script~GT~
```

The converted text does not contain any restricted tags. It is saved to the placeholder without any problems. Interestingly, the popular Telerik r.a.d. editor uses the same technique to allow special tags.

Since the conversion process is done when the posting is saved, we will modify the SavePlaceholderContent() method. Add the overridden SavePlaceholderContent() method directly below the constructor. Within the save, we will call a method named Escape() (defined right after this) that converts all tags to text.

```csharp
protected override void SavePlaceholderContent(PlaceholderControlSaveEventArgs e)
{
    EnsureChildControls();
    ((HtmlPlaceholder)(this.BoundPlaceholder)).Html = Escape(this.Html);
}
```

## Escaping Tags

The Escape() method simply replaces all less-than (<) characters with ~LT~ and all greater-than characters (>) with ~GT~.

It is probably worth noting that only HTML markup tags will contain the < and > characters. If an author enters these characters as part of the main text within the placeholder, MCMS encodes the less-than character to &lt; and the greater-than character to &gt;. Therefore we don't have to worry about the Escape() routine messing up content entered by the author.

```csharp
private string Escape(string input)
{
    return input.Replace("<", "~LT~").Replace(">", "~GT~");
}
```

## UnEscaping Tags

The act of reverting the ~LT~ and ~GT~ characters back to the < and > characters is done by the UnEscape() method. Add it directly below the Escape() method.

```csharp
private string UnEscape(string input)
{
    return input.Replace("~LT~", "<").Replace("~GT~", ">");
}
```

## Loading Content for Authoring and Presentation

To display the content, all we need to do is unescape the ~LT~ and ~GT~ strings back to the < and > characters respectively.

The first place where content is displayed is in authoring view when authors update the placeholder's content. We only need to convert the text when the placeholder is being edited, or when the Web Author is in the AuthoringReedit mode. The UnEscape() method defined earlier does the conversion. Add the overridden LoadPlaceholderContentForAuthoring() method shown below:

```
protected override void LoadPlaceholderContentForAuthoring(
    PlaceholderControlEventArgs e)
{
    EnsureChildControls();
    if (WebAuthorContext.Current.Mode == WebAuthorContextMode.AuthoringReedit)
    {
        base.Html = UnEscape(((HtmlPlaceholder)(base.BoundPlaceholder)).Html);
    }
    else
    {
        base.LoadPlaceholderContentForAuthoring(e);
    }
}
```

Uploaded content is, needless to say, also shown in presentation view. Regardless of whether or not the content is displayed when viewing the posting for preview or published modes, the ~LT~ and ~GT~ strings must be converted back to < and > characters respectively. We will do the conversion in the overridden LoadPlaceholderContentForPresentation() method. Append the method to the code:

```
protected override void LoadPlaceholderContentForPresentation(
    PlaceholderControlEventArgs e)
{
    EnsureChildControls();
    base.Html = UnEscape(((HtmlPlaceholder)(base.BoundPlaceholder)).Html);
}
```

The control is complete. Save and build the solution.

## Using the AllTagsHtmlPlaceholderControl

To use the AllTagsHtmlPlaceholderControl, copy the UsefulPlaceholderControl.dll library file to the bin directory or add it as a reference to the project. After adding the control to the Toolbox, drag and drop it onto template files as you would with regular placeholder controls. Set the PlaceholderToBind property of the control to an existing HtmlPlaceholderDefinition.

In addition, if authors are planning to save restricted tags like <script> blocks and <input> elements, you will need to set the AllowHtmlSourceEditing property of the control to true. The WYSIWYG editor does not allow authors to copy and paste the restricted tags in Design view. They will have to click on the HTML button on the authoring toolbar before pasting the HTML code into the control.
