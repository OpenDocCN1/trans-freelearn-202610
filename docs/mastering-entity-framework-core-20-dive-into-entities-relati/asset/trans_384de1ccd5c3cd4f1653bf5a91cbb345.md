

returnValue = await Context.SaveChangesAsync();
transaction.Commit();
}
return returnValue;
}
// Code removed for brevity
}

删除操作所需的代码如下：

```
public class DeletePostCommand : CommandBase, ICreatePostCommand<int>
{
    // Code removed for brevity
    public int Handle()
    {
        int returnValue;
        using (var transaction = Context.Database.BeginTransaction())
        {
            DeletePost();
            returnValue = Context.SaveChanges();
            DeleteTag();
            returnValue = Context.SaveChanges();
            transaction.Commit();
        }
        return returnValue;
    }

    public async Task<int> HandleAsync()
    {
        int returnValue;
        using (var transaction = Context.Database.BeginTransaction())
        {
            DeletePost();
            returnValue = await Context.SaveChangesAsync();
            DeleteTag();
            returnValue = await Context.SaveChangesAsync();
            transaction.Commit();
        }
        return returnValue;
    }
    // Code removed for brevity
}
```

## 科普天文学的现状与思考

孙正凡 (上海科学技术出版社)

天文科普是科学传播的重要组成部分。近年来,随着我国航天事业的发展和天文学研究的进步,社会公众对天文学的兴趣日渐浓厚,天文科普事业也迎来了新的机遇和挑战。

天文学是一门观测科学,离不开大型观测设备的支撑。但普通公众接触天文学,更多是通过媒体和科普活动。当前天文科普工作中存在的主要问题,一是专业天文学家参与科普的积极性不足,二是科普内容同质化现象比较突出,三是缺乏系统性的科普人才培养机制。

针对上述问题,我认为应当从以下几个方面入手改进天文科普工作:第一,鼓励更多的天文学家利用业余时间参与公众科普;第二,结合科技热点和新闻事件,及时开展有针对性的科普活动;第三,探索天文科普的新形式,让公众能够参与真实的天文数据处理,提升参与感和获得感。

令人欣喜的是,新一代天文从业者已经充分认识到科普的重要性,在做好科研工作的同时,积极投身于面向公众的天文知识传播。这种自觉的科普意识,正是天文科普事业持续发展的希望所在。

参考文献:
[1] 刘慈欣. 三体[M]. 重庆出版社, 2008.
[2] 卡尔·萨根. 宇宙[M]. 吉林人民出版社, 1998.
[3] 卞毓麟. 星星离我们有多远[M]. 湖南教育出版社, 2000.

我们需要命令接口 `ICreateFileCommand` 来定义 `CreateFileCommand` 执行所需的处理程序。我们将使用现有的 `ICommandHandler` 和 `ICommandHandleAsync` 作为基类型，如下所示：

```
public interface ICreateFileCommand<TReturn> :
    ICommandHandler<TReturn>, ICommandHandlerAsync<TReturn>
{
}
```

由于 `CommandBase` 使用博客上下文，我们需要一个单独的带有博客文件数据上下文的 `CommandFileBase`；`BlogFilesContext` 被 `CommandFileBase` 所使用，如下面代码所示：

```
public class CommandFileBase
{
    internal readonly BlogFilesContext Context;
    public CommandFileBase(BlogFilesContext context)
    {
        Context = context;
    }
}
```

`CreateFileCommand` 类型执行文件持久化，它维护事务，也使用通过构造函数传递给它的事务；通过构造函数共享的事务及其用法如下面代码所示：

```
public class CreateFileCommand : CommandFileBase,
    ICreateFileCommand<int>
{
    private readonly DbTransaction _dbTransaction;
    public CreateFileCommand(BlogFilesContext context) : base(context)
    {
    }
    public CreateFileCommand(BlogFilesContext context, DbTransaction
        dbTransaction)
        : this(context)
    {
        _dbTransaction = dbTransaction;
    }
    public Guid Id { get; set; }
    public string ContentType { get; set; }
    public string ContentDisposition { get; set; }
    public byte[] Content { get; set; }
    public long Length { get; set; }
    public string Name { get; set; }
```

```csharp
public string FileName { get; set; }
public int Handle()
{
    int returnValue = 0;
    using (var transaction = _dbTransaction != null
        ? Context.Database.UseTransaction(_dbTransaction)
        : Context.Database.BeginTransaction())
    {
        try
        {
            AddFile();
            returnValue = Context.SaveChanges();
            transaction.Commit();
        }
        catch (Exception exception)
        {
            transaction.Rollback();
            ExceptionDispatchInfo.Capture
                (exception.InnerException).Throw();
        }
    }
    return returnValue;
}
private void AddFile()
{
    File file = new File()
    {
        Id = Guid.NewGuid(),
        Name = Name,
        FileName = FileName,
        Content = Content,
        Length = Length,
        ContentType = ContentType
    };
    Context.Add(file);
}
public async Task<int> HandleAsync()
{
    int returnValue = 0;
    using (var transaction = _dbTransaction != null
        ? Context.Database.UseTransaction(_dbTransaction)
        : Context.Database.BeginTransaction())
    {
        try
        {
            AddFile();
            returnValue = await Context.SaveChangesAsync();
            transaction.Commit();
```

```
}
catch (Exception exception)
{
    transaction.Rollback();
    ExceptionDispatchInfo.Capture(
        exception.InnerException).Throw();
}
}
return returnValue;
}
```

Update 和 Delete 操作也需要类似的更改；让我们将其作为一个任务来完成。

该任务的解决方案在 Git 仓库中可用，文件持久化的核心实现可在 https://goo.gl/CUxFs9 获取。
文件持久化的具体实现可在 https://goo.gl/1fEbCa 获取。

`CreatePostCommand` 接收文件以及帖子内容。它创建事务并执行帖子持久化，并将其事务传递给文件命令，文件命令在其中持久化文件并重用事务，如下面代码中突出显示的那样：

```
public class CreatePostCommand : CommandBase, ICreatePostCommand<int>
{
    // Code removed for brevity
    public CreatePostCommand(BlogContext context, BlogFilesContext blogFilesContext)
        : this(context)
    {
        _blogFilesContext = blogFilesContext;
    }
    // Code removed for brevity
    public ICollection<int> TagIds { get; set; }
    public File File { get; set; }
    public int Handle()
    {
        // Code removed for brevity
    }
    public async Task<int> HandleAsync()
    {
        int returnValue = 0;using (var transaction =
            Context.Database.BeginTransaction())
```

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

Update 和 Delete 操作也需要类似的更改；让我们将其作为一个任务来完成。

该任务的解决方案可在 Git 仓库中获取，地址为 https://goo.gl/FDP1q4。

Create 操作支持文件上传功能，文件内容被传递给创建命令以将文件内容持久化到文件上下文。以下代码在控制器的 Create 操作中提供了文件上传支持：

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

![](img/17e01f90db603e72c0f0c6a7dce8920c_250_0.png)

