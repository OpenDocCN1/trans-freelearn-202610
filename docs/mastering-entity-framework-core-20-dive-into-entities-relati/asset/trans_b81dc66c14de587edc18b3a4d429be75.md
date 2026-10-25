

让我们进一步深入探究内部发生了什么，以及 Entity Framework 是如何处理这些查询的。我们可能会认为 EF Core 会检索博客条目并使用内存中的数据执行 LINQ 查询。下面的 **SQL Server Profiler** 跟踪将证明我们是错误的。如果我们仔细观察跟踪中突出显示的部分，就很明显 Entity Framework 是通过将 LINQ 查询翻译成所需的 SQL 查询并一次性在数据库中执行来实现 LINQ 到 SQL 查询的。很酷，对吧？下面的截图将说明参数化执行被翻译成 SQL 语句的情况：

![](img/17e01f90db603e72c0f0c6a7dce8920c_192_0.png)

我们已经研究了如何使用 LINQ 组合扁平 SQL 查询的机制，但我们还没有介绍 `Include()` 数据，而这正是 Entity Framework 的一项强大功能。你猜对了，我们也可以对由 LINQ 查询组成的扁平 SQL 查询执行 `Include()` 操作。我们需要做的就是像往常一样在 `.ToList()` 调用之前加上 `.Include()` 方法，这会将 LINQ 查询翻译成 SQL 查询。下面的代码将使用 `.Include()` 功能：

```
public async Task<IActionResult> LatestBlogs()
{
    //return View("Index", await _context.Blogs.FromSql("EXEC [dbo].
        [GetLatestBlogs]").ToListAsync());
}
```

同样的操作也适用于第一个博客 Posts 数组中的第二个帖子项，这确保了 `Include()` 方法按预期工作，为博客项返回对应的帖子：

![](img/17e01f90db603e72c0f0c6a7dce8920c_194_0.png)

作为额外检查，我们可以验证第二个博客项及其对应的单个帖子，该帖子也被包含在内：

![](img/17e01f90db603e72c0f0c6a7dce8920c_195_0.png)

我们已经看到了 `Include()` 功能如何与使用 LINQ 组合的扁平 SQL 查询一起工作；让我们看看它是如何被翻译成 SQL 查询的（LINQ to SQL）。下面的截图显示我们执行了两个 SQL 语句，其中第一个语句与之前从 `Blog` 表检索数据的语句类似：

![](img/17e01f90db603e72c0f0c6a7dce8920c_196_0.png)

第二个语句为我们完成了任务，执行了将帖子表数据包含到 LINQ 查询中的所需代码。这在下面的截图中有所体现：

![](img/17e01f90db603e72c0f0c6a7dce8920c_197_0.png)

在本节中，我们已经了解了如何使用 LINQ 将扁平 SQL 查询消费为 SQL 查询，到目前为止我们已经看到了相对于 DBSet 或 POCO 对象的扁平 SQL 查询的执行。让我们研究一下在 *Executing SQL query without a DBSet or POCO* 部分是否可以匿名实现同样的功能。

## Executing SQL query without a DBSet or POCO

我们需要研究 Microsoft 是否支持在不需要 DBSet 或 POCO 对象的情況下匿名执行扁平 SQL。由于它是开源的，我们很容易调查这一点。我们可以直接进入 GitHub 仓库（https://github.com/aspnet/EntityFramework）中的源代码，调查他们是否对预期行为有任何实现。

Microsoft 确实为关系数据库提供了一个外观的几个扩展方法，可在 https://github.com/aspnet/EntityFramework/blob/0024373adae7e331ed217de2b4bd12be5eedf925/src/EFCore.Relational/RelationalDatabaseFacadeExtensions.cs 获取。

位于前述 GitHub 位置的扩展方法如下所示。我们需要捕获它，因为我们将在本节中讨论其实现：

![](img/17e01f90db603e72c0f0c6a7dce8920c_198_0.png)

他们对原始 SQL 查询有实现，但这是一个有限的实现。它具有 `ExecuteSqlCommand`，仅支持 `ExecuteNonQuery` 功能，这意味着我们不能使用此方法来处理返回结果集的 SQL 查询：

```
```csharp
public static int ExecuteSqlCommand(
    [NotNull] this DatabaseFacade databaseFacade,
    RawSqlString sql,
    [NotNull] params object[] parameters);

// Note that this method doesn't start a transaction hence it doesn't use ExecutionStrategy
public static int ExecuteSqlCommand(
    [NotNull] this DatabaseFacade databaseFacade,
    [NotNull] FormattableString sql)
    => ExecuteSqlCommand(databaseFacade, sql.Format, sql.GetArguments());

