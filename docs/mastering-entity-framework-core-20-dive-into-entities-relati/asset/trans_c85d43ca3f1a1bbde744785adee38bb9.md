
```csharp
{
    var aboutViewModel = _context.People
        .Where(item => item.Id.Equals(id))
        .Select(item => new AboutViewModel
        {
            Name = item.FirstName,
            Biography = item.Biography
        }).SingleOrDefault();

    return View(aboutViewModel);
}
```

我们已经了解了跟踪的工作方式、如何禁用它，并探讨了 EF Core 中的投影及其跟踪行为。在下一节中，让我们研究如何修改更改检测行为。

## 更改检测

在进行批量插入（例如从其他来源导入数据或从另一个博客系统迁移内容）时，我们可能会遇到性能问题。在这些场景中，如果启用了 `AutoDetectChangesEnabled`，则可能会遇到性能问题，因为 EF Core 的 `ObjectStateManager.DetectChanges()` 会在每次插入操作时执行代价高昂的操作。

为了说明问题，我们可以在 `CreatePost` 代码中部分处理批量插入/更新的更改检测，该代码将执行批量标签插入。我们可以通过在添加实体之前禁用 `AutoDetectChangesEnabled`，并在调用 `SaveChanges()` 之前重新启用来解决此问题。下面这个被突出显示的小小改动将大大缩短执行时间：

```csharp
public class CreatePostCommand : CommandBase,
    ICreatePostCommand<int>
{
    // Code removed for brevity
    public int Handle()
    {
        // Code removed for brevity
        Context.ChangeTracker.AutoDetectChangesEnabled = false;
        foreach (int tagId in TagIds)
        {
            Context.Add(new TagPost
            {
```

```csharp
Post = post
});
}
Context.ChangeTracker.AutoDetectChangesEnabled = true;
// Code removed for brevity
}
// Code removed for brevity
}
```

我们已经了解了 `DetectChanges()` 如何影响批量插入/更新操作的性能以及该问题的解决方案。在下一节中，让我们重点关注异步操作。

## 异步操作

我们可以称之为肌肉记忆，因为我们可以看到异步操作已经就位；我们以异步方式执行所有操作。尽管已经介绍过了，但让我们重申一下在博客系统中如何处理异步操作，并看看使用 `async/await` 语法糖实现异步操作时的简洁性。

异步执行帮助我们并行支持多个请求，而不会将线程锁定在长时间运行的过程上。为了理解如何正确管理线程，让我们了解以下执行过程：

- 到达 ASP.NET 管道的请求会被分配给一个线程，由该线程负责执行
- 在请求完成之前，该线程会被占用
- 如果操作以同步方式执行任何长时间运行的过程，则该线程会被阻塞，直到过程执行完成。如果线程池中的所有线程都被占用，则无法再为其他请求提供服务
- 如果长时间运行的过程以异步方式实现，那么一旦操作触发，线程会被释放回线程池，释放的线程将被分配给新的请求
- 一旦长时间运行的过程完成，将分配一个新的线程来完成该请求的执行

由于我们已经有了异步实现，我们将从控制器一直到数据上下文执行来检查它们。让我们看一下文章控制器的 `Index` 操作，它使用 `async` 关键字进行修饰，这也要求返回类型包装在 `Task<>` 泛型类型中。异步方法应使用 `await` 关键字调用，如以下代码所示：

```
public async Task<IActionResult> Index()
{
    var posts = await _postRepository.GetAsync(
    new GetAllPostsQuery(_context)
    {
        IncludeData = true
    });
    // Code removed for brevity
}
```

让我们深入研究并调查这个 `GetAsync()` 是如何实现的。它是一个泛型实现，使用 async/await 关键字调用 `HandleAsync()`，如下所示：

```
public async Task<IEnumerable<Post>> GetAsync<T>(T query)
    where T : IQueryHandlerAsync<IEnumerable<Post>>
{
    return await query.HandleAsync();
}
```

