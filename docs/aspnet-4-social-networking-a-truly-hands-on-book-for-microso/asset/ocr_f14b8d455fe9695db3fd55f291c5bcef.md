In addition to this, recall that we added a couple of methods to the AlertService. These methods allowed us to send alerts to all the members of the group rather than the friends of the poster. The only thing that we haven't covered up to this point to pull off this integration is how the posting forum page knows if it is a group-owned forum or not.

The only part of the forum system that was modified to create this addition is the PostPresenter.Save() method (covered in last chapter). At the very bottom of this method we added a bit of code to determine if we could load a group based on the ForumID or not.

> Recall that this logic is valid only if we specify that a group can own only one forum (and vice versa). We currently do that by only creating the link at the time the group is created. However, if you provide other tools for forum or group creation, you will need to keep in mind that some form of validation will need to be put in place, or this group/forum validation that we are currently discussing will need to be changed!

```
```csharp
//Fisharoo/Web/Forums/Presenters/PostPresenter.cs
if (post.IsThread)
    thread = _postService.GetPostByID(post.PostID);
else
    thread = _postService.GetPostByID((long)post.ThreadID);

//CHAPTER 10- is this forum part of a group?
Group group = _groupService.GetGroupByForumID(forum.ForumID);

//add an alert to the filter
if (post.IsThread)
{
    //CHAPTER 10- is this a group forum?
    if (group != null)
        _alertService.AddNewBoardThreadAlert(category, forum,
            thread, group);
    else
        _alertService.AddNewBoardThreadAlert(category, forum, thread);
}
else
{
    //CHAPTER 10- is this a group forum?
    if (group != null)
        _alertService.AddNewBoardPostAlert(category, forum, post,
            thread, group);
```

```