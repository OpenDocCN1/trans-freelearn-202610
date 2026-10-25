```
```csharp
// Code removed for brevity
}
```
```

The preceding implementation would trigger a post filter based on the id passed on to the query object, thus wrapping the core implementation to the query type, satisfying the query object pattern:

+   - Finally, do not forget to support the constructor execution in the dependency injection in `Startup.cs` inside the `ConfigureServices` method:
+   
+     ```csharp
+     services.AddScoped<IPostDetailQueryHandler, PostDetailQueryHandler>();
+     ```

We have seen how to create query objects and handlers, and how to consume them in controllers. In *Chapter 9, Fail Safe Mechanism – Transactions*, we'll see how to leverage or improvise the repositories by consuming query objects.

# Improving repositories with the query object pattern

We need a provision to explain the necessity of a query object pattern, so let's create such a requirement and then we will look into query objects. We will create a repository for blogs in our system, we will see the advantages and disadvantages it brings to the application, and finally, we will see what are query objects and how they help us in improvising repositories.

# Introduction to repositories

We have been performing CRUD operations in the controller directly with the data context; we could incorporate a repository in our application first, and later, query objects could be implemented in the repository.

Let us start with creating the interface required for the repository. It includes basic retrieval, business-specific retrievals, and the remaining CRUD operations. The following code contains the methods required for the `IPostRepository`:

```
```csharp
public interface IPostRepository
{
    IEnumerable<Post> GetAllPosts();
```
```