`HandleAsync()` 方法在 `GetAllPostsQuery`（在我们的场景中）中实现，它使用 EF Core 的异步 `ToListAsync()` 方法，从而封装了我们的异步实现。这确保数据库执行以异步方式进行，并且在从数据存储返回数据后操作继续执行。以下代码对 `ToListAsync()` 方法执行异步操作，该方法将执行数据库操作：

```
public class GetAllPostsQuery : QueryBase,
    IGetAllPostsQuery<GetAllPostsQuery>
{
    // Code removed for brevity
    public async Task<IEnumerable<Post>> HandleAsync()
    {
        var posts = IncludeData
            ? await Context.Posts
            .Include(p => p.Author).Include(p => p.Blog)
            .Include(p => p.Category).Include(p=>p.TagPosts)
            .ToListAsync()
            : await Context.Posts
            .ToListAsync();
```

## 返回不必要的数据量

到目前为止，我们一直在奢侈地使用数据，而没有考虑使用量。一个很好的例子是删除操作，我们在所有 CRUD 操作中都能看到。如果我们看一下 Person 删除操作，可以看到整个 Person 实体是从上下文中返回的，并且它被用于删除操作。以下代码检索 Person 实体并使用该实体进行删除操作：

```
public async Task<IActionResult> DeleteConfirmed(int id)
{
    var person =
        await _context.People.SingleOrDefaultAsync(m => m.Id == id);
    _context.People.Remove(person);
    await _context.SaveChangesAsync();
    return RedirectToAction("Index");
}
```

前一种方法的缺陷在于，首先进行了数据库调用来检索整个 Person 对象，这是一个开销。之后，通过 Person 的 Id 执行所需的删除操作，而该 Id 在检索 Person 之前就已可用。以下截图突出显示了所讨论的两个查询：

![](img/17e01f90db603e72c0f0c6a7dce8920c_318_0.png)

我们可以简化这种方法：省略 Person 检索，仅使用 Id 值创建 Person 对象，并将实体的状态更新为 Deleted，这样就能指示 EF Core 直接执行删除操作，而无需从数据存储检索对象。以下代码将查询简化为仅执行删除操作的查询：

```
[HttpPost, ActionName("Delete")]
[ValidateAntiForgeryToken]
public async Task<IActionResult> DeleteConfirmed(int id)
{
    Person person = new Person() { Id = id };
    _context.Entry(person).State = EntityState.Deleted;
    await _context.SaveChangesAsync();
    return RedirectToAction("Index");
}
```

可以看到，前一种方法无需进行任何 Person 检索，然而删除操作仍然成功执行，如下面的截图所示：

![](img/17e01f90db603e72c0f0c6a7dce8920c_319_0.png)

我们已经了解了避免从数据存储中检索不必要数据的方法。在下一节中，我们将探讨 `N + 1 Select` 问题。

## N+1 Select 问题

`N + 1 Select` 问题的出现是由于任何 ORM 框架在内部执行单个操作时会多次调用数据存储。该问题可概述如下：

- 1. 返回一个实体列表，该列表将被进一步迭代以执行一些附加操作。
- 2. 假设在迭代内访问了导航属性：
  - 每次迭代尝试访问该导航属性时，都会产生一次数据库访问
  - 对于第一步中返回的 N 个项目，将发生 N 次访问
- 3. 这使得应用程序执行 `N+1 Select` 操作（N 次迭代，1 次是获取列表的首次调用），而实际上这些操作本应在一次数据库调用中完成。

这种行为在任何 ORM 框架中通常被称为**懒加载**，它会仅在需要时加载后续/依赖数据。EF Core 尚不支持懒加载。这可以在 EF Core 的路线图中得到验证，该路线图将其列为**延伸目标**，如以下截图所示：

![](img/17e01f90db603e72c0f0c6a7dce8920c_320_0.png)
