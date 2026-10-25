```
```csharp
optionsBuilder.UseSqlServer(@"Server=
    (localdb)\\mssqllocaldb;Database=MasteringEFCoreDbFirst;
    Trusted_Connection=True;");
}
protected override void OnModelCreating(ModelBuilder
modelBuilder)
{
    modelBuilder.Entity<Post>(entity =>
    {
        entity.HasIndex(e => e.BlogId).HasName("IX_Post_BlogId");
        entity.Property(e => e.Title).IsRequired();
        entity.HasOne(d => d.Blog).WithMany(p => p.Post)
            .HasForeignKey(d => d.BlogId);
    });
}
```
```

In case you have noticed the warning, we need to remove the section using dependency injection, which will be performed in the *Registering Context in Services (.NET Core DI)* section.

# Working with the Blog entity

We have discussed in detail the `Blog` model in Chapter 1, *Kickstart - Introduction to Entity Framework Core*, but still, the following highlighted part looks pretty new to us:

+   - The `Blog()` constructor initializes the `Post` property, which ensures that the collection has a concrete HashSet list created and ready to accept any new items
+   - The `Post` property has a `virtual` keyword, which instructs EF to lazy load the navigational property `Post`:
+   
+     ```csharp
+     public partial class Blog
+     {
+         public Blog()
+         {
+             Post = new HashSet<Post>();
+         }
+         public int Id { get; set; }
+         public string Url { get; set; }
+         public virtual ICollection<Post> Post { get; set; }
+     }
+     ```