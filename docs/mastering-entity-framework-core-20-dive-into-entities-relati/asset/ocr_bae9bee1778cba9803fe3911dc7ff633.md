![](bbox=[0.135, 0.221, 0.206, 0.282])

The million records we had inserted in the `Person` table would affect the preceding illustrated `Index()` action, which would return a list of people, it would throw a timeout error instead of rendering the list. We could avoid this issue by handling pagination implemented in Chapter 8, *Query Is All We Need – Query Object Pattern*, which is available in the `GetPaginatedPostByKeywordQuery.cs` of the following Git repository path: https://goo.gl/SwvVcC.

The same approach of `Skip()` and `Take()` could be used here to avoid timeout issue, a quick fix would be limiting to 100 records as shown, which should be a paginated query in the future.

```
var people = await _context.People.Take(100)
    .AsNoTracking()
    .ToListAsync();
```

If the tracking is not required for the entire controller, we could disable it on a context level using `QueryTrackingBehavior` of the `ChangeTracker` type. Since the blogging system initializes the context on each request, we could disable tracking on the constructor using `QueryTrackingBehavior` as shown in the following code:

```
public PeopleController(BlogContext context)
{
    _context = context;
    _context.ChangeTracker.QueryTrackingBehavior =
        QueryTrackingBehavior.NoTracking;
}
```

We have seen how the tracking could be disabled at the query level or context level using two approaches. Kindly use them wisely to avoid running into updating/deletion issues, as those operations require tracking.