The following code contains entity and its configuration of single navigation property, where Blog entity contains the navigation property and configuration uses HasOne() to complete the relationship:

```
public class Blog
{
    public int Id { get; set; }
    public string Url { get; set; }
    public ICollection<Post> Posts { get; set; }
}
public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }
    public DateTime PublishedDateTime { get; set; }
    // Foreign key "BlogId" and navigation property "Blog" was
    removed here
}

// onModelCreating method implementation inside context class
modelBuilder.Entity<Blog>()
    .HasMany(x => x.Posts)
    .WithOne()
```

The following diagram, illustrates the relationship built between entities with single navigation property:

![](bbox=[0.28, 0.6, 0.68, 0.88])