```
else
{
    // try to get the object via the URL
    return CmsHttpContext.Current.Searches.GetByUrl(url);
}
// if this point reached, unknown URL
return null;
}
catch
{
    return null;
}
```

+   3. Now, find the `rptSearchResults_ItemDataBound()` event handler. We need to trap the event when it's binding a data item to the `ItemTemplate` or `AlternateItemTemplate` in the Repeater. Then, we'll get a reference to the data item being bound to the template, in our case a DataRow, and get references to the ASP.NET objects we added to the template. Finally, we'll use the data in the DataRow to populate the properties of our controls. Here's what our completed `ItemDataBound()` event handler will look like:

```
private void rptSearchResults_ItemDataBound(object sender,
    System.Web.UI.WebControls.RepeaterItemEventArgs e)
{
    if ((e.Item.ItemType == ListItemType.AlternatingItem)
        || (e.Item.ItemType == ListItemType.Item))
    {
        // get a reference to the datarow being bound
        DataRow row = e.Item.DataItem as DataRow;
        HierarchyItem hi = GetResult(row[0].ToString());

        // get references to all the ASP.NET objects
        PlaceHolder resultContainer = e.Item.FindControl("phdSearchResult")
                                      as PlaceHolder;
        HyperLink resultTitle = e.Item.FindControl("hlkResultTitle")
                                as HyperLink;
        Literal resultDesc = e.Item.FindControl("litResultDescription")
                             as Literal;

        // if the URL doesn't resolve to an MCMS resource,
        // output it to the results
        if (hi != null)
        {
            // user has rights to this item so display it.
            resultContainer.Visible = true;

            // use values in DataRow to populate objects
            resultDesc.Text = hi.Description;
            if (hi is ChannelItem)
            {
                resultTitle.Text = (hi as ChannelItem).DisplayName;
                resultTitle.NavigateUrl = (hi as ChannelItem).Url;
            }
            else
            {
                if (hi is Resource)
                    resultTitle.NavigateUrl = (hi as Resource).Url;
                resultTitle.Text = hi.Name;
            }
        }
    }
}
```