
5. Next, we will inherit from the AdRotator class. This will jumpstart our control to have all the functionalities of the out-of-the-box Ad Rotator control. We will add some component design values so that the control will be given proper tag names when developers drag and drop it from the toolbox.

```
[ToolboxData("<{0}:MCMSAdRotator runat=server></{0}:MCMSAdRotator>")] public class MCMSAdRotator : AdRotator
{
}
```

6. We will provide a property named AdsGalleryPath, which will contain the path to the resource gallery that contains the images. Insert the code as shown within the class declaration.

```
public class MCMSAdRotator : AdRotator
{
    // Property to get and set the path of the
    // resource gallery that contains the Ads
    private string adsGalleryPath;

    [Bindable(true)]
    [Category("Behavior")]
    [DefaultValue("")]
    [Localizable(true)]
    public string AdsGalleryPath
    {
        get { return adsGalleryPath; }
        set { adsGalleryPath = value; }
    }
}
```

7. The advertisement to be displayed will be picked randomly from the resource gallery. The Randomizer() method will pick a number from 1 to the number of items in the gallery at random. Here, we will use the Random.Next() method to generate the random number. We could go for more sophisticated methods of generating the random number, but for now, this will suffice. Add the following code below the AdsGalleryPath property.

```
private int Randomiser(int max)
{
    // Generate a random number
    int index = 0;
    Random rand = new Random();
    index = rand.Next(0, max);

    return index;
}
```

8. After the Ad Rotator control loads but before the page loads, the control’s AdCreated() event is called. Here, we will first get an instance of the resource gallery and the collection of resources that make up our advertisements. We will retrieve only images—other file formats like Office documents will be ignored. Following this, we will call the Randomiser() method to select the advertisement to display. Once an advertisement has been selected, we set the control’s properties to show the resource, link it to the URL specified in the resource’s description, and set the image’s tooltip (or alternate text) to the resource’s display name. Append the OnAdCreated() event handler to the code.

```
protected override void OnAdCreated(AdCreatedEventArgs e)
{
    // Get an instance of the resource gallery that contains the Ads
    CmsHttpContext cmsContext = CmsHttpContext.Current;
    ResourceGallery rg =
        (ResourceGallery)cmsContext.Searches.GetByPath(adsGalleryPath);

    if (rg != null)
    {
        // Get only files that are images
        ResourceCollection ads = rg.Resources;
        ads.FilterByMimeType("image", true);

        // Randomly pick one of the Ads
        int resourceIndex = Randomiser(ads.Count);
        Resource r = ads[resourceIndex];

        // Display the image
        e.ImageUrl = r.Url;
        e.NavigateUrl = r.Description;
        e.AlternateText = r.DisplayName;
    }
}
```

9. The control is complete. Save the file and compile the project.

To see the control in action, first add it to the toolbox by right-clicking anywhere within the toolbox and selecting Choose Items. In the Choose Toolbox Item dialog, click browse and select the TropicalGreenControlLib.dll file (usually found in the \bin\debug\ folder of the project). Once the control has been added to the Toolbox, drag and drop the control anywhere on a web form or template file. Set the AdsGalleryPath property to contain the path of an existing resource gallery item, such as /Resources/TropicalGreen/PlantCatalog. The next time you view the page, you will see rotating advertisements.

## Tip #3: Considerations for Microsoft Office SharePoint Server 2007

Web Content Management (WCM) features that are today in MCMS will be part of the upcoming Microsoft Office SharePoint Server 2007 (MOSS 2007). MOSS 2007 will provide vastly richer capabilities as well:

- SharePoint and MCMS will ship as a single product, and henceforth, there will be no more confusion between the two. Along with WCM features, you will get the full suite of SharePoint capabilities such as:
  - Document management
  - Enterprise search
  - Surveys and discussion boards
  - Lists and alerts
  - Built-in support for portals and web parts
- In addition, MOSS 2007 includes several new exciting features:
  - Wikis and blogs.
  - An enhanced web-authoring experience (ActiveX components and client installations of Site Manager not required). Includes time-saving power tools such as recycle bins and one-click rollbacks to previous versions.
  - All the functionality in Site Manager is now part of the Publishing API. For example, you can programmatically administer roles, groups, and users, and other tasks that were not possible in the current version of the PAPI.
  - Extensible workflow built upon Windows Workflow Foundation.
  - Extreme scalability. Capable of handling millions of items in a single container.

MOSS 2007 will ship with many more new and enhanced features. For a complete list, take a look at the Microsoft Office SharePoint Server 2007 Guide available for download at the 2007 Microsoft Office system preview site (http://www.microsoft.com/office/preview/servers/sharepointserver/highlights.mspx).

For those of us who have an existing installation of MCMS and are planning to migrate to MOSS 2007, a key point to note is that WCM applications under Office SharePoint Server 2007 will use an entirely different set of APIs from the current version of MCMS. Part of the reason for the major code overhaul is the integration of the MCMS base architecture with that of Windows SharePoint Services (version 3.0).

While the complete revamp of the Publishing API will probably mean that most of the custom code that we have written for our web sites will have to be re-coded, the good news is: there will be a one-to-one mapping of functionality for most of the current properties and methods. Details may be found in the article, Mapping MCMS 2002 APIs to SharePoint 2007 (http://msdn.microsoft.com/office/server/moss/2007/migration/default.aspx?pull=/library/en-us/dnmscms02/html/CMSISVersionAPIDiffs.asp).

In addition, Microsoft will be providing migration utilities to ease the transition. The CMS Assessment Tool scans a site and generates various reports (e.g. PAPI Usage Summary Report, PAPI Class Usage Report, etc.) that will help you gauge the amount of work required to re-code your site to be ready for MOSS 2007. The CMS Assessment tool is available for download at Microsoft’s web site: http://www.microsoft.com/downloads/details.aspx?FamilyId=360D0E83-FA70-4C24-BCD6-426CAFBCC627&displaylang=en.

For various reasons, you may not wish to migrate to Microsoft Office SharePoint Server 2007 immediately and choose to continue building your site using MCMS 2002. If that is your intention, you can take steps today to ease your migration in the future. You could:

- Isolate code that uses the PAPI into modules.
- Use the Provider Model design pattern as far as possible.

## Isolate Code that Uses the PAPI into Modules

The move to change the underlying MCMS PAPI has a huge impact on existing web applications should you plan to upgrade to MOSS 2007 in the future. Unless you have used the out-of-the box solution without any form of customization, chances are you have used the PAPI to tailor the solution to meet the requirements of your website.

All the customizations that you have done using the PAPI will have to be rewritten. While rewriting code is inevitable, you can plan the code structure in such a way that the re-coding process is made as painless as possible. A good idea would be to write modular code and isolate code that uses the PAPI as far as possible. In this way, when the time comes to upgrade the website, the code that requires rewriting will be easier to locate.

Take, for example, a fairly common scenario of a custom control that lists all pages on the site that have been modified in the last seven days. Previously, the list could be programmed to be displayed in a DataGrid. The list is retrieved from the PAPI using the `Searches.NewPostings()` method. Also, when the page loads, the list is bound directly to the DataGrid using the `DataGrid.DataBind()` method. Here’s an example of how the code would probably look:
