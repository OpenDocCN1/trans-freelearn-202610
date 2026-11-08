We'll use the apollo-angular/testing module, which includes ApolloTestingModule and ApolloTestingController, to make it simple to test code that calls Apollo Client APIs by mocking calls to the GraphQL endpoint.

2. Begin by including the following imports:

```
import {
    ApolloTestingController,
    ApolloTestingModule
} from 'apollo-angular/testing';
import {
    AuthResponse,
    RegisterResponse,
    REGISTER_MUTATION,
    User
} from 'src/app/shared';
```

3. Let's now extend the describe() block by defining a testing controller, as follows:

```
let controller: ApolloTestingController;
```

Then, create a fake user object, as follows:

```
const fakeUser: User = {
    id: 'id#1',
    fullName: 'A B',
    username: 'a.b',
    email: 'a.b@email.com'
} as User;
```

Create a fake token, as follows:

```
const fakeToken: string = '<ADD_A_TOKEN_HERE>';
```

You can get a valid token from your server using Apollo Studio, or use an online service such as https://token.dev/.

4. Then, using the fake user and token, create an auth response, as follows:

```
const authResponse: AuthResponse = {
    token: fakeToken,
    user: fakeUser
};
```