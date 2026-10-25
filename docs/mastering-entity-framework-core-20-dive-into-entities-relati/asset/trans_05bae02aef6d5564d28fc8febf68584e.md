

这个无效用户名会在创建用户界面中抛出验证错误。我们可以看到，文本框中提供的用户名在数据库中不存在，相应的验证错误会显示在文本框旁边：

![](img/17e01f90db603e72c0f0c6a7dce8920c_177_0.png)

最后一件事，在边界情况下，我们提到参数名应该与为远程验证配置的字段名匹配。我们需要知道如果出现偏差会发生什么吗？如果参数名与配置的名称不同该怎么办？那么，值绑定将不会发生，值反而会是 null。

到目前为止，我们还没有讨论过赋值，那么就把它作为一个任务，完成如下图所示的正确验证：

![](img/17e01f90db603e72c0f0c6a7dce8920c_178_0.png)

本章我们学习了远程验证，这也结束了我们在框架内置验证之外可以执行的自定义验证。

## 总结

本章我们学习了 Entity Framework 和 MVC 提供的自定义验证。我们从客户端验证开始，它彻底改变了用户体验，然后我们研究了如果有人入侵并绕过客户端验证会发生什么。我们通过在服务端应用第二层验证解决了这些问题。我们看到了手动验证的必要性，而不是让框架开箱即用地替我们完成；我们随后执行了一些内置验证之外所需的自定义验证。我们还通过 `IClientModelValidator` 将相同的验证应用到了客户端。最后，我们执行了一个远程验证，它甚至不是 Entity Framework 的一部分，而是 MVC 框架中提供的。

到目前为止，我们只通过数据上下文访问数据，从未编写过任何纯 SQL 查询。在一些合理的场景中，我们需要绕过数据上下文以获得更好的性能，或者避免框架执行繁重的任务。我们将在第 7 章《*使用原始 SQL – 在 LINQ 中利用 SQL 查询*》中解决这个问题。

## 7 使用原始 SQL – 在 LINQ 中利用 SQL 查询

我们已经了解了自定义验证、如何利用客户端验证、当有人入侵系统并绕过客户端验证时如何响应，以及在服务端增加额外的安全层。最后，我们执行了来自 MVC 框架的远程验证。

在本章中，我们将讨论以下与数据安全相关的问题：

- 在 **Entity Framework (EF)** 中，我们有控制 SQL 查询的机制吗？
  - 我们可以直接从 EF 执行内联 SQL 查询。
- 如果有人入侵系统并执行 SQL 注入会怎样？SQL 查询真的会带来这些安全问题吗？
  - 我们可以使用参数化查询来避免 SQL 注入。
- 如果我们走纯 SQL 查询这条路，还能利用 LINQ 查询吗？
  - 是的，我们仍然可以在 LINQ 查询中利用它们。
- 我们能在没有 `DbSet` 类或 **普通旧 CLR 对象 (POCO)** 对象的情况下执行查询吗？
  - 是的，我们确实有这样的机制，可以在没有 `DbSet` 或 POCO 对象的情况下执行查询。

解决了这些问题之后，我们对本章将要介绍的内容就有了清晰的认识：

- 基本的原始 SQL 查询
- 构建参数化查询
- 与 LINQ 组合使用
- 在没有 DbSet 类或 POCO 对象的情况下执行 SQL 查询

### 基本的原始 SQL 查询

将 SQL 查询从框架中抽象出来的主要目的是，无论应用程序中使用何种数据源，都可以执行实现。我们可能会认为创建原始 SQL 查询必定违背了 EF Core 存在的初衷。有一些合理的需求可能需要原始 SQL，而不是让 Entity 替我们完成工作。

原因可以是任何东西，比如通过 LINQ 查询无法实现的功能，或者 Entity 生成的查询未能优化性能。原因可以是任何东西，但归根结底，我们都在为一个结果而工作，一个性能更优的优化结果。只要理由比 API 的使用更充分，我们甚至可能愿意采取极端措施，不与框架/API 保持一致。

我们可以使用原始模式从 EF 执行基本 SQL 查询、存储过程或函数。框架通过 DbSet 中的 FromSql 方法为我们提供了这一机制，让我们可以使用 EF 执行原始 SQL。它还允许我们基于 SQL 查询创建 LINQ 查询；因为 FromSql 是从 DbSet 类型公开的，所以这成为可能。

让我们扩展博客系统，将博客控制器的执行改为使用原始 SQL 查询，我们将从一个使用原始 SQL 查询的 Index 操作开始，如下所示：

```csharp
public async Task<IActionResult> Index()
{
    return View(await _context.Blogs.FromSql("Select * from dbo.Blog").ToListAsync());
}
```

上述实现会在视图中产生以下输出，并且与现有的渲染视图没有太大区别：

![](img/17e01f90db603e72c0f0c6a7dce8920c_181_0.png)

我们将创建一个新的操作，只是为了演示在 EF 中使用原始 SQL 查询执行存储过程（SP）GetLatestBlogs。下面列出了使用 FromSql 实现 SP 的代码：

```csharp
public async Task<IActionResult> LatestBlogs()
{
    return View("Index", await _context.Blogs.FromSql("EXEC [dbo].[GetLatestBlogs]").ToListAsync());
}
```

上述实现会在视图中产生以下输出。它所做的只是在现有的 Index 视图中渲染 SP 的结果：

![](img/17e01f90db603e72c0f0c6a7dce8920c_181_1.png)

FromSql 只有在返回的数据是所访问的 DbSet 类型时才能执行原始 SQL 查询。在前面的示例中，我们使用了 Blogs 和 DbSet，它期望查询返回 blogs，而其他实体不会被框架接受。

让我们尝试返回一个不同于为 DbSet 配置的类型，看看 EF 会有什么行为：

```csharp
public async Task<IActionResult> Index()
{
    return View(await _context.Blogs.FromSql("Select * from dbo.Post").ToListAsync());
}
```

由于返回的数据与它试图映射的类型不匹配，上述查询将触发一个无效操作：

![](img/17e01f90db603e72c0f0c6a7dce8920c_182_0.png)

从上面显示的错误来看，很明显这里发生了一些超出类型不匹配的问题，因为错误没有指明任何关于类型不匹配的信息。

![](img/17e01f90db603e72c0f0c6a7dce8920c_183_0.png)

`FromSql` 要求从数据库返回的列名与映射对象中可用的列名匹配。如果对象中存在不匹配，框架将抛出一个无效操作异常。

如果我们需要在原始 SQL 查询中使用 `FromSql` 消费匿名类型，那么在 EF Core 2.0 之前我们肯定无法实现，相关信息可以在以下共享链接 `https://github.com/aspnet/EntityFramework/issues/1862` 中看到，其中写道：

> “这是我们仍在努力在 2.0 中实现的功能，但时间安排很紧，而且我们很可能会把它推迟到 2.0 之后的下一个次要版本。” —— Diego Vega.
>
> “遗憾的是，我们没有时间在 2.0 版本中实现这一点。我们将其视为 2.0 之后的高优先级。” —— Arthur Vickers

上述回复来自 Entity Framework GitHub 仓库的所有者和成员，因此这是官方的说法：我们今年可能无法获得这个功能。

让我们尝试限制从数据库返回的列，看看是否会对应用程序产生影响。`SELECT` 语句被调整为只选择 `Blog` 实体中的部分列：

```csharp
public async Task<IActionResult> Index()
{
    return View(await _context.Blogs.FromSql("Select [Id],[Title],[Subtitle],[Description],[Url] from dbo.Blog").ToListAsync());
}
```

