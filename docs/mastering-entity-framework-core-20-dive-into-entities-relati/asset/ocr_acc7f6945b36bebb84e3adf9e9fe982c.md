# # Incorporating the query object pattern into repositories

In the *Introduction to query objects* section of this chapter, the query object pattern we had explored need to be incorporated into the repository. Since we know the steps that need to be followed in creating query objects, let's start creating the required types.

# ## List query object support in the repository

We have seen the `GetAllPosts` method, which includes related data for all implementations. Since we are creating a common query, which could be consumed in multiple places, we need to include this as a filter in the `GetAllPostsQuery` object:

```
public class GetAllPostsQuery
{
    public GetAllPostsQuery(bool includeData)
    {
        this.IncludeData = includeData;
    }
    public bool IncludeData { get; set; }
}
```

The `GetAllPostsQuery` consumes only the `IncludeData` filter in the type, which doesn't filter any data from the result set, but only excludes/includes related data. The following code would create a contract to enforce consumers to implement handle methods:

```
public interface IPostQueryHandler<T> where T : class
{
    IEnumerable<Post> Handle(T query);
    Task<IEnumerable<Post>> HandleAsync(T query);
}
```

The generic query handler interface now supports both synchronous and asynchronous operations, which returns a list of Posts. The `GetAllPostsQueryHandler` would perform the concrete implementation which will yield a list of Posts in synchronous and asynchronous manner as displayed in the following code:

```
public class GetAllPostsQueryHandler :
    IPostQueryHandler<GetAllPostsQuery>
{
    private readonly BlogContext _context;
    public GetAllPostsQueryHandler(BlogContext context)
    {
        this._context = context;
```