Still not convinced? Me neither. Unless we handle injection for a string column, I won't be convinced. Let's tweak the system just for the purpose of illustration by adding the following action, which would filter blogs using a wildcard search in the Title column, returning the first item from the result set and finally rendering them in an existing Details view:

```
public async Task<IActionResult> GetBlogByTitle(string keyword)
{
    if (keyword == null)
    {
        return NotFound();
    }

    var blog = await _context.Blogs.FromSql("Select * from dbo.Blog WHERE Title like '%' + @keyword + '%',
    new SqlParameter("keyword", keyword)).FirstOrDefaultAsync();
    if (blog == null)
    {
        return NotFound();
    }

    return View("Details", blog);
}
```

Try to inject the values as we did earlier, by visiting the following paths:

http://localhost:54346/Blogs/GetBlogByTitle/keyword=core OR 1=1
http://localhost:54346/Blogs/GetBlogByTitle?keyword=core OR 1=1

Still, the system would be handling the injection, but now at the database end as it would be treated as a string value. The query fails to return any records, thereby returning NotFound() from the action:

![](bbox=[0.1, 0.68, 0.9, 0.87])