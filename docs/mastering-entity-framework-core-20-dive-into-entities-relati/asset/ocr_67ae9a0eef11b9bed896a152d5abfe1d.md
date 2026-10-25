```
new GetAllPostsQuery(true));
}

// Code removed for brevity
}
```

We can see the controller started using the new repository through dependency injection and corresponding the Get action being called using the appropriate query object. In the preceding scenario, we have been using the GetAllPostsQuery object that was passed on to the GetAsync method of the new repository. In the next section, let's explore the implementation required to return an individual post object from the repository.

# Single query object support in the repository

We have seen how a list of objects was returned from the repository in a generic fashion. Let's have a provision to return a single object from the repository:

```
public interface IPostRepositoryWithQueries
{
    // Code removed for brevity
    Post GetSingle<T>(T query) where T : class;
    Task<Post> GetSingleAsync<T>(T query) where T : class;
}
```

The preceding code adds synchronous and asynchronous methods to return a single Post object from the repository, still retaining the generic implementation since it could be used by multiple query objects that might return individual Post objects:

```
public class GetPostByIdQuery
{
    public GetPostByIdQuery(int? id, bool includeData)
    {
        this.Id = id;
        this.IncludeData = includeData;
    }

    public int? Id { get; set; }
    public bool IncludeData { get; set; }
}
```