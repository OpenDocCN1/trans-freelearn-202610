![](bbox=[0.11, 0.18, 0.18, 0.27])

Usually, we will perform SaveChanges() twice, one for Post and another for the Tag object. The first SaveChanges() will insert Post and allow us to fetch the auto-generated PostId that will be used in Tag row creation. We could leverage the default transaction by making only one SaveChanges() call and still fill the PostId data by passing the whole Post object to the Tag object, and EF ensures that it fills the PostId once the Post row is created.

EF ensures that even if any one of the transactions fails, the changes will be rolled back, as EF covers our back by default and we need to ensure that we make only one SaveChanges() to leverage this transaction behavior. The following implementation performs write operations in Post and Tag entities, yet having only one SaveChanges() ensures that transaction is maintained:

```
public class CreatePostCommand : CommandBase,
    ICreatePostCommand<int>
{
    // Code removed for brevity
    public ICollection<int> TagIds { get; set; }
    public int Handle()
    {
        Post post = new Post
        {
            // Code removed for brevity
        };
        Context.Add(post);
        foreach (int tagId in TagIds)
        {
            Context.Add(new TagPost
            {
                TagId = tagId,
                Post = post
            });
        }
        return Context.SaveChanges();
    }

    public async Task<int> HandleAsync()
    {
        Post post = new Post
        {
            // Code removed for brevity
        };
        Context.Add(post);
        foreach (int tagId in TagIds)
```