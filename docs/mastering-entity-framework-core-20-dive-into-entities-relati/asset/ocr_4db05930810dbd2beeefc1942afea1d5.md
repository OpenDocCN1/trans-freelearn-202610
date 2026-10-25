```
{
    dbUpdateConcurrencyException.Entries.Single().Reload();
    await _context.SaveChangesAsync();
    return RedirectToAction("Index");
}
catch (Exception exception)
{
    ExceptionDispatchInfo.Capture
        (exception.InnerException).Throw();
}
}
}
// Code removed for brevity
}
```

We have seen how the database wins approach handles the concurrency conflict. In the next section, let's explore how the client wins approach handles the concurrency conflict.

# Client wins

The **client wins** approach allows the system/application to override the database changes with the client/UI changes and update the data into the database.

The following code shows how the concurrency conflicts are handled in the client wins approach. It just updates the original values from the database, which will avoid concurrency conflict. It also allows the system/application to persist the changes from client/UI to be updated in the database without any issues:

```
public async Task<IActionResult> Edit(int id, [Bind("Id,Title,Content,Summary," + "PublishedDateTime,Url,VisitorCount,CreatedAt,ModifiedAt,BlogId,AuthorId," + "CategoryId,TagIds,FileId,Timestamp")] Post post, IFormFile headerImage)
{
    if (id != post.Id)
    {
        return NotFound();
    }
    if (ModelState.IsValid)
    {
        var transactions = new TransactionScope();
        try
        {
            // Code removed for brevity
        }
    catch (DbUpdateConcurrencyException dbUpdateConcurrencyException)
```