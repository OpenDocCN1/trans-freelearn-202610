

在我们的控制器中，我们可以编写如下代码来查询数据库、从数据库结果填充模型并返回视图进行渲染：

```
using (var connection = new SqlConnection(connectionString))
{
    await connection.OpenAsync();
    var blogPosts = await connection.QueryAsync<BlogPost>(@"
        SELECT * FROM BlogPost");
    foreach (var post in blogPosts)
    {
        var comments = await
            connection.QueryAsync<BlogPostComment>(@"
            SELECT * FROM BlogPostComment
            WHERE BlogPostId = @BlogPostId",
            new { BlogPostId = post.BlogPostId });
        post.CommentCount = comments.Count();
    }
    return View(blogPosts);
}
```

我们测试了它并且它工作了！我们对自己感到相当满意。它在本地测试数据库上完成得很快，该数据库只包含少量行。我们在各处使用了异步方法，这一定就是它如此快速的原因。我们甚至只获取每个博客的相关评论，而不是每次都获取所有评论。我们还使用了参数化查询来避免 SQL 注入，一切看起来都很不错。发布吧！

由于这是一个示例，我们为了清晰起见保持了简单。在实际应用程序中，你需要使用诸如依赖注入（例如 ASP.NET Core 内置的 DI）等技术使其更具灵活性。

不幸的是，当数据开始增加（随着帖子和评论的添加）时，博客开始变得非常缓慢，页面加载时间变长。我们的读者等待不耐烦并放弃。受众数量随之下降，收入也随之下降。

让我们分析数据库以查看问题可能出在哪里。我们运行 SQL Server Profiler，过滤目标数据库，并查看正在执行的 SQL。

## 高效分页

在实际应用程序中，你需要实现分页，以便当表中数据很多时列表不会过长。在单个页面上列出数千个项目是不好的主意。

你可能想用 LINQ 命令来实现这一点，因为它们非常方便。然而，你需要小心。如果你的 O/RM 不感知 LINQ，或者你过早地错误地转换了类型，那么过滤可能会在应用程序内部发生，而最佳的过滤位置实际上是在数据库中。你的代码可能在你没有意识到的情况下检索所有数据并丢弃大部分数据。

也许你想修改 action 方法的返回语句，使其看起来像以下这样：

```
return View(blogPosts.OrderByDescending(bp => bp.CommentCount)
    .Skip(pageSize * (pageNumber - 1))
    .Take(pageSize));
```

这可行，并且会显著加快你的应用程序速度。然而，它可能没有你想象的效果。应用程序更快是因为视图渲染更快，因为生成的页面更小。这也减少了将页面发送到浏览器以及浏览器渲染 HTML 的时间。

然而，应用程序仍然从数据库获取所有博客帖子并将其加载到内存中。随着数据量的增长，这可能会成为一个问题。如果你想使用此类 LINQ 方法，则需要检查它们是否一直处理到数据库。阅读你的 O/RM 或框架的文档并使用分析器仔细检查生成的 SQL 是一个非常好的主意。

让我们看看 SQL 应该是什么样子的。例如，如果你使用 SQL Server，从前面的查询开始，你可以通过以下方式仅获取评论最多的前十篇帖子：

```
SELECT TOP 10
    bp.BlogPostId,
    bp.Title,
    COUNT(bpc.BlogPostCommentId) 'CommentCount'
FROM BlogPost bp
LEFT JOIN BlogPostComment bpc
    ON bpc.BlogPostId = bp.BlogPostId
GROUP BY bp.BlogPostId, bp.Title
ORDER BY COUNT(bpc.BlogPostCommentId) DESC
```

我们按评论计数降序排序。但是，如果你愿意，也可以按降序 ID 排序以获得大致的逆时间顺序。从这个有序集合中，我们仅选择（或获取）前十条记录。

如果你想跳过记录以进行分页，SELECT TOP 子句是不够的。在 SQL Server 2012 及更高版本中，你可以使用以下方法：

```
SELECT
    bp.BlogPostId,
    bp.Title,
    COUNT(bpc.BlogPostCommentId) 'CommentCount'
FROM BlogPost bp
LEFT JOIN BlogPostComment bpc
    ON bpc.BlogPostId = bp.BlogPostId
GROUP BY bp.BlogPostId, bp.Title
ORDER BY COUNT(bpc.BlogPostCommentId) DESC
OFFSET 0 ROWS
FETCH NEXT 10 ROWS ONLY
```

