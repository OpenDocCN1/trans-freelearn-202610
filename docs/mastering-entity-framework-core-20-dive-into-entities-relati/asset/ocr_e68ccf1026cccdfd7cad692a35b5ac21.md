The following code shows how the concurrency conflicts are handled in the user-specific custom resolution approach. It just adds the model state error on to the conflicting fields with the current value, which gets displayed in the client/UI. The following code would handle concurrency and update the model state errors based on validation errors:

```
public async Task<IActionResult> Edit(int id, [Bind("Id,Title, Content,Summary," + "PublishedDateTime,Url,VisitorCount, CreatedAt,ModifiedAt, BlogId,AuthorId," +"CategoryId,TagIds, FileId,Timestamp")] Post post, IFormFile headerImage)
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
        {
            if (!PostExists(post.Id))
            {
                return NotFound();
            }
            else
            {
                try
                {
                    foreach (var entry in dbUpdateConcurrencyException.Entries)
                    {
                        if (entry.Entity is Post)
                        {
                            var postEntry = entry.GetDatabaseValues();
                            var postFromDatabase = (Post)postEntry.ToObject();
                            var postToBeUpdated = (Post)entry.Entity;
                            if (postFromDatabase.Title != postToBeUpdated.Title)
                            {
                                ModelState.AddModelError("Title", $"Current value: {postFromDatabase.Title}");
                            }
                            if (postFromDatabase.Content != postToBeUpdated.Content)
                            {
                                ModelState.AddModelError("Content", $"Current value:

```