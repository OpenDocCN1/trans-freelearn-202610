The following screenshot shows the URL mapped to the BlogId control, and as part of the exercise, kindly make changes to the BlogId label with the literal changes you made in the Creating controller action section:

![](bbox=[0.105, 0.19, 0.895, 0.61])

Edit a post

The same change needs to be applied to the Post action of Edit as well:

```
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Edit(int id,
[Bind("Id,BlogId,Content,PublishedDateTime,Title")] Post post)
{
...
ViewData["BlogId"] = new SelectList(_context.Blog, "Id", "Url",
post.BlogId);
return View(post);
}
```