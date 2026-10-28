

在 presenter 中，我们使用从路由数据集合中获取的 groupname 加载特定群组数据，然后针对公开/私有查看做进一步处理。

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

除了显示数据之外，它还作为我们查看和管理与群组相关的其他部分（例如群组拥有的论坛）的起点。

## 私有/公开

判断群组是公开还是私有，只需检查数据库中设置的一个位标志即可；而判断谁能实际看到私有群组则稍显复杂（请参阅上面的 presenter 代码）。

回想一下，我们创建了一些辅助函数，用来判断用户是群组的所有者、成员还是管理员。在 ViewGroupPresenter 中，我们调用 ViewMembers 方法来确定用户所处的位置。

```csharp
//Fisharoo/Web/Groups/Presenters/ViewGroupPresenter.cs
public void ViewMembers()
{
    _redirector.GoToGroupsMembers(_webContext.GroupID);
}
```

这让我们能够快速轻松地判断是否可以显示私有群组。如果不能，则向用户显示相应消息，说明该群组为私有，并提示他们申请加入。当你请求成为成员时，会向群组所有者发送一条关于此请求的提醒。

```csharp
//Fisharoo/Web/Groups/Presenters/ViewGroupPresenter.cs
public void RequestMembership(string groupID)
{
    _alertService.AddGroupMembershipRequestAlert(gm.AccountID, gm.GroupID, _webContext.CurrentUser.Username);
}
```

![](img/15a62e134135dde036b0ac3cbb01dab6_332_0.png)

如果你是群组成员或群组所有者，则会看到群组描述下方的成员列表。

## MyGroups

MyGroups 页面是主页面的一个副本，因为它同样会流式展示一堆群组。最直接的区别是，此页面上的所有群组都属于当前查看者。除了安全检查和数据源变更之外，我们还提供了一些额外功能。在这个页面上，我们可以删除选中的群组，还可以链接到 ManageGroup.aspx 页面。

## 群组

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

## 论坛增强

前面我们讨论过，将会有群组拥有并运营的论坛。为了实现这一点，我们必须在论坛系统中添加一个新类别，专门称为**群组论坛（group-forums）**。然后，我们将该类别从论坛类别显示（论坛主页）中排除。这由 BoardCategoryRepository.GetAllCategories() 方法处理。论坛主页仍然看起来像这样：

![](img/15a62e134135dde036b0ac3cbb01dab6_334_0.png)

这里不存在任何群组类别！还要注意，显示中没有**海马（sea-horses）**论坛，而我们可以从**海马（sea-horses）**群组清晰地链接到**海马（sea-horses）**论坛。回想一下，前面我们讨论过，当 GroupService 创建群组时，我们也会创建一个新论坛，并将其附加到**群组论坛（group-forums）**类别（类别 4）。有了这个机制，当加载 ViewGroup 页面时，我们也可以加载该群组的论坛链接。

此外，回想一下我们向 AlertService 添加了几个方法。这些方法允许我们向群组的所有成员发送提醒，而不是仅向发帖人的好友发送提醒。到目前为止，我们还没涉及的集成细节是：发帖的论坛页面如何知道它是否是一个群组拥有（group-owned）的论坛。

为创建此功能而修改的论坛系统部分只有 PostPresenter.Save() 方法（上一章已介绍）。在该方法的末尾，我们添加了一些代码，用于判断是否可以根据 ForumID 加载群组。

> 请记住，此逻辑只有在以下情况下才有效：我们规定一个群组只能拥有一个论坛（反之亦然）。目前，我们是在创建群组时创建关联来实现这一点的。但是，如果你提供其他工具来创建论坛或群组，则需要记住，必须加入某种形式的验证，否则我们目前讨论的这种群组/论坛验证就需要更改！

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

有了这段代码，我们的论坛不仅知道自己属于某个群组，而且还能向群组成员（而非仅限于好友）发送提醒。

## 总结

有了这个核心框架，你可以看到群组如何将许多不同的系统汇集在一起，从而开始创建子社区。显然，这个部分还可以加入许多其他功能。就我目前想到的，以下几个功能的集成可能会为群组部分增添一些有趣的亮点：

- 消息
  - 你可以创建一个“发送到群组”的所见即所得（WYSIWYG）区域，向所有群组成员发送消息。
  - 此外，你还可以将用户加入的所有群组整合到消息系统中，这样群组名称就可以作为联系人名称，直接从邮件部分群发消息。
- 好友
  - 群组成员可以很方便地整合为好友。
  - 你不仅可以在用户配置中指定公开、私有和好友区域，还可以添加类似“我所属群组的成员”这样的筛选器。

## 11 用户交互

让我们的用户能够对网站内容进行评论、添加标签和评分，可以为他们提供一定程度的交互性，使他们愿意一次又一次地回来。这些功能之所以能激起用户的好奇心，主要有两个原因。首先，对任何可接触到的事物发表看法是人类的天性。一旦第一群用户对所有内容都发表了自己的看法，你会发现第二群用户会开始关注并与那些评分最高、评论和标签最多的内容进行互动。

在本章中，我们将深入探讨如何创建一个灵活的系统，使我们可以为系统中的任何对象（在数据库级别）添加标签、评分、投票和评论。我们将把标签、评分和评论系统应用到照片库部分，并将投票、标记为答案和勋章应用到论坛部分。标签和评论会相对直接，因为它们只是获取用户的原始输入并将其与各个内容区域关联。另一方面，评分系统将使用 ASP.NET AJAX 库，在带有评分控件的模态窗口中提供多个属性，这些属性聚合后就会为该条目提供平均分。

