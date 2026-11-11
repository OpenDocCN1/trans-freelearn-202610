```
// Add each posting to the site map
foreach (Posting p in pc)
{
    smnc.Add(GetSiteMapNodeFromChannelItem(p));
}
}
return smnc;
}
```

6. The class is complete. Save the MCMSSiteMapProviderSummary class file.

7. As before, we need to add a reference to the custom site-map provider in the web.config file before we can use it. Add the highlighted line to the <providers> section of the web.config file.

```
<providers>
    . . . code continues . . .
    <add name="MCMSSiteMapProviderSummary"
        type="TropicalGreen.MCMSSiteMapProviderSummary"/>
</providers>
```

# Configuring the TreeView Control to Display a Flat Listing of Items

Now that we have created the custom site-map provider that provides a listing of all pages within a given channel, we need to find a control that is capable of displaying the list. Out of the box, we have a choice of using either the SiteMapPath, Menu, or TreeView controls. For the purpose of this example, let’s pick the TreeView control.

It may seem counterintuitive to use the TreeView control to display a flat listing of items. After all, it was designed to show a hierarchical list. Nevertheless, we shall see that the TreeView control is a very versatile control, able to display hierarchical nested items as well as a single-dimension flat list.

+   1. First, create a new web form in the /Templates/ folder of the TropicalGreen project. Name the file ChannelRenderingScript.aspx. Choose to select a master page.
+   2. In the Select a Master Page dialog, choose the tropicalgreen.master file located in the Templates folder.
+   3. With ChannelRenderingScript.aspx opened in Design view, drag a SiteMapDataSource control from the Data section of the toolbox and drop it anywhere in the content area. Give the control the following property values: