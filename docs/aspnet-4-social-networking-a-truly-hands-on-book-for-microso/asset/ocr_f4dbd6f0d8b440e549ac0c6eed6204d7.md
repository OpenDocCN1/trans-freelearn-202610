# Blogs

In the first section of the **Init** method we are checking the **WebContext.BlogID** property to see if we have something to work with. If we do then we load the UI with the appropriate blog. If not, it means it is a new blog entry we are creating. The next item you see is the SavePost method that takes care of passing a loaded/newly created blog into the BlogService to be saved.

Next, we will take a look at the code behind that the presenter works with. Here we will see the LoadPost and btnSave_Click methods. There is nothing fancy to follow here. But the one aspect to pay attention to is that we keep track of the BlogID in the page so that we know what we are working with later.

```
protected void btnSave_Click(object sender, EventArgs e)
{
    Blog blog = new Blog();
    if (litBlogID.Text != "")
        blog.BlogID = Convert.ToInt64(litBlogID.Text);
    blog.IsPublished = chkIsPublished.Checked;
    blog.PageName = txtPageName.Text;
    blog.Post = txtPost.Text;
    blog.Subject = txtSubject.Text;
    blog.Title = txtTitle.Text;
    _presenter.SavePost(blog);
}
public void LoadPost(Blog blog)
{
    txtTitle.Text = blog.Title;
    txtSubject.Text = blog.Subject;
    txtPost.Text = blog.Post;
    txtPageName.Text = blog.PageName;
    chkIsPublished.Checked = blog.IsPublished;
    litBlogID.Text = blog.BlogID.ToString();
}
```

This takes care of the details of data inputs and outputs. However, let's now take a look at what we need to do make the UI usable. You may recall that we used the Xinha WYSIWYG editor before. We will use it here too. All that is required to hook this up is a multiline text box control and a single line of JavaScript.

```
---
<asp:TextBox TextMode="MultiLine" ID="txtPost"
             runat="server"></asp:TextBox>
---
<script type="text/javascript">
    xinha_editors[xinha_editors.length] = 'Content_txtPost';
</script>
---
```

With this in place we should be ready to create a blog post!