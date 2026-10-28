# Forums

```
```csharp
_presenter = new ViewForumPresenter();
string categoryname = Page.RouteData.Values["categoryname"] as string;
string forumname = Page.RouteData.Values["forumname"] as string;
_presenter.Init(this, categoryname, forumname);
}

public void LoadDisplay(List<PEBoardPost> Threads, string CategoryName, string ForumName, Int32 ForumID)
{
    //forum can be viewed by anyone but for posting one needs to login
    if (_presenter.IsUserLoggedIn())
    {
        //navigation URL will follow the URL routing pattern
        linkNewThread.NavigateUrl = Page.GetRouteUrl("NewThreadRoute", new
        {
            categoryname = linkCategoryName.Text,
            forumname = litForumName.Text,
        });
    }
    else
    {
        linkNewThread.Text = "Login to post to forums";
        linkNewThread.NavigateUrl = "/Accounts/Login.aspx";
    }
    repTopics.DataSource = Threads;
    repTopics.DataBind();
}

protected void repTopics_ItemDataBound(object sender, RepeaterItemEventArgs e)
{
    if (e.Item.ItemType == ListItemType.Item || e.Item.ItemType == ListItemType.AlternatingItem)
    {
        HyperLink linkViewTopic = e.Item.FindControl("linkViewTopic") as HyperLink;
        //navigation URL will follow the URL routing pattern
        linkViewTopic.NavigateUrl = Page.GetRouteUrl("ThreadRoute",
            new
            {
```
```