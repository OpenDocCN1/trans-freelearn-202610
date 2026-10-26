# Useful Placeholder Controls

Usually, when building custom placeholder controls, the logic for adding these controls is coded by overriding the `CreateAuthoringChildControls()` method. We won't do that here because we will be making heavy use of JavaScript within our control. The easiest and most fuss-free way of adding all that client-side script would be to code in the old fashion way: as regular HTML.

Therefore, in the overridden `CreateAuthoringChildControls()` method, we will simply add a single Literal web control. Later on, we will attach HTML code to the Text property of this Literal control. Add the following code below to the ResourceGalleryOnly property:

```
private System.Web.UI.WebControls.Literal baseAuthoringContainer;

protected override void CreateAuthoringChildControls
    (BaseModeContainer authoringContainer)
{
    baseAuthoringContainer = new Literal();
    authoringContainer.Controls.Add(baseAuthoringContainer);
}
```

Now, let's create the table. Because the table is dynamically generated, with placeholder content mixed with HTML, we'll code it in the overridden `LoadPlaceholderContentForAuthoring()` method.

The table consists of five columns:

| Column 1 | Column 2 | Column 3 | Column 4 | Column 5 |
|---|---|---|---|---|
| Displays the attachment as an icon. The icon shown is related to the type of the file. For example, Office Word documents will have icons that look as shown below. | Contains a TextBox control that shows the display text of the attachment. It also holds a hidden form field for storing the URL of the icon displayed in Column 1. It's hidden, so you can't see it in presentation view, but its value will be used by the client-side script that we will be coding later on. | Contains a read-only TextBox that shows the URL of the attachment. | Provides a button labeled Change... that triggers the upload process. | Contains a Delete button to remove the attachment from the list. |
| Doc1.doc | Alt-Text: Doc1.doc | Attachment-URL: /NR/rdonlyres/00000015/i | Change... | Delete |

As we are writing the code from a class file, we can't insert HTML directly as we would with regular HTML or ASPX files. Instead, we will write the code as a string variable and add it to the Text property of the Literal control we created earlier. We shall start with the table's header. Append the following code to the class file:

```
private string crlf = "\n";

protected override void
LoadPlaceholderContentForAuthoring(PlaceholderControlEventArgs e)
```