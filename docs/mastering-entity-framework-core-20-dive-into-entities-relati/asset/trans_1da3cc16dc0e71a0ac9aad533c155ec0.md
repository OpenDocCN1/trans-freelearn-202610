
```csharp
.ExecuteReaderAsync(
    databaseFacade.GetService<IRelationalConnection>(),
    parameterValues: rawSqlCommand.ParameterValues,
    cancellationToken: cancellationToken);
}
}
```

下面的代码用法非常简单；它所做的只是将 ExecuteSqlQueryAsync 扩展方法所需的 SQL 查询传递进去，并以 DbDataReader 的形式返回结果集：

```csharp
var results = await _context.Database
    .ExecuteSqlQueryAsync(@"select b.Title as BlogTitle, p.* from
        Post p join Blog b on b.Id = p.BlogId");
while (results.DbDataReader.Read())
{
    Console.Write(
        $"Blog Title: '{results.DbDataReader["BlogTitle"]}', " +
        $"Post Title: '{results.DbDataReader["Title"]}'");
}
```

我们遍历了 DbDataReader，它会将帖子及其博客标题打印出来。下面的截图证明，使用我们自己的扩展方法，从 Entity Framework 的扁平 SQL 查询中返回了匿名类型：

![](img/17e01f90db603e72c0f0c6a7dce8920c_202_0.png)

作为额外检查，让我们验证结果集中返回的第二项，以及它与 Post 表中可用的 .NET Core 条目是否匹配。下面的截图确认了帖子条目仅来自 .NET Core 博客：

## 8

## 查询即所需 – 查询对象模式

我们已经介绍了 Entity Framework（EF）中的原生 SQL 查询执行，在少数有效场景下，这可能是为了控制查询或提高性能所必需的。我们看到了一个简单的 SQL 查询、它可能带来的安全威胁，以及相应的解决方案。然后，我们介绍了在 Entity Framework 中执行原生 SQL 查询的不同方式。最后，我们创建了一个扩展方法，用于在没有 `DbSet` 和 POCO 模型的情况下执行原生 SQL 查询。

让我们讨论一下当前在对象关系映射（ORM）层中使用的仓储模式，审视其设计问题，我们将看到查询对象模式如何帮助我们高效地实现这一点。本章将讨论以下设计问题：

- 仓储模式最初是为了抽象 ORM 层，但最近仓储中包含的方法急剧增加，这违背了它的初衷：
  - 简化数据访问层的必要性催生了查询对象模式
- 我们一直在仓储中创建多个方法，这些方法仅因查询性质不同而有所区别：
  - 查询对象模式概括并改进了仓储方法
- 仅靠查询不足以简化数据访问层：
  - 将查询与通用命令组合，可将该模式扩展到所有 CRUD 操作
- 查询对象模式仍然不允许用户动态扩展查询：
  - 简单的查询对象在查询内部执行实现，而表达式树将实现委托给用户，从而为用户提供了所需的可扩展性

理想情况下，我们将涵盖以下主题：

- 查询对象简介
- 使用查询对象模式改进仓储
- 将查询与命令组合
- 使用表达式树增强查询

### 查询对象简介

查询对象模式可以通过引入查询类型并通过该类型处理查询来整合，而不是直接处理查询。请按照以下步骤将该模式整合到任何应用程序中：

1. 创建一个查询类型，其中包含查询所需的字段，如下所示：
   - 拥有一个参数化构造函数，强制只有在提供了必填字段值时才创建类型实例
   - 查询所需的必填字段必须包含在构造函数中

让我们创建一个按 Id 筛选帖子的查询类型；我们需要一个具有 Id 字段和填充 Id 字段的构造函数的查询类型：

```csharp
public class PostDetailQuery
{
    public PostDetailQuery(int? id)
    {
        this.Id = id;
    }
    public int? Id { get; set; }
}
```

查询类型已准备好实现；让我们继续下一步，创建处理器。

2. 创建一个查询处理器接口，强制该类型处理查询；在接口中创建 `Handle()` 方法，强制类型实现它。

