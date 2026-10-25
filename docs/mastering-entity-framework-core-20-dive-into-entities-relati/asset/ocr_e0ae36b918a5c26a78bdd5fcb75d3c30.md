![](bbox=[0.35, 0.11, 0.65, 0.48])

We have identified the provision, the UserId field that will allow the entity to configure the relationship. Let us build the relationship using the Fluent API in the next section.

# Building one-to-one relationships using the Fluent API

We could configure the relationship using the Fluent API with the HasOne, WithOne, and HasForeignKey methods. The HasForeignKey method needs to be generic in a one-to-one relationship (a one-to-many relationship doesn't require this) since we need to explicitly mark the dependent type. In our case, we have specified the Address entity for the foreign key:

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>().ToTable("Blog");
    modelBuilder.Entity<Post>().ToTable("Post");
    modelBuilder.Entity<User>()
        .ToTable("User")
        .HasOne(x=>x.Address)
        .WithOne(x=>x.User)
        .HasForeignKey<Address>(x=>x.UserId);
    modelBuilder.Entity<Address>().ToTable("Address");
}
```