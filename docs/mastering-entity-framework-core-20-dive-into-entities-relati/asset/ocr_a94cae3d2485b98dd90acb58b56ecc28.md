```
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
```

We have seen how tracking works, ways to disable it, and also explored projections and their tracking behavior in EF Core. In the next section, let's investigate how detecting change behavior could be modified.

# Detecting changes

We might run into performance issue while trying to perform bulk insertion that might be importing data from other sources or migrating content from another blogging system. In those scenarios, if we have `AutoDetectChangesEnabled`, then we might run into performance issues since EF Core's `ObjectStateManager.DetectChanges()` performs too costly operations on each insert operation.

For illustration, we could handle the bulk insert/update detect changes partly in our `CreatePost` code, which will perform a bulk tag insertion. We could handle this issue by disabling `AutoDetectChangesEnabled` just before adding the entities and enabling them back before calling `SaveChanges()`. The following little highlighted change would provide a huge performance improvement against the execution time:

```
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
                TagId = tagId,
```
```