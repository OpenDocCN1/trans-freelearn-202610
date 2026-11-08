# Implementing JWT authentication with subscriptions

This section explains how to implement authentication over WebSocket. Refer to the documentation at https://www.apollographql.com/docs/apollo-server/data/subscriptions/#operation-context for more information.

Until now, our subscriptions have not been secure, so we need to prevent subscriptions from users that are not authenticated. This means that we only allow WebSocket connections if the user is authenticated.

We can secure our subscriptions in the same way we did with queries and mutations. We simply need to pass a context parameter to our subscriptions each time a user connects over WebSocket!

When we create an instance of SubscriptionServer, we can use an onConnect function that gets executed before every WebSocket connection. This function accepts an object of the ConnectionParams type as one of its arguments. If it returns an object, it gets passed to the resolvers as context.

Using ConnectionParams, we can get the JWT sent from the client. Inside onConnect, we will extract the authenticated user from the JWT and add it to the context:

+   1. Open the server/src/index.ts file and add the following imports:

```
import { ApolloServer, AuthenticationError } from 'apollo-server-express';
import { SubscriptionServer, ConnectionParams } from 'subscriptions-transport-ws';
```

We add AuthenticationError to the existing import declaration from apollo-server-express and ConnectionParams from the existing import declaration from subscriptions-transport-ws.

+   2. Next, add the onConnect function to the object passed to the SubscriptionServer.create() method and extract the token that can be retrieved from the parameter passed to onConnect:

```
const subscriptionServer = SubscriptionServer.create(
{
  schema, execute, subscribe, onConnect:
    (connectionParams: ConnectionParams) => {
```