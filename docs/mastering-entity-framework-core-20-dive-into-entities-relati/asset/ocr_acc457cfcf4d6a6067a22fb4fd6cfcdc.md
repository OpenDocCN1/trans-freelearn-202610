The new column should be included in the Edit method of PostController:

```
public async Task<IActionResult> Edit(
    int id, [Bind("Id,Title,Content,Summary," + "PublishedDateTime, Url,VisitorCount,CreatedAt,ModifiedAt,BlogId,AuthorId," + "CategoryId,TagIds,FileId,Timestamp")] Post post, IFormFile headerImage)
{
    // Code removed for brevity
    if (ModelState.IsValid)
    {
        var transactions = new TransactionScope();
        try
        {
            Models.File file = null;
            if (headerImage == null || (headerImage != null && !headerImage.ContentType.ToLower().StartsWith("image/")))
            {
                await _postRepository.ExecuteAsync(
                    new UpdatePostCommand(_context)
                {
                    // Code removed for brevity
                    Timestamp = post.Timestamp
                });
                return RedirectToAction("Index");
            }
            // Code removed for brevity
            transactions.Transactions.Add
                (_context.Database.BeginTransaction());
            await _postRepository.ExecuteAsync(
                new UpdatePostCommand(_context)
                {
                    // Code removed for brevity
                    Timestamp = post.Timestamp
                });
            transactions.Commit();
        }
        catch (DbUpdateConcurrencyException exception)
        {
            // Code removed for brevity
        }
        return RedirectToAction("Index");
    }
    // Code removed for brevity
}
```