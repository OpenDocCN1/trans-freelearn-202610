```
@Injectable({
  providedIn: 'root',
})
export class GetUserGQL extends Query<UserResponse> {
  document = USER_QUERY;
}
```

We import the necessary APIs, such as the Injectable decorator to decorate classes to define Angular services such as, the Mutation and Query services of apollo-angular, the constants that contain the queries and mutations that will be sent to the server, and the response types.

Next, we create our GraphQL services by extending the Mutation or Query services and setting the document property to the appropriate query or mutation.

The authentication service, which encapsulates the user's authentication functionality, will be implemented next.

# Implementing the user authentication service

The authentication service is a part of the app module into which all previously defined Apollo services will be injected.

This service will contain the authentication logic for the user and provide methods for the user's components to register, sign in, log out, check the authentication state, get a specific user by ID, and finally, search for users.

Let's get started with the implementation of the authentication service by taking the following steps:

+   1. Open the core/services/auth/auth.service.ts file and start by adding these imports:

```
import { Apollo } from 'apollo-angular';
import { Observable, BehaviorSubject } from 'rxjs';
import { map, tap } from 'rxjs/operators';
```

Also, add the following imports from the shared module:

```
import {
  User,
  RegisterResponse,
  LoginResponse,
```