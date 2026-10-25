

The issue (lazy loading support) created in the EF Core repository is also listed as follows:
https://github.com/aspnet/EntityFrameworkCore/issues/3797

尽管当前框架尚不支持该功能，但理想情况下应分析该问题并研究延迟加载的解决方案。为讨论起见，如果 EF Core 在未来支持延迟加载，以下实现将产生 N + 1 Select 问题，每当访问 Comments 导航属性时都会执行单独的数据存储调用。

以下高亮代码将触发单独的数据存储调用以填充 NoOfComments 属性：

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

在 EF Core 2.0 中，前述代码将返回导航属性 Comments 为 NULL，假设我们未访问该属性，因为它未被显式加载。以下截图将突出显示评论未被显式加载：

![](img/17e01f90db603e72c0f0c6a7dce8920c_322_0.png)

它会奇怪地执行额外的数据库调用以从数据存储中检索计数，即使导航属性返回为 NULL：

![](img/17e01f90db603e72c0f0c6a7dce8920c_323_0.png)

当我们在 LINQ 查询中包含 Comments 导航属性时，它将执行 Comments 实体的显式加载以满足其请求。以下代码将使 EF Core 执行评论的显式加载：

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

我们可以在以下截图中看到为 Comments 实体显式加载所进行的执行：

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

显式加载现在将为对博客文章发表评论的人员返回适当的评论。现在我们可以执行我们的操作，在列表页面中列出用户发表的评论数量，如以下高亮图像所示，评论可用：

![](img/17e01f90db603e72c0f0c6a7dce8920c_325_0.png)

评论数量现在按预期在 People 列表页面中列出，如以下截图所示：

![](img/17e01f90db603e72c0f0c6a7dce8920c_325_1.png)

我们讨论了 N + 1 Select 问题，并看到了当前 EF Core 框架如何处理该场景以及我们可以提供的解决方案。在下一节中，我们将探讨如何将从服务或控制器返回的数据限制为仅视图所消耗的字段。

## More data than required

我们正在创建的 About 组件消耗 Person 实体，其中包含大量对组件不必要的信息，但仍然会被处理并返回到 View。我们可以在以下代码中看到，AboutComponent 消耗 GetPersonByIdQuery 并将整个 Person 实体返回给 View：

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

前述 View 组件仅渲染用户实体的 Name 和 Biography 属性。我们可以在以下视图组件代码中看到它们：

```
<h2>About @Model.Name</h2>
<p>
@Model.Biography
</p>
```

View 组件然后消耗数据仅渲染 Person 实体的 FirstName 和 Biography。很明显我们需要解决这个问题，因为我们正在向视图返回不必要的数据。

我们可以通过以下截图可视化渲染的视图，并看到它未消耗用户实体的任何其他属性：

![](img/17e01f90db603e72c0f0c6a7dce8920c_327_0.png)

如果我们尝试分析数据存储调用，我们可以看到特定调用中包含大量列，而它只消耗两列数据：

![](img/17e01f90db603e72c0f0c6a7dce8920c_328_0.png)

我们现在可以使用投影和 View 模型来缩小返回的数据范围，仅限制为 Name 和 Biography：

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

现在，分析器突出显示的数据库调用仅包含用于渲染数据库所需的两列，从而减少了从数据库检索的内容，如以下截图所示：

![](img/17e01f90db603e72c0f0c6a7dce8920c_329_0.png)

这解决了从数据存储返回过多数据的问题。在下一节中，让我们来看一下不匹配的数据类型。

## Mismatched data types

不匹配的数据类型会损害性能。这乍一看似乎很明显，但如果我们仔细观察，很明显我们应该解决这个问题。以下调整的 About 视图组件说明了使用 `PhoneNumber` 而不是 `Id` 检索数据：

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

前述 LINQ 查询将被翻译成以下 SQL 查询；启用查询执行的 **Actual Execution** 计划，就会很明显这里出了问题。每当查询中存在不匹配的数据类型时，数据类型转换将强制执行索引扫描操作。在以下截图中，我们可以看到正在发生 **Clustered Index Scan** 而不是执行查找操作，这进一步影响了性能：

![](img/17e01f90db603e72c0f0c6a7dce8920c_330_0.png)

如果我们悬停在执行计划上，我们可以看到更多信息，例如记录数、操作成本、执行次数、受影响的行数等，可以在以下截图中可视化：

![](img/17e01f90db603e72c0f0c6a7dce8920c_331_0.png)

如果我们将查询更改为基于 Id，我们可以看到它神奇地使用查找而不是扫描操作。以下截图证明按预期执行了查找操作：

![](img/17e01f90db603e72c0f0c6a7dce8920c_332_0.png)

以下截图证实了我们的理论，即查找操作比扫描操作更快，因为查找中读取的行数为 1，而扫描中为 2。以下截图执行查找操作证实了关于行读取的讨论：

![](img/17e01f90db603e72c0f0c6a7dce8920c_333_0.png)

因此，修复模型的数据类型将避免这种数据类型转换并执行有效的数据过滤。在下一节中，让我们分析缺失的索引。

