# Configuring timestamp tokens through data annotation

We have already seen how data annotation configuration works. For timestamp-based tokens, we need a property that will have a byte array, and it should be marked using the Timestamp data annotation. This is the only configuration required from our end; EF will take care of the rest:

```
public class Post
{
    // Code removed for brevity
    [Timestamp]
    public byte[] Timestamp { get; set; }
}
```

The preceding configuration will let EF Core consider the Timestamp column as the concurrency column, and any further update to this column will restrict users from performing parallel updates.

Since we are introducing a new column to the entity, related commands should be updated as well. In our scenario, we need to update the UpdatePostCommand type with new byte array Timestamp property and its usage as shown in the following code:

```
public class UpdatePostCommand : CommandBase, ICreatePostCommand<int>
{
    // Code removed for brevity
    public byte[] Timestamp { get; set; }
    // Code removed for brevity
    public async Task<int> HandleAsync()
    {
        var post = new Post
        {
            // Code removed for brevity
            Timestamp = Timestamp
        };
        // Code removed for brevity
    }
}
```