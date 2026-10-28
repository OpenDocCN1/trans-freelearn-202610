The following screenshot shows how the search results get displayed:

![](bbox=[0.178, 0.176, 0.822, 0.309])

The fun stuff in this case comes in the form of the **ProfileDisplay** user control that was created so that we have an easy way to display concise profile data in various places with one chunk of reusable code that will allow us to make global changes.

You should see that we have `<Fisharoo:ProfileDisplay ... />`. You will also notice that our tag has custom properties that are set in the tag definition. In this case you see `ShowDeleteButton="false"`.

All the logic and display is very standard. You have the MVP plumbing, which makes up most of it. The **ProfileDisplay** control also has a `LoadDisplay()` method responsible for loading the UI for that control. This is invoked in the Search page in the `repAccounts_ItemDataBound()` method.

```
```csharp
//Fisharoo/Web/Search.aspx.cs
protected void repAccounts_ItemDataBound(object sender,
                                         RepeaterItemEventArgs e)
{
    if(e.Item.ItemType == ListItemType.Item || e.Item.ItemType ==
                                      ListItemType.AlternatingItem)
    {
        ProfileDisplay pd = e.Item.FindControl("pdProfileDisplay") as
                                      ProfileDisplay;
        pd.LoadDisplay((Account)e.Item.DataItem);
        if(_webContext.CurrentUser == null)
            pd.ShowFriendRequestButton = false;
    }
}
```
```

Let's look at **ProfileDisplay** user control code in order of display, code behind, and the presenter (omitting the interface of the view as usual).

```
```aspx
//Fisharoo/Web/UserControls/ProfileDisplay.ascx
<%@ Control Language="C#" AutoEventWireup="true"
CodeBehind="ProfileDisplay.ascx.cs"
```
```