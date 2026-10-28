Finally, let's quickly look at one important part of the ViewPost.aspx page where we load the category and forum names directly from the routed data collection.

```
```aspx
<asp:Hyperlink ID="linkCategoryName" Text="<%$RouteValue:categoryname %>" NavigateUrl="~/Forums/Default.aspx" runat="server" />
&nbsp; | &nbsp;
<asp:Hyperlink ID="linkForumName" Text="<%$RouteValue:forumname %>" runat="server" />
```
```

Also note that linkCategoryName hyperlink control links back to the forum's default page.

# Post.aspx

The post page is the one page that the forums actually require you to be logged into so that you can interact with it. Once logged in you are able to create a new thread or reply to an existing thread or post.

![](bbox=[0.178, 0.411, 0.823, 0.7])

The only thing you might miss about this page is that it is dual purpose. This means that we need to be able to handle creating threads and posts. The difference here is that a post has its title prepopulated as per the thread's title, and you cannot edit it.