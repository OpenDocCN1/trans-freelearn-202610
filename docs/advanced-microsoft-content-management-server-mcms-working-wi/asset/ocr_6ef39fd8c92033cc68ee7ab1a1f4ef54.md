```
```csharp
protected override void Render( HtmlTextWriter writer )
{
    ChannelItem currentItem = CmsHttpContext.Current.ChannelItem;

    if( currentItem != null )
    {
        string followKey = (currentItem.IsRobotFollowable)?Follow:noFollow;
        string indexKey = (currentItem.IsRobotIndexable)?Index:noIndex;

        // write the robot META tag
        writer.Write("<meta name=\"ROBOTS\"\n"
                   + " content=\""+followKey+","+indexKey+"\">\n");

        if( this.RenderBaseHref )
        {
            // write the Base tag
            writer.Write("<base href=\""+Page.Request.Url.AbsoluteUri+"\">\n");
        }
    }
}
```
```

Save and build the solution. Add the control to the toolbar as you would other custom controls and it’s ready to be dropped into channel rendering scripts or template files between the `<head>` tags.

# Outputting META Tags

As well as the ROBOTS META tag, you can emit other META tags to help indexing by the particular search engine you are working with. Common tags used by search engines include the `<title>` tag and the Description and Keywords META tags.

In MCMS, channels and postings have Description and DisplayName properties, which are often suitable for the Description META tag and the `<title>` tag respectively.

> > Custom properties can also be used as META tags and rendered as `<meta`
> > `name="CustomPropertyName" content="CustomPropertyValue">`

Keywords can be entered by authors and stored as custom property values. What you will need to do then is to retrieve these values from the repository and display them as META tags in the head region of the page.

There are several ways you can go about this:

+   - The fastest method is to simply use `<%= %>` constructs or `Response.Write()` calls within the template HTML code.
+   - If content that resides in custom properties or placeholders, you can retrieve it using code-behind files.

Like regular controls, use a web user control to share code among template files in the same project. If you need to generate the same set of tags across pages stored in multiple projects, consider writing a custom control as we did for the ROBOTS tag earlier.