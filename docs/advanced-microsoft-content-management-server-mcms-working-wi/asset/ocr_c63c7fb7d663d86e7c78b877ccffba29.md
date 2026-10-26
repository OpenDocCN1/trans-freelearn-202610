As we are building the Web Part as a server control, we must declare our controls and instantiate them by overriding the CreateChildControls method in NavigationWebPart.cs:

```
#region declarations - Web controls
protected System.Web.UI.WebControls.Xml XmlXsIWebControl;
#endregion

#region Preparing the controls
protected override void CreateChildControls()
{
    XmlXsIWebControl = new System.Web.UI.WebControls.Xml();
    this.Controls.Add(XmlXsIWebControl);
}
#endregion
```

The System.Web.UI.WebControls.Xml Web Control can transform XML using specified XSLT, which is more efficient than developing the XML/XSLT by hand.

The RenderWebPart() method needs to be overridden to ensure that the rest of our code is being executed. Let's add the code to the RenderWebPart() method to generate the MCMS listing:

```
#region Methods
protected override void RenderWebPart(HtmlTextWriter output)
{
    // Check our child controls are created OK
    EnsureChildControls();

    try
    {
        // Get the Page listing from MCMS
        ExecuteMCMSListing(output);
    }
    catch(Exception ex)
    {
        // In production show only a short message with no internal exception details
        if (this.Mode == enumMode.Production)
        {
            // Suppress Exceptions
            output.Write("An Error Occurred in the Navigation Web Part.");
        }
        else
        {
            // Output detailed exception message (escape characters)
            output.Write(SPEncode.HtmlEncode(ex.Message + "," + ex.Source + "," + ex.StackTrace));
        }
    }
}
```

The next method, ExecuteMCMSListing(), determines which mode the Web Part is running in and retrieves the channel listing data. The channel listing data can be read from MCMS or can be loaded from the test XML file on the file system.