处理器必须分两步创建：首先，应创建一个接口，其中包含处理执行所需的约定：

```csharp
public interface IPostDetailQueryHandler
{
    Task<Post> Handle(PostDetailQuery query);
}
```

约定已就绪；让我们使用具体的查询处理器类型来创建实现。

3. 创建一个使用数据上下文执行查询的查询处理器类型，通过构造函数注入数据库上下文

`BlogContext` 数据库上下文通过构造函数注入到处理器中，如下所示：

```csharp
public class PostDetailQueryHandler : IPostDetailQueryHandler
{
    private readonly BlogContext _context;
    public PostDetailQueryHandler(BlogContext context)
    {
        _context = context;
    }
}
```

数据上下文现在可用；让我们使用查询对象的上下文来执行查询：

- 使用数据上下文通过实际查询实现 `Handle()`

查询处理器实现使用了之前创建的查询类型，数据上下文利用查询对象中可用的数据创建筛选器并执行查询：

```csharp
public class PostDetailQueryHandler : IPostDetailQueryHandler
{
    // Code removed for brevity
    public async Task<Post> Handle(PostDetailQuery query)
    {
        return await _context.Posts
            .Include(p => p.Author)
            .Include(p => p.Blog)
            .Include(p => p.Category)
            .FirstOrDefaultAsync(x => x.Id == query.Id);
    }
}
```

现在查询处理器实现已完成，但应用程序中尚未在任何地方使用它。让我们看看如何在控制器中使用它。

4. 在控制器中使用查询处理器，通过构造函数注入查询处理器

PostDetailQueryHandler 类型通过构造函数，使用 .NET Core 支持的依赖注入（正如我们之前对 BlogContext 所做的那样），针对 IPostDetailQueryHandler 接口进行注入：

```csharp
public class PostsController : Controller
{
    private readonly BlogContext _context;
    private readonly IPostDetailQueryHandler
        _postDetailQueryHandler;

    public PostsController(BlogContext context,
        IPostDetailQueryHandler postDetailQueryHandler)
    {
        _context = context;
        _postDetailQueryHandler = postDetailQueryHandler;
    }
    // Code removed for brevity
}
```

查询处理器现在已在构造函数中可用；让我们使用它来执行所需操作。找到（控制器中）需要该查询的操作，并使用查询处理器来执行。

控制器中的 Details 操作需要基于帖子 Id 执行查询，因此让我们使用为此需求创建的 PostDetailQuery 来使用该查询。以下代码将在控制器中使用查询处理器：

```csharp
public async Task<IActionResult> Details(int? id)
{
    // Code removed for brevity
    var post = await _postDetailQueryHandler.Handle(new
        PostDetailQuery(id));
    // Code removed for brevity
}
```

前面的实现将根据传递给查询对象的 id 触发帖子筛选，从而将核心实现封装到查询类型中，满足查询对象模式：

- 最后，不要忘记在 `Startup.cs` 的 `ConfigureServices` 方法中支持依赖注入中的构造函数执行：

```csharp
services.AddScoped<IPostDetailQueryHandler, PostDetailQueryHandler>();
```

我们已经了解了如何创建查询对象和处理器，以及如何在控制器中使用它们。在*第 9 章，故障安全机制 – 事务*中，我们将了解如何通过使用查询对象来利用或改进仓储。

### 使用查询对象模式改进仓储

我们需要一个前提来解释查询对象模式的必要性，所以让我们创建这样一个需求，然后我们将研究查询对象。我们将在系统中为博客创建一个仓储，我们将看到它为应用程序带来的优点和缺点，最后，我们将了解什么是查询对象，以及它们如何帮助我们改进仓储。

### 仓储简介

我们一直在控制器中直接使用数据上下文执行 CRUD 操作；我们可以先在应用程序中引入仓储，之后，可以在仓储中实现查询对象。

让我们从创建仓储所需的接口开始。它包括基本检索、业务特定检索以及其余的 CRUD 操作。以下代码包含 `IPostRepository` 所需的方法：

```csharp
public interface IPostRepository
{
    IEnumerable<Post> GetAllPosts();
}
```
