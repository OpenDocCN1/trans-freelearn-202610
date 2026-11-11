7. The IHttpModule requires the implementation of both the Init() and Dispose() methods. Add the following code to the CorrectLinksInThemes class file:

```
public class CorrectLinksInThemes : IHttpModule
{
    public void Init(HttpApplication httpApp)
    {
    }

    public void Dispose()
    {
    }
}
```

8. As all <link> tags stored in the page are analyzed before the page is rendered, we will write the code in the OnPreRender() event handler of the page object. The code loops through each control found in the page header. If the control is a <link> tag, it checks if the link is relative, and converts it to an absolute link if necessary. Add the OnPreRender() event handler below the Dispose() method.

```
public void OnPreRender(object sender, EventArgs eventArgs)
{
    Page currentPage = sender as Page;
    HtmlHead pageHeader = currentPage.Header as HtmlHead;
    if (pageHeader != null)
    {
        foreach (Control control in pageHeader.Controls)
        {
            HtmlLink link = control as HtmlLink;
            if (link != null &&
                VirtualPathUtility.IsAppRelative(link.Href))
            {
                link.Href = VirtualPathUtility.ToAbsolute
                    (link.Href);
            }
        }
    }
}
```

9. We need to register the OnPreRender() event handler that we have written earlier to the PreRender() event of the page. A good place to do so would be within the OnPreRequestHandlerExecute() event handler of the current