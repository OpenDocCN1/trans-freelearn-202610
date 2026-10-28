

## URL 路由

为了支持用户友好的 URL，我们将再次使用 URL 路由功能。我们的附加代码将插入到 Global.asax.cs 中的 RegisterRoute() 方法中。我们将为论坛支持四种不同的 URL 模式。让我们快速查看每种模式：

- 像 **http://server/appname/forums/categoryname/forumname.aspx** 这样的 URL 用于访问特定类别中的特定论坛。此请求将由 **ViewForum.aspx** 处理。
- 像 **http://server/appname/forums/categoryname/forumname/postid** 这样的 URL 用于访问论坛中的特定帖子/主题。此请求将由 **ViewPost.aspx** 处理。
- 像 **http://server/appname/forums/categoryname/forumname.aspx/new** 这样的 URL 用于在论坛中创建新主题。此请求将由 **Post.aspx** 处理。
- 像 **http://server/appname/forums/categoryname/forumname.aspx/reply** 这样的 URL 用于在论坛中创建新主题。此请求将由 **Post.aspx** 处理。

您可以查看代码，了解这些模式是如何形成并添加到路由集合中的。

### ViewForum.aspx

视图论坛页面负责显示单个论坛及其所有主题，如本章第二张截图所示。

视图论坛页面首先从路由数据集合中提取类别名称和论坛名称，并将其传递给呈现器。呈现器返回特定论坛中所有主题的集合。视图拿到这个集合后，便填充页面的显示内容。在每次遍历主题时，repTopics_ItemDataBound 方法会加载 linkViewTopic HyperLink 控件的 NavigateUrl 属性，该属性是指向特定帖子页面的花式路径。该路径的一部分是 IsThread=1 变量。原因是这个页面是一个双用途页面，既用于创建帖子，也用于创建主题。

```csharp
//Fisharoo/Web/Forums/ViewForum.aspx.cs
public partial class Forum : System.Web.UI.Page, IViewForum
{
    private ViewForumPresenter _presenter;

    protected void Page_Load(object sender, EventArgs e)
    {
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
                    categoryname = linkCategoryName.Text,
                    forumname = litForumName.Text,
                    postid = ((PEBoardPost)e.Item.DataItem).PostID
                });
        }
    }
}
```

> 你可能不会想到此页面会直接加载每个主题的 HyperLink 导航。我们没有经过 Redirector 类。原因是为了性能。这样我们可以避免回发到同一页面，然后使用重定向类执行 Response.Redirect，再加载页面。相反，我们使用用户友好的 URL 直接跳转到帖子页面。

### ViewPost.aspx

查看帖子页面提取类别名称、论坛名称和主题 ID，并使用这些信息显示主题及其所有帖子，如本章第三张截图所示。

```csharp
//Fisharoo/Web/Forums/ViewPost.aspx.cs
public partial class ViewPost : System.Web.UI.Page, IViewPost
{
    private ViewPostPresenter _presenter;

    protected void Page_Load(object sender, EventArgs e)
    {
        _presenter = new ViewPostPresenter();
        string categoryname = Page.RouteData.Values["categoryname"] as string;
        string forumname = Page.RouteData.Values["forumname"] as string;
        int postID = Convert.ToInt32(Page.RouteData.Values["postid"]);
        _presenter.Init(this, IsPostBack, categoryname, forumname, postID);
    }

    public void LoadData(PEBoardPost Thread, List<PEBoardPost> Posts, string CategoryName, string ForumName)
    {
        linkForumName.NavigateUrl = Page.GetRouteUrl("ForumRoute", new
        {
            categoryname = CategoryName,
            forumname = ForumName
        });

        linkUsername.Text = Thread.Username;
        linkUsername.NavigateUrl = "~/" + Thread.Username;
        lblCreateDate.Text = Thread.CreateDate;
        lblThreadTitle.Text = Thread.Name;

        lblPost.Text = Thread.Post;
        imgProfile.ImageUrl = "/images/profileavatar/profileimage.aspx?AccountID=" + Thread.AccountID.ToString();
        //to reply one needs to login
        if (_presenter.IsUserLoggedIn())
        {
            linkReply.Text = "Reply";
            linkReply.NavigateUrl = Page.GetRouteUrl("ReplyThreadRoute", new
            {
                categoryname = CategoryName,
                forumname = ForumName,
                postid = Thread.PostID.ToString()
            });
        }
        else
        {
            linkReply.Text = "Login to Reply";
            linkReply.NavigateUrl = "/Accounts/Login.aspx";
        }

        repPosts.DataSource = Posts;
        repPosts.DataBind();
    }
}
```

