# Composing with LINQ

We have performed flat SQL queries so far. This doesn't involve LINQ queries. If we had a mechanism that could leverage flat queries composed of LINQ to SQL queries, then we could get the best of both worlds. Fortunately, we do have built-in support in Entity Framework, and the flat SQL queries could be composed of LINQ queries, and we will cover them in detail in this section.

Let's comment the current implementation in LatestBlogs() and perform the SP implementation using flat queries composed with LINQ. Ideally, what we are trying to achieve is to get the table data using a LINQ query and compose the result with a LINQ query to perform filtering and ordering from a LINQ query (this might not be a real-world scenario, but it works well for illustration):

```
public async Task<IActionResult> LatestBlogs()
{
    //return View("Index", await _context.Blogs.FromSql("EXEC [dbo].
    [GetLatestBlogs]").ToListAsync());
    var comparisonDateTime = DateTime.Now.AddMonths(-3);
    return View("Index", await _context.Blogs
        .FromSql("Select * from dbo.Blog")
        .Where(x=>x.CreatedAt >= comparisonDateTime)
        .OrderByDescending(x=>x.Id)
        .ToListAsync());
}
```

We can see the outcome as expected, and it reflects that of the earlier result set for the latest blogs in the following screenshot:

![](bbox=[0.104, 0.623, 0.896, 0.838])