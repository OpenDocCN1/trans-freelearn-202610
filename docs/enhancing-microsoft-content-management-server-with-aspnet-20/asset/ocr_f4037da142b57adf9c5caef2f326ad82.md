```
if (c.CustomProperties["MenuLocation"].Value == "Right")
{
    smnc.Add(GetSiteMapNodeFromChannelItem(c));
}
}
else if(c.Parent.Path!=base.RootChannel)
{
    // Add sub menu items
    smnc.Add(GetSiteMapNodeFromChannelItem(c));
}
}
}
return smnc;
}
```

+   5. The class is complete. Save the file.
+   6. Before the `MCMSSiteMapProviderRight` class can be used, we need to register it in the web.config file.

```
<siteMap defaultProvider ="MCMSSiteMapProvider" enabled ="true">
    <providers>
        <add name="MCMSSiteMapProvider"
            type="TropicalGreen.MCMSSiteMapProvider"/>
        <add name="MCMSSiteMapProviderTop"
            type="TropicalGreen.MCMSSiteMapProviderTop"/>
        <add name="MCMSSiteMapProviderRight"
            type="TropicalGreen.MCMSSiteMapProviderRight"/>
    </providers>
</siteMap>
```

The `SiteMapProviderRight` class has been built and registered. Let’s add a `SiteMapDataSource` and menu control to the `tropicalgreen.master` template.

+   1. With `tropicalgreen.master` opened, drag a **SiteMapDataSource** control from the **Data** section of the toolbox and assign the following property values:

| Property | Value |
|---|---|
| ID | SiteMapDataSourceRight |
| SiteMapProvider | MCMSSiteMapProviderRight |
| ShowStartingNode | False |