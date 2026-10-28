# Friends

Our presenter then loads the display with all the current user's friends by calling into the FriendService and passing that collection down to the view.

```
///Fisharoo/Web/Friends/Presenters/DefaultPresenter.cs
public void LoadDisplay()
{
    _view.LoadDisplay(
        _friendService.GetFriendsAccountsByAccountID
            (_userSession.CurrentUser.
             AccountID));
}
```

The view then hooks up the repeater's data source. On each ItemDataBound of the repeater we spin up the ProfileDisplay user control

```
///Fisharoo/Web/Friends/Default.aspx.cs
protected void repFriends_ItemDataBound(object sender,
                                        RepeaterItemEventArgs e)
{
    if(e.Item.ItemType == ListItemType.Item || e.Item.ItemType ==
       ListItemType.AlternatingItem)
    {
        ProfileDisplay pdProfileDisplay =
            e.Item.FindControl("pdProfileDisplay") as ProfileDisplay;
        pdProfileDisplay.LoadDisplay(((Account)e.Item.DataItem));
    }
}
```

We then end up with the output as shown in the following screenshot:

![](bbox=[0.177, 0.581, 0.824, 0.724])

We have not done it yet, but down the road we are most likely to have some form of pagination implemented here too!