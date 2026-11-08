Then, add and export the user query for getting a user by ID:

```
export const USER_QUERY = gql'
query getUser($userId: ID!){
  getUser(userId:$userId){
    id fullName bio email username image coverImage
    postsCount createdAt }
}
';
```

Here, we use the query operation type and we pass the user ID as an argument to ask the API server to return an existing user with that ID. The Query type on the server contains the getUser field that we are querying and because it returns a user object, we provide a selection set to ask for the fields that we are interested in, on the client side.

5. Add and export the following query to search for users:

```
export const SEARCH_USERS_QUERY = gql'
query searchUsers($searchQuery: String!){
  searchUsers(searchQuery:$searchQuery){
    id fullName bio username image
  }
}
';
```

Here, we use a query operation type to ask for the searchUsers field on the top-level query of the GraphQL schema, which returns an array of users with full names that contain the search term passed as an argument. We then specify a selection set for selecting the user fields that we are interested in, such as the user's ID, full name, bio, username, and image.

6. Finally, create a shared/index.ts file (barrel file) and export the defined constants, as follows:

```
export * from './constants/auth';
export * from './constants/user';
```

Following the definition of the queries and mutations that will be parsed and sent to the server via Apollo Client, we must define the types of responses/results that will be returned from the API server.