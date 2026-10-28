```
public void SendFriendRequest(Int32 AccountIdToInvite)
{
    _redirector.GoToFriendsInviteFriends(AccountIdToInvite);
}

public void DeleteFriend(Int32 FriendID)
{
    if (_userSession.CurrentUser != null)
    {
        _friendService.DeleteFriendByID(
            _userSession.CurrentUser.AccountID,
            FriendID);
        HttpContext.Current.Response.Redirect(
            HttpContext.Current.Request.RawUrl);
    }
}
```

The ProfileDisplay control also has a couple of properties—one to show/hide the delete friend button and the other to show/hide the invite friend button. These buttons are not appropriate for every page that the control is used in. In the search results page we want to hide the Delete button as the results are not necessarily friends. We would want to be able to invite them in that view. However, in a list of our friends the Invite button (to invite a friend) would no longer be appropriate as each of these users would already be a friend. The Delete button in this case would now be more appropriate.

Clicking on the Invite button makes a call to the Redirector class and routes the user to the InviteFriends page. An important point to note is the construction of the URL in the LoadDisplay() method, which takes you to the user's profile page.

You will recall that we had defined the ProfileRoute in previous chapter to support fancy URL directly linking to user's profile page. In order to create the link, we use the GetRouteURL helper method. This is useful if we end up changing our route pattern, as defined in Global.asax.cs, we don't need to make the changes to regenerate the URL. The GetRouteURL method will use the current route definition and return a valid URL.

# Invite your friends

This page allows us to manually enter email addresses of friends that we want to invite. It is a standard From, To, Message format where the system defaults the sender (to you), and you specify who to send to and the message that you want to send.