+   6. Open the src/app/app-routing.module.ts file and import the guard:

```
import { AuthGuard } from 'src/app/core';
```

Next, update the feed route in the routes array, as follows:

```
{
  path: 'feed',
  canActivate: [AuthGuard],
  loadChildren: () =>
    import('./feed/feed.module').then(m =>
      m.FeedModule)
}
```

To apply the auth guard we just created, we use the route's canActivate option. This protects the feed module's child routes from unauthenticated users.

We've added auth to our app and secured the feed routes from public access. Users of our application can now create an account, sign in, and search for other network users using the search input in the header area (actually, this last functionality is not fully implemented yet).

Because we now have two apps, a frontend and backend, we can run the following command from the root of our monorepo project to start both:

```
lerna run start --stream
```

This will run the start script of the client and server packages.

# Sending the JWT

If you try to search for users at this point, you will receive an error stating that the user is not authenticated. This is due to the fact that the access token must be sent along with the request.

Here's how you can do this:

+   1. Open the src/app/graphql.module.ts file and start by adding the following highlighted imports:

```
import { ApolloClientOptions, from, InMemoryCache }
  from '@apollo/client/core';
import { HttpLink } from 'apollo-angular/http';
import { setContext } from
```