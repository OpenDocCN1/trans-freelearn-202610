```
public void repCategories_ItemDataBound(object sender,
RepeaterItemEventArgs e)
{
    if (e.Item.ItemType == ListItemType.Item || e.Item.ItemType == ListItemType.AlternatingItem)
    {
        if (((PEBoardCategory) e.Item.DataItem).Forums != null)
        {
            Repeater repForums = e.Item.FindControl("repForums") as Repeater;
            repForums.DataSource = ((PEBoardCategory) e.Item.DataItem).Forums;
            repForums.DataBind();
        }
    }
}
```

```
public void repForums_ItemDataBound(object sender,
RepeaterItemEventArgs e)
{
    HyperLink linkForum = e.Item.FindControl("linkForum") as HyperLink;
    linkForum.NavigateUrl = Page.GetRouteUrl("ForumRoute", new {
        categoryname = ((PEBoardForum) e.Item.DataItem).CategoryName,
        forumname = ((PEBoardForum)e.Item.DataItem).Name
    });
}
```

In each iteration of the repCategories_ItemDataBound method, we then load the repForums repeater with the list of forums for the given category (PEBoardForum) and bind it. This continues until the list of categories and forums is thrown on the page.

Also notice that when each forum item is bound, we set the appropriate URL to the hyperlink control's NavigateUrl property. This is the friendly URL and will take us to the specific forum page.