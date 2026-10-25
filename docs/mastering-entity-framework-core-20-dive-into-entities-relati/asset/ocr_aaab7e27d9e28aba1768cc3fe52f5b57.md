# Creating a simple transaction

We have seen the default behavior of a transaction if the multiple persistence occurs within a single SaveChanges(), what if we have a business requirement where multiple SaveChanges() are required and we still need to maintain the transaction scope? We have a provision to explicitly define a transaction and bring in the changes required inside this scope; EF exposes the Database type in Context, which has the BeginTransaction() functionality.

Let's try to achieve the aforementioned functionality in our CreatePostCommand type and still maintain the transaction scope. We could perform the same by retrieving the transaction object using the BeginTransaction() function; the retrieved object could be used either to commit or roll back the changes. The following code commits the changes after persisting the Post and Tag objects:

```
public class CreatePostCommand : CommandBase, ICreatePostCommand<int>
{
    // Code removed for brevity
    public int Handle()
    {
        int returnValue;
        using (var transaction = Context.Database.BeginTransaction())
        {
            // Code removed for brevity
            Context.Add(post);
            returnValue = Context.SaveChanges();
            foreach (int tagId in TagIds)
            {
                Context.Add(new TagPost
                {
                    TagId = tagId,
                    Post = post
                });
            }
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
```