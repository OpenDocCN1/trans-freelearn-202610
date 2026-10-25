
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

We have seen how tracking works, ways to disable it, and also explored projections and their tracking behavior in EF Core. In the next section, let's investigate how detecting change behavior could be modified.

## Detecting changes

We might run into performance issue while trying to perform bulk insertion that might be importing data from other sources or migrating content from another blogging system. In those scenarios, if we have `AutoDetectChangesEnabled`, then we might run into performance issues since EF Core's `ObjectStateManager.DetectChanges()` performs too costly operations on each insert operation.

For illustration, we could handle the bulk insert/update detect changes partly in our `CreatePost` code, which will perform a bulk tag insertion. We could handle this issue by disabling `AutoDetectChangesEnabled` just before adding the entities and enabling them back before calling `SaveChanges()`. The following little highlighted change would provide a huge performance improvement against the execution time:

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

We have seen how `DetectChanges()` affects the performance of bulk insert/update operations and the solution to the issue. In the next section, let's focus on asynchronous operations.

## Asynchronous operations

We could call it a muscle memory, as we could notice that an asynchronous operation is already in place; we perform all operations in an asynchronous way. Even though it's been covered, let's reiterate how the asynchronous operations were handled in our blogging system and see the simplicity when we used `async/await` syntactic sugar implementations for asynchronous operations.

The asynchronous execution helps us in supporting multiple requests in parallel, without locking a thread against a long-running process. To understand how the threads should be managed properly, let's go through the following execution process:

- The request reaching the ASP.NET pipeline will be allocated to a thread, which takes care of the execution
- The thread will be occupied until the request is complete
- If the action consumes any long-running process in a synchronous way, then the thread will be blocked until the process execution is complete. If all the threads in the thread pool is occupied, then it cannot serve additional requests
- If the long-running process is implemented in an asynchronous way, then once the action is triggered, the thread will be released back to the thread pool, the released thread would be allocated to new request
- Once the long-running process is complete, a new thread will be allocated to complete the request execution

Since we already have an asynchronous implementation in place, we will start examining them right from the controller up to the data context execution. Let's take a look at the post controller `Index` action that is decorated with the `async` keyword, which also requires a return type to be wrapped inside a `Task<>` generic type. The asynchronous method should be invoked using the `await` keyword as shown in the following code:

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

Let's dig in deep and investigate how this `GetAsync()` is implemented. It is a generic implementation that invokes `HandleAsync()` using the async/await keywords as shown here:

```
public async Task<IEnumerable<Post>> GetAsync<T>(T query)
    where T : IQueryHandlerAsync<IEnumerable<Post>>
{
    return await query.HandleAsync();
}
```

The `HandleAsync()` method is again implemented inside `GetAllPostsQuery` (in our scenario), which consumes the EF Core asynchronous `ToListAsync()` method, which wraps up our asynchronous implementation. This ensures that the database execution happens in an asynchronous way, and the operation continues after the data is returned from the data store. The following code performs asynchronous execution on `ToListAsync()` methods which will perform database operation:

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

## Unnecessary volume returned

We have been using data luxuriously so far without keeping usage in mind. One fine example is the delete operation, which we can see in all the CRUD operations. If we take a look at the Person delete operation, we can see that the entire Person entity was returned from the context and it was used for the deletion. The following code retrieves the Person entity and uses the entity for delete operation:

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

The pitfall for the preceding approach was, first a database call was made to retrieve the entire Person object, which is an overhead. After that, the required delete operation was performed using the Id of the person, which was available before the Person retrieval. The following screenshot is highlighted with the discussed two queries:

![](img/17e01f90db603e72c0f0c6a7dce8920c_318_0.png)

We could simplify the approach by removing the person retrieval and creating a Person object using just the Id value, and updating the state of the entity as Deleted would instruct EF Core to perform a delete operation directly without retrieving the object from the data store. The following code reduces the query into one which performs delete operation alone:

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

We can see that the preceding approach does not make any Person retrievals yet. Still, the delete operation was performed successfully, as shown in the following screenshot:

![](img/17e01f90db603e72c0f0c6a7dce8920c_319_0.png)

We have seen ways to avoid retrieving unnecessary data from the data store. In the next section, we will explore the `N + 1 Select` problem.

## The N+1 Select problem

The `N + 1 Select` problem arises due to an internal data store call made by any ORM framework to perform a single operation. The problem could be outlined as follows:

- 1. A list of entities was returned that will be further iterated to perform some additional operation.
- 2. Assuming a navigational property is accessed inside the iteration:
  - Each time an iteration tries to access the navigational property, a database hit occurs
  - The hits will happen N times for N number of items returned in the first step
- 3. This makes the application perform `N+1 Select` (N for the iterations and 1 for the first call that retrieves the list), which should have been performed in a single database call.

This behavior is often termed as **lazy loading** in any ORM frameworks, which loads the subsequent/dependent data only when required. The lazy loading is yet to be supported by EF Core. This can be verified in the EF Core road map, which lists it as **Stretch goals**, as shown in the following screenshot:

![](img/17e01f90db603e72c0f0c6a7dce8920c_320_0.png)