// Note that this method doesn't start a transaction hence it doesn't use ExecutionStrategy
public static int ExecuteSqlCommand(
    [NotNull] this DatabaseFacade databaseFacade,
    RawSqlString sql,
    [NotNull] IEnumerable<object> parameters)
{
    Check.NotNull(databaseFacade, nameof(databaseFacade));
    Check.NotNull(sql, nameof(sql));
    Check.NotNull(parameters, nameof(parameters));

    var concurrencyDetector = databaseFacade.GetService<IConcurrencyDetector>();

    using (concurrencyDetector.EnterCriticalSection())
    {
        var rawSqlCommand = databaseFacade
            .GetRelationalService<IRawSqlCommandBuilder>()
            .Build(sql, parameters);

        return rawSqlCommand
            .RelationalCommand
            .ExecuteNonQuery(
                databaseFacade.GetRelationalService<IRelationalConnection>(),
                rawSqlCommand.ParameterValues);
    }
}
```
```

同样的实现也有异步版本，这告诉我们在计划扩展我们的需求时需要执行两个实现。下面的截图将显示异步实现：

```
154 RawSqlString sql,
155 CancellationToken cancellationToken = default(CancellationToken)
156 => ExecuteSqlCommandAsync(databaseFacade, sql, Enumerable.Empty<object>(), cancellationToken);
157
158 // Note that this method doesn't start a transaction hence it doesn't use ExecutionStrategy
159 public static Task<int> ExecuteSqlCommandAsync(
160 [NotNull] this DatabaseFacade databaseFacade,
161 RawSqlString sql,
162 [NotNull] params object[] parameters)
163 => ExecuteSqlCommandAsync(databaseFacade, sql, (IEnumerable<object>)parameters);
164
165 // Note that this method doesn't start a transaction hence it doesn't use ExecutionStrategy
166 public static async Task<int> ExecuteSqlCommandAsync(
167 [NotNull] this DatabaseFacade databaseFacade,
168 RawSqlString sql,
169 [NotNull] IEnumerable<object> parameters,
170 CancellationToken cancellationToken = default(CancellationToken)
171 {
172 Check.NotNull(databaseFacade, nameof(databaseFacade));
173 Check.NotNull(sql, nameof(sql));
174 Check.NotNull(parameters, nameof(parameters));
175
176 var concurrencyDetector = databaseFacade.GetService<IConcurrencyDetector>();
177
178 using (await concurrencyDetector.EnterCriticalSectionAsync(cancellationToken))
179 {
180 var rawSqlCommand = databaseFacade
181 .GetRelationalService<IRawSqlCommandBuilder>()
182 .Build(sql.Format, parameters);
183
184 return await rawSqlCommand
185 .RelationalCommand
186 .ExecuteNonQueryAsync(
187 databaseFacade.GetRelationalService<IRelationalConnection>(),
188 rawSqlCommand.ParameterValues,
189 cancellationToken);
190 }
```

EF Core 社区针对我们的需求创建了一个开放问题，可在 https://github.com/aspnet/EntityFramework/issues/1862#issuecomment-220787464 获取。

在前述问题线程中，用户 sirentek 为我们在 Microsoft 在框架中修复之前提供了一个变通方法。让我们实现匿名 SQL 查询执行所需的扩展方法：

- 1. 创建一个 DatabaseExtensions 静态类来保存扩展方法。
- 2. 实现 `ExecuteSqlQuery` 用于返回结果集的 SQL 查询，而 `ExecuteSqlCommand` 不返回任何内容。
- 3. 实现 `ExecuteSqlQueryAsync` 用于异步操作，与 Microsoft 的 `ExecuteSqlCommandAsync` 实现相匹配。

Microsoft 目前没有计划在 EF Core 2.0 中包含它们，甚至也不在当前时间。这与我们之前讨论的匿名类型支持类似：

```
public static class DatabaseExtensions
{
    public static RelationalDataReader ExecuteSqlQuery(this
        DatabaseFacade databaseFacade, string sql, params
        object[] parameters)
    {
        var concurrencyDetector =
            databaseFacade.GetService<IConcurrencyDetector>();
        using (concurrencyDetector.EnterCriticalSection())
        {
            var rawSqlCommand = databaseFacade
                .GetService<IRawSqlCommandBuilder>()
                .Build(sql, parameters);

            return rawSqlCommand
                .RelationalCommand
                .ExecuteReader(
                    databaseFacade.GetService<IRelationalConnection>(),
                    parameterValues: rawSqlCommand.ParameterValues);
        }
    }

    public static async Task<RelationalDataReader>
        ExecuteSqlQueryAsync(this DatabaseFacade databaseFacade,
        string sql,
        CancellationToken cancellationToken = default(CancellationToken),
        params object[] parameters)
    {
        var concurrencyDetector =
            databaseFacade.GetService<IConcurrencyDetector>();
        using (concurrencyDetector.EnterCriticalSection())
        {
            var rawSqlCommand = databaseFacade
                .GetService<IRawSqlCommandBuilder>()
                .Build(sql, parameters);

            return await rawSqlCommand
                .RelationalCommand
```

