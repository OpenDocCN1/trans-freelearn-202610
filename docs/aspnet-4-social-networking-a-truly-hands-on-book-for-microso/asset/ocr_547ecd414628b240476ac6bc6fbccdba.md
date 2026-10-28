# # Latest blog posts

Viewing the latest blog posts is a single call to the BlogService. In order for our UI to get to any service, it first has to hand off its control of all display interactions. It does this by initializing an instance of the DefaultPresenter and then passing a reference to itself into the Init() method of the presenter. Once in the Init() method of the presenter we can then make the call into the service to get the latest blogs. The Blog entity is translated to PEBlog, which is a presentation entity and has only those fields that are relevant for the UI.

```
```csharp
//Fisharoo/Web/Blogs/Presenters/DefaultPresenter.cs
public class DefaultPresenter
{
    private IDefault _view;
    [Import]
    private IBlogService _blogService;
    public DefaultPresenter()
    {
        MEFManager.Compose(this);
    }

    public void Init(IDefault View)
    {
        _view = View;
        List<Blog> list = _blogService.GetLatestBlogs();
        _view.LoadBlogs(Translator.BlogToPEBlog(list));
    }
}
```
```

With the latest blogs in hand we are then able to pass them into the UI's code behind where the blogs are bound in the Default.aspx page. The only method of interest here is ItemDataBound() method that handles each item that is bound to a list view control.

```
```csharp
//Fisharoo/Web/Blogs/Default.aspx.cs
public void lvBlogs_ItemDataBound(object sender, ListViewItemEventArgs e)
{
    Literal litBlogID = e.Item.FindControl("litBlogID") as Literal;
    HyperLink linkTitle = e.Item.FindControl("linkTitle") as HyperLink;
    Literal litPageName = e.Item.FindControl("litPageName") as Literal;
    Literal litUsername = e.Item.FindControl("litUsername") as Literal;
    //navigation URL will follow the URL routing pattern
    linkTitle.NavigateUrl = Page.GetRouteUrl("BlogRoute", new { username = litUsername.Text, blogpage = litPageName.Text });
}
```
```