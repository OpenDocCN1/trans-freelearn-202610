
The following code contains entity and its configuration of single navigation property, where Blog entity contains the navigation property and configuration uses HasOne() to complete the relationship:

```csharp
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

The following diagram illustrates the relationship built between entities with single navigation property:

![](img/17e01f90db603e72c0f0c6a7dce8920c_115_0.png)

We have modified the foreign key as `SomeBlogId` and configured against the `Blog` navigation property of the `Post` entity, and it is configured using the Fluent API.

## Building relationships using a principal key

The principal key doesn't require a Fluent API configuration if it's a primary key, but what if we had to apply a relationship against an alternate key? Then, we need the Fluent API to complete the configuration. We could achieve the same using the `HasPrincipalKey` Fluent API method exposed to us:

The code required for custom foreign key mapping in entity and configuration using principal key instead of primary key were displayed as follows:

```csharp
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
    public int SomeBlogId { get; set; }
    [ForeignKey("SomeBlogId")]
    public Blog Blog { get; set; }
}

// onModelCreating method implementation inside context class
modelBuilder.Entity<Post>()
    .HasOne(x => x.Blog)
    .WithMany(x => x.Posts)
    .HasForeignKey(x => x.SomeBlogId)
    .HasPrincipalKey(x => x.Url); // not a valid
        relationship in real time
```

We have created a relationship using the modified foreign key `BlogSomeId` against the alternate key `Url` from `Blog`. This was achieved using the `HasPrincipalKey` Fluent API method.

## Building relationships using the IsRequired method

We can configure the relationship as required or optional using the same Fluent API with the IsRequired method. We can use the same approach to define whether a relationship is mandatory or optional:

```csharp
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
    public int BlogSomeId { get; set; }
    [ForeignKey("BlogSomeId")]
    public Blog Blog { get; set; }
}

// onModelCreating method implementation inside context class
modelBuilder.Entity<Blog>()
    .Property(x => x.Url)
    .IsRequired();
modelBuilder.Entity<Post>()
    .HasOne(x => x.Blog)
    .WithMany(x => x.Posts)
    .IsRequired();
```

The first configuration applies the Url property on the Blog entity to be a mandatory/required field, and the second configuration enforces the relationship as mandatory.

## Cascade delete

The relationship we have created so far allows us to build the data with relationship, and the deletion mechanism needs to be configured as well. What would happen if we try to delete a principal entity row? Would that do nothing on dependent elements or remove them as well? Let’s see the different ways available to perform those operations:

- **Cascade:** If we delete the principal entity row, the dependent entity rows get deleted as well
  The following code would configure that the dependent rows would be deleted as well if the primary row is deleted:

```csharp
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
    public int BlogSomeId { get; set; }
    [ForeignKey("BlogSomeId")]
    public Blog Blog { get; set; }
}

// onModelCreating method implementation inside context class
modelBuilder.Entity<Post>()
    .HasOne(x => x.Blog)
    .WithMany(x => x.Posts)
    .OnDelete(DeleteBehavior.Cascade);
```

This enforces EF to delete Post elements if a Blog element is deleted.

- **SetNull:** If we delete the principal entity row, the dependent entity row's foreign key will be set to null values:
  The following code would configure the dependent rows to be set to null if the primary row is deleted:

```csharp
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
    public int BlogSomeId { get; set; }
    [ForeignKey("BlogSomeId")]
    public Blog Blog { get; set; }
}

// onModelCreating method implementation inside context class
modelBuilder.Entity<Post>()
    .HasOne(x => x.Blog)
    .WithMany(x => x.Posts)
    .OnDelete(DeleteBehavior.SetNull);
```

This enforces EF to set null values on the `Blog` column of the `Post` entity elements if a `Blog` element is deleted:

- **Restrict:** If we delete the principal entity row, the dependent entity's rows remain untouched:
  The following code would configure that the dependent rows would retain their values even though the primary row is deleted:

```csharp
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
    public int BlogSomeId { get; set; }
    [ForeignKey("BlogSomeId")]
    public Blog Blog { get; set; }
}

