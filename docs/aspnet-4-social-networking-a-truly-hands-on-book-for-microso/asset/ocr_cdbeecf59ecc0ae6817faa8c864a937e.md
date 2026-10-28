# Forums

```
alertMessage = "<div class=\"AlertHeader\">" +
    GetProfileImage(_userSession.CurrentUser.AccountID) +
    GetProfileUrl(_userSession.CurrentUser.Username) + " has just added a new post to:<b>" +
    post.Name + "</b></div>";

alertMessage += "<div class=\"AlertRow\"><a href=\"" +
    webContext.RootUrl + "forums/" + category.Name +
    "/" + forum.Name + ".aspx/" + thread.PostID +
    "\">" + _webContext.RootUrl +
    "forums/" + category.Name + "/" + forum.Name +
    ".aspx/" + thread.PostID + "</a></div>";
alert.Message = alertMessage;
SaveAlert(alert);
SendAlertToFriends(alert);
}

public void AddNewBoardThreadAlert(BoardCategory category,
    BoardForum forum, BoardPost post)
{
    Init();
    alert.AlertTypeID = (int)AlertType.AlertTypes.
        NewBoardThread;
    alertMessage = "<div class=\"AlertHeader\">" +
        GetProfileImage(_userSession.CurrentUser.AccountID) +
        GetProfileUrl(_userSession.CurrentUser.Username) + " has just added a new thread
        on the board: <b>" +
        post.Name + "</b></div>";

    alertMessage += "<div class=\"AlertRow\"><a href=\"" +
        webContext.RootUrl + "forums/" + category.Name +
        "/" + forum.Name + ".aspx/" + post.PostID +
        "\">" + _webContext.RootUrl +
        "forums/" + category.Name + "/" + forum.Name +
        ".aspx/" + post.PostID + "</a></div>";
    alert.Message = alertMessage;
    SaveAlert(alert);
    SendAlertToFriends(alert);
}
```

# Implementing the Presentation layer

Now let's discuss the implementation of our presentation layer.