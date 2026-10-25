
The issue (lazy loading support) created in the EF Core repository is also listed as follows:
https://github.com/aspnet/EntityFrameworkCore/issues/3797

Even though the framework currently does not support the feature, it would be ideal to analyze the issue and study the solutions for lazy loading. For discussion's sake, if EF Core supports lazy loading in the future, the following implementation will create the N + 1 Select issue, which will perform individual data store calls whenever the Comments navigational property is accessed.

The following highlighted code would trigger individual data store calls for populating the NoOfComments property:

```
var people = await _context.People
    .AsNoTracking()
    .ToListAsync();
var peopleViewModel = new List<PersonViewModel>();
people.ForEach(item =>
{
    peopleViewModel.Add(new PersonViewModel
    {
        Person = item,
        NoOfComments = item.Comments != null ?
            item.Comments.Count : 0
    });
});
```

In EF Core 2.0, the preceding code will return the navigational Comments property as NULL, assuming we were not accessing the property, since it is not eager-loaded. The following screenshot would highlight that the comment was not eager loaded:

![](img/17e01f90db603e72c0f0c6a7dce8920c_322_0.png)

It would strangely perform an additional database call to retrieve the count from the data store, even though the navigational property was returned as NULL:

![](img/17e01f90db603e72c0f0c6a7dce8920c_323_0.png)

When we include the Comments navigational property in the LINQ query, it would perform eager loading of the Comments entity to serve its request. The following code would let the EF Core to perform eager loading the comments:

```
var people = await _context.People
.Include(item => item.Comments)
.AsNoTracking()
.ToListAsync();
var peopleViewModel = new List<PersonViewModel>();
people.ForEach(item =>
{
    var comment = item.Comments;
    peopleViewModel.Add(new PersonViewModel
    {
        Person = item,
        NoOfComments = item.Comments != null ?
        item.Comments.Count : 0
    });
});
```

We can see the execution made for the eager loading of the Comments entity in the following screenshot:

| EventClass | TextData |
|---|---|
| Audit Logout |  |
| RPC:Completed | exec sp_reset_connection |
| Audit Login | -- network protocol: Named Pipes s... |
| SQL:BatchStarting | SELECT [item].[Id], [item].[Biograp... |
| SQL:BatchCompleted | SELECT [item].[Id], [item].[Biograp... |
| SQL:BatchStarting | SELECT [c1].[Id], [c1].[CommentedAt... |
| SQL:BatchCompleted | SELECT [c1].[Id], [c1].[CommentedAt... |
| RPC:Completed | exec sp_executesql N'SELECT COUNT(*... |
| RPC:Completed | exec sp_executesql N'SELECT COUNT(*... |

SELECT [c1].[Id], [c1].[CommentedAt], [c1].[Content], [c1].[CreatedAt],
FROM [Comment] AS [c1]
WHERE EXISTS (
    SELECT 1
    FROM [Person] AS [item]
    WHERE [c1].[PersonId] = [item].[Id])
ORDER BY [c1].[PersonId]

The eager loading will now return appropriate comments for the person who has commented on the blog posts. Now we can perform our operation of listing the number of comments the user has posted in the listing page as in the following highlighted image the comments were available:

![](img/17e01f90db603e72c0f0c6a7dce8920c_325_0.png)

The number of comments is now listed as expected in the People list page, shown as in the following screenshot:

![](img/17e01f90db603e72c0f0c6a7dce8920c_325_1.png)

We have discussed the N + 1 Select problem, and then saw how the current EF Core Framework handles the scenario and the solution we could provide to resolve the issue. In the next section, we will explore how the data returned from the service or controller could be limited to only the fields that were consumed by the views.

## More data than required

The About component we were creating consumes the Person entity, which contains a lot of information that is not necessary for the component, but still it is processed and returned to the View. We can see in the following code, the AboutComponent consumes the GetPersonByIdQuery and returns the entire Person entity to the View:

```
public IViewComponentResult Invoke(int id)
{
    var user = _repository.GetSingle(
        new GetPersonByIdQuery(_context)
        {
            IncludeData = true,
            Id = id
        });
    return View(user);
}
```

The preceding View component renders only the Name and Biography properties of the user entity. We can see them in the following view component code:

```
<h2>About @Model.Name</h2>
<p>
@Model.Biography
</p>
```

The View component then consumes the data to render only the FirstName and Biography of the Person entity. It is obvious that we need to address this problem, as we are returning unnecessary data to the view.

We can visualize the rendered view from the following screenshot and see that it does not consume any other property from the user entity:

![](img/17e01f90db603e72c0f0c6a7dce8920c_327_0.png)

If we try to profile the data store call, we can see that there are a lot of columns included in the particular call, which consumes only two-column data:

![](img/17e01f90db603e72c0f0c6a7dce8920c_328_0.png)

We can now narrow down the data returned using projection and a View model, which restricts the data only to Name and Biography:

```
public IViewComponentResult Invoke(int id)
{
    var aboutViewModel = _context.People
        .Where(item => item.Id.Equals(id))
        .Select(item => new AboutViewModel
        {
```

```
Name = item.FirstName,
Biography = item.Biography
}).SingleOrDefault();

return View(aboutViewModel);
}
```

Now, the profiler highlights the database call that contains only two columns required for the rendering of the database, thereby reducing the content that was retrieved from the database, as shown in the following screenshot:

![](img/17e01f90db603e72c0f0c6a7dce8920c_329_0.png)

This addresses our problem of returning too much data from the data store. In the next section, let's look into mismatched data types.

## Mismatched data types

The mismatched data type would hurt the performance. This might look obvious at first, but if we take a closer look, it is evident that we should be addressing the concern. The following tweaked About view component illustrates the retrieval of data using `PhoneNumber` rather than `Id`:

```
var aboutViewModel = _context.People
    .Where(item => item.PhoneNumber.Equals("9876543210"))
    //.Where(item => item.Id.Equals(id))
    .Select(item => new AboutViewModel
    {
        Name = item.FirstName,
        Biography = item.Biography
    }).SingleOrDefault();
```

The preceding LINQ query would be translated into the following SQL query; enable **Actual Execution** plan with the query execution, and it would be evident that something is wrong here. Whenever we have mismatched data types in a query, the data type conversion would enforce an index scan operation. In the following screenshot, we can see that the **Clustered Index Scan** is happening rather than performing a seek operation, which further affects the performance:

![](img/17e01f90db603e72c0f0c6a7dce8920c_330_0.png)

If we hover on the execution plan, we can see more information, such as the number of records, the cost of the operation, the number of execution, rows affected, and so on, which can be visualized in the following screenshot:

![](img/17e01f90db603e72c0f0c6a7dce8920c_331_0.png)

If we change the query to be based on Id, we can see that it magically uses seek instead of a scan operation. The following screenshot proves that the seeking operation is performed as expected:

![](img/17e01f90db603e72c0f0c6a7dce8920c_332_0.png)

The following screenshot confirms our theory that the seek operation is faster than the scan operation, as the number of rows read was 1 in seek and 2 in the scan operation. The following screenshot performs seek operation confirms the discussion about row reads:

![](img/17e01f90db603e72c0f0c6a7dce8920c_333_0.png)

So, fixing the data type of the model would avoid this data type conversion and would perform effective filtering of data. In the next section, let's analyze missing indexes.
