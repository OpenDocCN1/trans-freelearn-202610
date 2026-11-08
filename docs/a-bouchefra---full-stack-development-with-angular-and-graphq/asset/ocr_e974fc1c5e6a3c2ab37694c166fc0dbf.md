# Improving the authentication system

Before we can proceed with building the profile component, we must address a token expiration issue. When a token expires, users must return to the login page and sign in again to acquire a new token. Even after receiving the token and being referred to the user's profile page, the profile's posts are not retrieved, and users see a message indicating that they are not authenticated.

This is because, after re-authentication, the Apollo context is still utilizing the previous— expired—token, which we simply need to replace, as follows:

+   1. Import the following symbols into the core/services/auth/auth.service.ts file:
+   2. Inject HttpLink through the constructor, like this:
+   3. Locate the login() function and insert the following code just after the updateAuthState() call, within the next handler of the object provided to the tap() operator:

```
```ts
import { createApollo } from 'src/app/graphql.module';
import { HttpLink } from 'apollo-angular/http';
```
```

The authentication token is obtained from local storage and supplied to the Apollo context in the createApollo() method.

Apollo Client uses HttpLink to send GraphQL queries to the server. We are importing it since the createApollo() method requires it.

```
```ts
export class AuthService {
  constructor(
    private httpLink: HttpLink) { /* ... */ }
```
```

We inject HttpLink since it is required by the createApollo() method we'll be calling in the next step. See https://apollo-angular.com/docs/data/network/#http-link for more on this.

```
```ts
const acOpts = createApollo(this.httpLink);
this.apollo.client.setLink(acOpts.link!);
```
```

We call the GraphQL module's createApollo() function, which causes the context link to obtain a new token from localStorage, and then we call the setLink() method to set the returned link chain and pass it to Apollo Client.