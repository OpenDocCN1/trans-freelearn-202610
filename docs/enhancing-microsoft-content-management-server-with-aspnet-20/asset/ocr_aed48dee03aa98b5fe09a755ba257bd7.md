Notice that we have set the PostBackUrl property of the button to point to a page named checkout.aspx (which we will be building in the next section). Instead of having the results post back to the current page, the data will be sent to checkout.aspx.

+   5. Double-click anywhere on the form to get to the code-behind file. At the top of the page, add the highlighted namespace.

```
using System;
... code continues ...
using Microsoft.ContentManagement.Publishing;

public partial class Forms_Order : System.Web.UI.Page
{
    ... code continues ...
}
```

+   6. Within the Page_Load() event handler, we will populate the drop-down list with a list of all plants available in the catalog. Add the code as shown here:

```
protected void Page_Load(object sender, EventArgs e)
{
    // Get the current context
    CmsHttpContext cmsContext = CmsHttpContext.Current;

    // Get an instance of the plant catalog
    Channel plantCatalog = cmsContext.Searches.GetByPath(
        "/Channels/tropicalgreen/PlantCatalog") as Channel;

    if (plantCatalog != null)
    {
        // Get a collection of all postings in the plant catalog
        // channel
        PostingCollection plants = plantCatalog.Postings;

        // Sort the postings by display name
        plants.SortByDisplayName();

        // Populate the dropdown with a list of plants from the
        // catalog
        foreach (Posting plant in plants)
        {
            ListItem li =
                new ListItem(plant.DisplayName, plant.DisplayName);
            ddlPlant.Items.Add(li);
        }
    }
}
```