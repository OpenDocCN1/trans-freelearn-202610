# Configuring non-timestamp tokens through
Fluent API

The Fluent API configuration is made possible because Microsoft has exposed a series of extension methods, which allows us to execute appropriate functionality against the property of the model that supports it. In our blogging system, we will configure the Url of the Post entity as the concurrency token by enabling the property using `IsConcurrencyToken()` as shown in the following code:

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // Code removed for brevity
    modelBuilder.Entity<Post>()
        .ToTable("Post")
        .Property(x=>x.Url)
        .IsConcurrencyToken();
    // Code removed for brevity
}
```

The preceding configuration will let EF Core consider the Url column as the concurrency token, and any further update to this column will restrict users from performing parallel updates, as it did in the data annotation configuration.

We have exhaustively seen how this concurrency token works, so let's move on to the next section that explores timestamp-based concurrency tokens.

# Timestamp-based concurrency tokens

We have already explored `Timestamp`-based concurrency tokens in the *Introducing concurrency tokens* section. We can jump directly into the configuration, consider the same scenario and see how the conflicts were handled using the `Timestamp` concurrency token.

As we did with the non-timestamp based concurrency token, timestamp-based concurrency tokens could also be configured in the following ways:

+   - Data annotations
+   - Fluent API

We will see how the configurations were performed using both the approaches in our blogging system, and later we will see them in action.