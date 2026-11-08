We used the mutation operation type since we need to call a mutation on the GraphQL server. We named it register and provided the necessary arguments using GraphQL variables (since we need to dynamically pass the full name, username, email, and password of the user to register an account for them).

In the body of our mutation, we simply asked for the register field, which we've previously added to the Mutation type in our GraphQL server schema, and because the corresponding resolver will return an object containing the authentication token and the user saved in the database, we specified a selection set to request the required fields.

Check out https://graphql.org/learn/queries/ for more information about operation types.

+   3. Add and export the login mutation for authenticating users:

```
```ts
export const LOGIN_MUTATION = gql'
mutation signIn($email: String!, $password: String!){
  signIn(email:$email, password:$password){
    token, user { id fullName bio email username image
      coverImage postsCount createdAt }
  }
}
';
```
```

Again, we used the gql template literal to build the GraphQL document that will be sent to the server to authenticate users. We used the mutation type to instruct the server to call the signIn field on the top-level Mutation type and we used variables to pass dynamic arguments, such as the name and password, which are required to authenticate users.

Since the signIn mutation field refers to an object with a token string and a user object, we use a selection set to ask for all the required information on the client side, including a JWT and the authenticated user.

+   4. Create a shared/constants/user.ts file and import the gql tag, as follows:

```
```ts
import { gql } from 'apollo-angular';
```
```