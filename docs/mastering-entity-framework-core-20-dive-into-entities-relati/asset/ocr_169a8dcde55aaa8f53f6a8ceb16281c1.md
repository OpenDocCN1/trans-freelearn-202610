```
{
    try
    {
        CreateFileCommand createFileCommand =
        new CreateFileCommand(_blogFilesContext,
        transaction.GetDbTransaction())
        {
            Id = Guid.NewGuid(),
            Name = File.Name,
            FileName = File.FileName,
            Content = File.Content,
            Length = File.Length,
            ContentType = File.ContentType,
            ContentDisposition = File.ContentDisposition
        };
        returnValue = await createFileCommand.HandleAsync();
        Post post = new Post
        {
            // Code removed for brevity
            FileId = File.Id
        };
        // Code removed for brevity
        foreach (int tagId in TagIds)
        {
            Context.Add(new TagPost
            {
                TagId = tagId,
                Post = post
            });
        }
        returnValue = await Context.SaveChangesAsync();
        transaction.Commit();
    }
    catch (Exception exception)
    {
        transaction.Rollback();
        ExceptionDispatchInfo.
            Capture(exception.InnerException).Throw();
    }
    return returnValue;
}
```