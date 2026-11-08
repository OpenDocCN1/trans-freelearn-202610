5. Finally, as shown in the following code, replace the code line where we create Apollo Server by providing the context with the schema:

```
```ts
const server: ApolloServer = new ApolloServer({ schema,
  context });
```
```

To work with the database, we can now use this context object in the resolvers together with the provided repositories. We could alternatively call the getRepository() function for each entity in the resolvers, but this is the preferred solution for a variety of reasons, including easy testability. You can view the changes of the previous steps from the commit at https://git.io/JzdQM.

Following that, we'll implement the resolvers for our GraphQL schema:

1. Open the server/src/graphql/resolvers.ts file and update the imports, as follows:

```
```ts
import { Context } from '...';
import {
  Resolvers,
  User,
  Post,
  Comment,
  Like,
  Notification } from '@ngsocial/graphql';
import { ApolloError } from 'apollo-server-errors';
```
```

2. Update the type of the resolvers object to Resolvers instead of IResolvers:

```
```ts
const resolvers: Resolvers = {
  Query: {
    message: () => 'It works!'
  }
};
export default resolvers;
```
```

All of our schema resolvers and their arguments will be type safe as a result of this.