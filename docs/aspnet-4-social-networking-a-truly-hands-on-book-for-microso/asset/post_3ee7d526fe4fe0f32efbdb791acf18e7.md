
In the presenter we load the specific group data using the groupname that we got from the route data collection and then do some more processing with regards to public/private viewing.

```csharp
//Fisharoo/Web/Groups/ViewGroupPresenter.cs
public void LoadData(string groupname)
{
    Group group = _groupService.GetGroupByPageName(groupname);
    string url = "/files/photos/" + _uploadService.GetFullFilePathByFileID(group.FileID, File.Sizes.S);
    _view.LoadData(group, url);
    if(_webContext.CurrentUser != null)
        _view.ShowRequestMembership(true);
    else
        _view.ShowRequestMembership(false);

    //is this public or private data?
    if (group.IsPublic)
    {
        _view.ShowPrivate(true);
        _view.ShowPublic(true);
    }
    else if (ViewerIsMember(group.GroupID))
    {
        _view.ShowPrivate(true);
        _view.ShowPublic(true);
    }
    else
    {
        _view.ShowPrivate(false);
        _view.ShowPublic(true);
    }
}
```

In addition to displaying data, it will act as our jumping off point for viewing and managing other sections that are related to the group—such as the group-owned forum.

## Private/Public

Determining whether a group is public or private is performed simply by checking a bit flag set in the database; determining who can actually see a private group is a bit more complex (refer to the presenter code above).

Recall that we had created a few help functions to determine if a user was the owner of the group, a member of the group, or an administrator of the group. In our ViewGroupPresenter, we make a call to our ViewMembers method to determine where the user stands.

```csharp
//Fisharoo/Web/Groups/Presenters/ViewGroupPresenter.cs
public void ViewMembers()
{
    _redirector.GoToGroupsMembers(_webContext.GroupID);
}
```

This lets us know quickly and easily if we can display the private group. If not, then the user is shown the appropriate message stating that the group is private and they are prompted to request membership. When you request to become a member, an alert is sent to the group owner for this request.

```csharp
//Fisharoo/Web/Groups/Presenters/ViewGroupPresenter.cs
public void RequestMembership(string groupID)
{
    _alertService.AddGroupMembershipRequestAlert(gm.AccountID, gm.GroupID, _webContext.CurrentUser.Username);
}
```

![](img/15a62e134135dde036b0ac3cbb01dab6_332_0.png)

If you are part of the group or the owner of the group, you would instead see the list of members below the group description.

## MyGroups

The MyGroups page is a duplication of the homepage in that it streams a bunch of groups. The immediate difference is that all the groups on this page belong to the viewer of the page. In addition to the security check and changed data source, we also offer a couple of additional features. From this page, we can delete the selected group. We also have the ability to link to the ManageGroup.aspx page.

## Groups

```csharp
LinkButton lbPageName = e.Item.FindControl("lbPageName") as LinkButton;
ImageButton ibDelete = e.Item.FindControl("ibDelete") as ImageButton;
Literal litGroupID = e.Item.FindControl("litGroupID") as Literal;
ImageButton ibEdit = e.Item.FindControl("ibEdit") as ImageButton;
ibDelete.Attributes.Add("GroupID", litGroupID.Text);
ibEdit.Attributes.Add("GroupID", litGroupID.Text);
ibDelete.Attributes.Add("onclick", "return confirm(\"Are you sure you want to delete this group?\");");
lbPageName.Attributes.Add("PageName", litPageName.Text);
imgGroupImage.ImageUrl = "/files/photos/" + _presenter.GetImageByID(Convert.ToInt64(litImageID.Text), File.Sizes.S);
}
}
}
```

## Forum enhancements

We discussed earlier that we would have Group-owned-and-operated Forums. To pull this off, we had to add a new category to the board system specifically called **group-forums**. We then restricted that category from the board categories display (the forum homepage). This is handled in the BoardCategoryRepository.GetAllCategories() method. The forum homepage still looks like this:

![](img/15a62e134135dde036b0ac3cbb01dab6_334_0.png)

No group category exists here! Also note that there isn't a **sea-horses** forum present in the display whereas we can clearly link to the **sea-horses** forum from the **sea-horses** group. Recall that earlier we discussed when a group is created by the GroupService, we also create a new forum and attach it to the **group-forums** category (category 4). With this in place, when we load up the ViewGroup page, we can also load in the forum link for that group.

In addition to this, recall that we added a couple of methods to the AlertService. These methods allowed us to send alerts to all the members of the group rather than the friends of the poster. The only thing that we haven't covered up to this point to pull off this integration is how the posting forum page knows if it is a group-owned forum or not.

The only part of the forum system that was modified to create this addition is the PostPresenter.Save() method (covered in last chapter). At the very bottom of this method we added a bit of code to determine if we could load a group based on the ForumID or not.

> Recall that this logic is valid only if we specify that a group can own only one forum (and vice versa). We currently do that by only creating the link at the time the group is created. However, if you provide other tools for forum or group creation, you will need to keep in mind that some form of validation will need to be put in place, or this group/forum validation that we are currently discussing will need to be changed!

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
    else
        _alertService.AddNewBoardPostAlert(category, forum, post, thread);
}

RouteValueDictionary values = new RouteValueDictionary {
    {"categoryname", category.Name}, {"forumname", forum.Name},
    {"postid", thread.PostID} };
VirtualPathData vpd = RouteTable.Routes.GetVirtualPath(null,
    "ThreadRoute", values);
_redirector.GoToForumsViewPost(vpd.VirtualPath);
```

With this code in place, our forum is not only aware that it is part of a group but is also able to send alerts to group members other than friends.

## Summary

With this core framework in place you can see how groups can be used to bring many different systems together in a way that you start to create sub-communities. Obviously, this section has many other features which could be added to it. From the top of my mind I can think of integration in terms of the following features that might make some interesting additions to the group section:

- Messaging
  - You could create a "send to group" WYSIWYG section that sends messages to all the group members.
  - Additionally, you could integrate all the groups that a user is a member of into the messaging system so that the group name could be used as a contact name to blast messages directly from the mail section.
- Friends
  - Group members can be integrated easily as friends.
  - Not only can you specify public, private, and friends' areas in your user profiles but also have a filter such as members of groups I am part of.

## 11 User Interactivity

Giving our users the ability to comment, tag, and rate the content on our site provides them a level of interactivity that will make them want to come back again and again. There are mainly two reasons for these features to arouse the curiosity of users. First, it is human nature to want to provide their opinions about anything available to them. Once a group of users has offered its opinion on everything, you will find that a second group of users will rear their heads to look at and interact with the highest rated, most heavily commented and tagged content.

In this chapter, we will delve into what it takes to create a flexible system that will allow us to tag, rate, vote, and comment any object in our system (at the database level). We will apply tagging, rating, and commenting systems to our photo gallery section and vote, mark as answer, and medals to our forums section. Tagging and commenting will be somewhat straightforward in that they will simply take raw user input and tie it to the various content areas. The rating system on the other hand will use the ASP.NET AJAX library to provide multiple attributes in a modal window with the rating control, which when aggregated will provide us with an average score for that item.