// onModelCreating method implementation inside context class
modelBuilder.Entity<Post>()
```

It could be resolved by unloading the project file and including the `DotNetCliToolReference` code block manually in the `.csproj` file content, then reloading the project and restoring the NuGet package. This should resolve the package-related issue:

```xml
<ItemGroup>
    <DotNetCliToolReference Include=
        "Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.1" />
</ItemGroup>
```

Go to the `Developer Command Prompt for VS 2017` (search for the command in the Start menu), and execute the following .NET CLI command to add the migration:

```bash
dotnet ef migrations add
```

The following screenshot adds the migration "Models with relationship" and the execution log shows `Done`. To undo this action, use `ef migrations remove`, which means the data migration was successful:

![](img/17e01f90db603e72c0f0c6a7dce8920c_122_0.png)

The database update could be performed using the following .NET CLI command:

```bash
dotnet ef database update
```

The following screenshot confirms that the database was updated successfully with a `Done` success message, as shown in the following screenshot:

```text
CONSTRAINT [PK_TagPost] PRIMARY KEY ([Id]),
CONSTRAINT [FK_TagPost_Post_PostId] FOREIGN KEY ([PostId]) REFERENCES [Post] ([Id]) ON DELETE CASCADE,
CONSTRAINT [FK_TagPost_Tag_TagId] FOREIGN KEY ([TagId]) REFERENCES [Tag] ([Id]) ON DELETE CASCADE
);
Info: Microsoft.EntityFrameworkCore.Database.Command[20101]
Executed DbCommand (2ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
CREATE UNIQUE INDEX [IX_Address_UserId] ON [Address] ([UserId]);
Info: Microsoft.EntityFrameworkCore.Database.Command[20101]
Executed DbCommand (1ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
CREATE INDEX [IX_Blog_AuthorId] ON [Blog] ([AuthorId]);
Info: Microsoft.EntityFrameworkCore.Database.Command[20101]
Executed DbCommand (1ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
CREATE INDEX [IX_Post_BlogId] ON [Post] ([BlogId]);
Info: Microsoft.EntityFrameworkCore.Database.Command[20101]
Executed DbCommand (24ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
CREATE INDEX [IX_Post_UserId] ON [Post] ([UserId]);
Info: Microsoft.EntityFrameworkCore.Database.Command[20101]
Executed DbCommand (1ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
CREATE INDEX [IX_TagPost_PostId] ON [TagPost] ([PostId]);
Info: Microsoft.EntityFrameworkCore.Database.Command[20101]
Executed DbCommand (1ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
CREATE INDEX [IX_TagPost_TagId] ON [TagPost] ([TagId]);
Info: Microsoft.EntityFrameworkCore.Database.Command[20101]
Executed DbCommand (36ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
VALUES (N'20171123090841_Models with relationship', N'2.0.1-rtm-125');
Done.
C:\Users\Prabhakar\Documents\GitHub\Mastering-Entity-Framework-Core\Chapter 4\Final\MasteringEFCore.BuildRelationships.Final>
```

I love to fool people in the beginning and then provide them with a solution, and that makes sure that they never forget it. Let me break the ice, the migration issue could be simply resolved by removing and adding the Microsoft.EntityFrameworkCore.Tools NuGet package.

Yes, we have explored .NET CLI commands within the context of a data migration issue, and also learned how to resolve it using both CLI and package update in this section.

## Summary

We have learned and built different kinds of relationships supported by EF in this chapter. We started with regular one-to-one, one-to-many, and many-to-many relationships, and from there applied a few Fluent API methods to achieve relationships. Then, we covered different usages of the Fluent API on the terminologies we saw in Chapter 3, Relationships – Terminology and Conventions. Finally, we have seen the behavior of Delete operations on dependent entities if a principal entity gets deleted. These sections dealt with relationships, but never looked at validations, except where required. Let’s explore them in the next chapter.
