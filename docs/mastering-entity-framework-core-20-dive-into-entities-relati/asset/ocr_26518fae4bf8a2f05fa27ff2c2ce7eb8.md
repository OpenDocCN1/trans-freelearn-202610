Let's dig in further and see what is happening internally and how Entity Framework is processing these queries. We might be thinking that EF Core would be retrieving blog entries and does a LINQ query with in-memory data. The following **SQL Server Profiler** trace will prove us wrong. If we closely watch the highlighted part of the trace, it is evident that Entity Framework is performing a LINQ to an SQL query by translating the LINQ query into the required SQL query and executing it in one shot in the database. Cool, right? The following screenshot would illustrate that the parameterised execution is translated into SQL statement:

![](bbox=[0.103, 0.282, 0.894, 0.672])

We have looked at the mechanism to compose flat SQL queries with LINQ, but we haven't covered the Include() data, which is a powerful feature of Entity Framework. You guessed it right, we could perform the Include() operation on flat SQL queries composed of LINQ queries as well. All we need to do is have the .Include() method as usual before the .ToList() call, which translates the LINQ queries into SQL queries. The following code would consume the .Include() functionality:

```
public async Task<IActionResult> LatestBlogs()
{
    //return View("Index", await _context.Blogs.FromSql("EXEC [dbo].
        [GetLatestBlogs]").ToListAsync());
}
```