In the context function, we call the get() method of the request object to retrieve the value of the authorization header, which should contain the "JWT: " string, plus the token. Next, we split the value using space and pass the second part, which contains the actual JWT, to our getAuthUser() function to decode the token and get the user's information, that is, the user's ID and email. Then, we create a ctx object (typed with Context) with the authUser and the orm object that contains the TypeORM repositories.

Please note that we've moved the creation of the context object from outside to inside the context method of Apollo Server, so you must remove the previous code for creating the instance that exists right above the call for bootstrapping Apollo Server. Check this GitHub commit for the updates we've made: https://git.io/JgCgO.

After implementing this, if a JWT is sent back with any request to our GraphQL API, we'll be able to access the logged-in user's ID and email from the context parameter of any resolver function.

+   5. After that, you can go to the server/src/graphql/resolvers.ts file and modify every resolver that you need to protect by adding the following:

```
getUser: async (_, args, { orm, authUser }) => {
  if (!authUser) throw new Error('Not authenticated!');
  // [...] 
}
```

We destructure the context parameter passed to the function to get the authUser object that contains information about the authenticated user. Then, we check whether that object is not null; otherwise, we throw an error with the Not authenticated! message.

This will ensure that every resolver will be protected against unauthenticated users.

A better solution is using directives, so let's see how to implement this second approach. We'll implement an @auth directive that can be applied to the query and mutation fields in the schema to check whether the user is authenticated before they can complete the operation:

+   1. Open the server/src/graphql/schema.ts file and import the following symbols:

```
import { defaultFieldResolver } from 'graphql';
import { getDirective, MapperKind, mapSchema } from '@
```