在 Page_Load() 方法中，我们提取路由数据值并将其传递给呈现器，以获取主题和相关帖子。

返回的 Thread 和 Post 详细信息在 LoadData() 方法中绑定到 UI 元素。请注意，linkForumName 超链接控件的 NavigateUrl 属性设置为将我们带回论坛页面。注意，每个项目旁边还有一个回复链接。在我们的实现中，它们都做同样的事情——通过用户友好的 URL 将用户导航到 Post.aspx 页面，以创建帖子或回复。但是，只有在用户登录时，回复选项才可用。否则，用户将被重定向到登录页面。

最后，让我们快速看一下 ViewPost.aspx 页面的一个重要部分，我们从路由数据集合中直接加载类别和论坛名称。

```aspx
<asp:Hyperlink ID="linkCategoryName" Text="<%$RouteValue:categoryname %>" NavigateUrl="~/Forums/Default.aspx" runat="server" />
&nbsp; | &nbsp;
<asp:Hyperlink ID="linkForumName" Text="<%$RouteValue:forumname %>" runat="server" />
```

还要注意，linkCategoryName 超链接控件链接回论坛的默认页面。

### Post.aspx

帖子页面是论坛实际上要求您登录后才能操作的页面。一旦登录，您就可以创建新主题或回复现有的主题或帖子。

![](img/15a62e134135dde036b0ac3cbb01dab6_303_0.png)

关于此页面，您可能唯一会忽略的是它是双用途的。这意味着我们需要能够处理创建主题和帖子。区别在于，帖子的标题会按照主题的标题预先填充，并且无法编辑。

一旦所有 UI 正常工作，我们就需要连接 **保存** 按钮。这在一定程度上是通过代码后置中的 **btnSubmit_Click** 方法完成的，我们在其中捕获来自 UI 的所有数据。然后，我们尝试将数据以 BoardPost 的形式传递给呈现器，以将数据实际持久化到数据库中。

```csharp
//Fisharoo/Web/Forums/Post.aspx.cs
protected void btnSubmit_Click(object sender, EventArgs e)
{
    BoardPost post = new BoardPost();
    post.Name = txtName.Text;
    post.Post = txtPost.Text;
    _presenter.Save(post);
}
```

现在，呈现器中的 Save 方法比前面这些代码要复杂一些。

```csharp
//Fisharoo/Web/Forums/Presenters/PostPresenter.cs
public void Save(BoardPost post, string forumName, long postID)
{
    BoardForum forum = _forumService.GetForumByName(forumName);
    Boolean newPost = false;
    //is new thread
    if (postID == 0)
    {
        post.ForumID = forum.ForumID;
        post.IsThread = true;
        newPost = true;
    }
    //is reply post
    else
    {
        BoardPost postToReplyToo = _postService.GetPostByID(postID);
        if (postToReplyToo.IsThread)
            post.ThreadID = postToReplyToo.PostID;
        else
            post.ThreadID = postToReplyToo.ThreadID;

        post.ForumID = postToReplyToo.ForumID;
    }
    post.CreateDate = DateTime.Now;
    post.UpdateDate = DateTime.Now;
    post.AccountID = _webContext.CurrentUser.AccountID;
    post.Username = _webContext.CurrentUser.Username;
    post.ReplyCount = 0;
}
```

