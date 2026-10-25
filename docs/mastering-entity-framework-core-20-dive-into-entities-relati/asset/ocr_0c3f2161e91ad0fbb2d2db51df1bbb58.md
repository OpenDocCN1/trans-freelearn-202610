Similar changes are required for the Update and Delete operations; let's take it up as an assignment.

![](bbox=[0.14, 0.21, 0.21, 0.3])

The solution to the assignment is available in the Git repository at https://goo.gl/FDP1q4.

The Create action supports the file upload functionality, where the file content is passed on to the create command to persist the file content to the file context. The following code provides file upload support in the controller Create action:

```
public async Task<IActionResult> Create([Bind("Id,Title,Content,Summary," + "PublishedDateTime,Url,VisitorCount,CreatedAt,ModifiedAt,BlogId," + "AuthorId,CategoryId,TagIds")] Post post, IFormFile headerImage)
{
    if (ModelState.IsValid)
    {
        Models.File file = null;
        if (headerImage != null ||
            headerImage.ContentType.ToLower().StartsWith("image/"))
        {
            MemoryStream ms = new MemoryStream();
            headerImage.OpenReadStream().CopyTo(ms);
            file = new Models.File()
            {
                Id = Guid.NewGuid(),
                Name = headerImage.Name,
                FileName = headerImage.FileName,
                Content = ms.ToArray(),
                Length = headerImage.Length,
                ContentType = headerImage.ContentType
            };
        }
        await _postRepository.ExecuteAsync(
            new CreatePostCommand(_context, _filesContext)
            {
                // Code removed for brevity
                TagIds = post.TagIds,
                File = file
            });
        return RedirectToAction("Index");
    }
    // Code removed for brevity
}
```