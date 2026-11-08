We utilize our custom Context type for the context object of the resolvers because it is typed with any. We can also tell GraphQL Codegen to utilize this type for the context. Please refer to https://the-guild.dev/blog/better-type-safety-for-resolvers-with-graphql-codegen for additional details.

Next, inside the Query object, add the resolver functions listed below, one by one, separated by a comma:

+   1. Begin by including the getUser query resolver, which is used to find a user by ID:

```
```ts
getUser: async (_, args, ctx: Context) => {
    const orm = ctx.orm;
    const user = await orm
        .userRepository
        .findOne({
            where:
                { id: args.userId }});
    if (!user) {
        throw new ApolloError(
            "No user found",
            "USER_NOT_FOUND");
    }
    return user as unknown as User;
}
```
```

To find the user with the specified ID, we utilize the findOne() method of the user repository, which is obtained from the orm object passed with the resolver's context. If no user is found, an Apollo error is thrown.

We cast the user object from the User entity to the User type before returning it, and because TypeScript warns that the types do not sufficiently overlap, we first cast the object to the unknown type.

+   2. Implement the getPostsByUserId resolver to retrieve posts based on the user ID:

```
```ts
getPostsByUserId: async (_, args, ctx: Context) => {
    const posts = await ctx.orm.postRepository
        .createQueryBuilder("post")
        .where({ author: { id: args.userId } })
        .leftJoinAndSelect("post.author", "post_author")
```
```