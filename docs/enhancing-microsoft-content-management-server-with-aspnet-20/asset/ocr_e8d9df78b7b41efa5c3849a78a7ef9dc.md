```
```
"  {\n" +
"    __CrossPostBack_Backup_WebForm_DoPostBackWithOptions = " + 
"        WebForm_DoPostBackWithOptions;\n" + 
"    WebForm_DoPostBackWithOptions = " + 
"        __CrossPostBack_Redirect_WebForm_\n        DoPostBackWithOptions;\n" + 
"  }\n" + 
"  function __CrossPostBack_Redirect_WebForm_DoPostBackWithOption\ns(options)" + 
"  {\n" + 
"    __CMS_PostbackFormBeenReset = true;\n" + 
"    __CrossPostBack_Backup_WebForm_DoPostBackWithOptions\n    (options);\n" + 
"  }\n";
```
```

```
```csharp
public void OnInit(object sender, EventArgs eventArgs)
{
    System.Web.UI.Page currentPage = sender as System.Web.UI.Page;
    currentPage.ClientScript.RegisterStartupScript(this.GetType(),
        "Override_WebForm_DoPostBackWithOptions",
        Override_WebForm_DoPostBackWithOptions, true);
}
```
```

The class is complete. Save and build the project.

To use the HTTP module, add the following entry to the <HttpModules> section of the web.config project.

```
```xml
<add type="TropicalGreenHttpModules.CorrectCrossPostBack,
    TropicalGreenHttpModules"
    name="CorrectCrossPostBack"/>
```
```

Now, let’s see if the HTTP module has indeed corrected the problem. Attempt to order a plant from the ordering page. When the Order button is clicked, notice that the information gets posted correctly to the confirmation page.

# Tip #2: How to Implement an Ad Rotator

Advertisements are everywhere these days. On most commercial Internet websites, you will find some form of advertising either as banners running along the height or width of the page or as pop-up windows grabbing attention whenever a page loads.

Popular sites will probably have multiple advertisers requesting to put up their advertisements online. When the competition gets hot, a process must be devised to determine whose ads appear first, how the ads should be ordered, and when they should be published or taken offline.