```
    - - - code continues - - -
}
}
```

The code gets a reference to the `HierarchyItem` whose properties you wish to view. For template galleries, templates, resource galleries, and resources, this is obtained by getting the GUID from the `CMSObjectGuid` querystring parameter and using the `Searches.GetByGuid()` method. Channels and postings are obtained from the `CmsHttpContext.Current.ChannelItem` property.

In Design view, drag and drop the `Styles.css` file and `DataGrid` onto the `Properties.aspx` web form and give the `DataGrid` the following property values:

| Property | Value |
|---|---|
| Auto Format | Simple 3 |
| Font-Size | 10pt |
| ID | DataGrid1 |

![](bbox=[0.207, 0.367, 0.792, 0.66])

Below the Page_Load() event handler, add the `ListProperties()` method. The method first creates a `DataTable` containing the following columns:

+   - The property name
+   - The property value
+   - Whether or not the property can be written to

We use the `GetType.GetProperties()` method to retrieve a collection of all properties associated with the hierarchy item. Next, iterate through each property of the current `ChannelItem` and add each one as a row to the `DataTable`. Notice that the property value is obtained by calling `PropertyInfo.GetValue()` and passing in the hierarchy item as an input parameter. Finally, we bind the `DataTable` to the `DataGrid`.