你可以调整 OFFSET 的值以获取页面编号的正确条目。FETCH NEXT 的值将更改页面大小（页面上的条目数）。你可以使用参数化查询传入这些值，如下所示：

```
using (var connection = new SqlConnection(connectionString))
{
    await connection.OpenAsync();
    var blogPosts = await connection.QueryAsync<BlogPost>(@"
        SELECT
            bp.BlogPostId,
            bp.Title,
            COUNT(bpc.BlogPostCommentId) 'CommentCount'
        FROM BlogPost bp
        LEFT JOIN BlogPostComment bpc
            ON bpc.BlogPostId = bp.BlogPostId
        GROUP BY bp.BlogPostId, bp.Title
        ORDER BY COUNT(bpc.BlogPostCommentId) DESC
        OFFSET @OffsetRows ROWS
        FETCH NEXT @LimitRows ROWS ONLY", new
        {
            OffsetRows = pageSize * (pageNumber - 1),
            LimitRows  = pageSize
        }
    );
    return View(blogPosts);
}
```

截图中的查询获取了页面大小设置为 50 条目时的第三页数据。因此，它使用了 100 的偏移量（跳过前两页各 50 条）并获取接下来的 50 行。对应的 URL 查询字符串可能如下所示：

```
/?pagenumber=3&pagesize=50
```

我们可以看到查询的持续时间已从之前的 24 ms 减少到现在的 14 ms。

![](img/5e416ac29f1bccaad147c6829853fd77_125_0.png)

注意当参数传递到查询中时 SQL 的执行方式不同。这比直接将用户提供的值连接到 SQL 命令中要安全得多（因此，永远不要这样做拼接）。如果你使用用户输入构建 SQL 查询，那么你的应用程序就会暴露于 SQL 注入攻击之下。你的整个数据库可能通过基本的点按工具通过网络被下载。你的数据库也可能被篡改或删除并被勒索，但攻击者实际上并没有他们声称的备份。数据库被窃的另一种方式是如果你粗心地将未加密的备份放在可通过网络访问的位置。

如果你不使用任何参数，则使用默认值，首页仅显示十个条目，看起来可能如下所示的截图，具体取决于数据库中的数据：

![](img/5e416ac29f1bccaad147c6829853fd77_125_1.png)

对于与数据库过于频繁通信的写得不好的应用程序，从性能角度来看，将应用程序部署在靠近数据库（或与数据库在同一服务器上）的服务器上会更好。也许应用程序工作区和数据库服务器可以在同一个**服务器机架**中，位于**数据中心**，并通过千兆（或 10 千兆）**以太网**连接。

用户然后可以使用远程桌面连接或**Citrix**会话与应用程序交互。这将减少到数据库的延迟，即使考虑到远程 UI 的延迟，也能加快速度。这有效地将桌面 PC 变成**瘦客户端**，类似于旧式大型机的工作方式。

例如，你可以构建一台具有 RAID SSD 和大量 RAM 的高性能服务器，其成本远低于修复大型应用程序的开发人员时间。即使是写得不好的软件，如果你将应用程序和数据库一起运行在同一台机器上，尤其是如果在**裸金属**上运行且没有虚拟机阻碍，也能表现良好。这种策略可以为你赢得时间，以便正确修复问题。

这些远程应用程序和虚拟化技术通常被作为帮助部署和维护的工具出售。然而，其潜在的性能优势也值得考虑。

由于 Web 应用程序的兴起，**厚客户端**桌面应用程序现在变得不那么常见。架构似乎在服务器端计算和客户端工作之间振荡，因为网络速度和处理能力的相对进步相互竞争。

## Web 应用程序

同样的迁移方法对于 Web 应用程序通常并不那么有效，但这取决于使用的架构。好消息是，对于 Web 应用程序，你通常控制基础设施。对于原生应用程序硬件则通常不是这种情况。

如果你使用**三层架构**，那么你可以将应用程序服务器移近数据库服务器。这是否有效取决于 Web 服务器与应用程序服务器的通信是否过于频繁。如果它们发出太多 Web API 请求，那么这种方法效果不佳。

**两层架构**（Web 服务器直接与数据库通信）在典型的 Web 应用程序中更常见。有使用**集群**数据库或只读镜像的解决方案来将数据放置在靠近 Web 服务器的位置，但这会增加复杂性和成本。

**代理服务器**可以带来显著差异。流行的开源代理服务器是 Varnish，你也可以使用 NGINX Web 服务器作为代理。代理服务器缓存 Web 服务器的输出，因此页面不必为每个用户重新生成。

