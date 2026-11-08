# Protecting GraphQL queries and mutations

After implementing the GraphQL mutations for registering and signing users, we need to protect our other queries and mutations using the JWT token that we obtain when users are registered or signed in.

Following user authentication, the server will transmit a JWT token to the client, which should store it locally and send it back with each request using an HTTP header (the authorization header).

We can use the context option of Apollo Server to access the authorization header from the request object and pass this data to the resolvers. The context is available in each resolver.

The following are the steps required to protect our GraphQL API:

+   1. Open the server/src/index.ts file and add the following code:
+   2. Add the following method, which will be used to verify and retrieve the user's information from the JWT using the verify() method, from jsonwebtoken, and the secret:

```
import jwt from 'jsonwebtoken'
import dotenv from 'dotenv';
dotenv.config();
const { JWT_SECRET } = process.env;
```

We import the necessary symbols and retrieve the JWT_SECRET environment variable from the process.env object (loaded during runtime from the .env file using dotenv). This will enable us to get our JWT secret from the .env file. Check out https://nodejs.dev/learn/how-to-read-environment-variables-from-nodejs for more details about how to read environment variables in Node.js.

```
const getAuthUser = (token: string) => {
  try {
    if (token) {
      return jwt.verify(token, JWT_SECRET as string);
    }
    return null;
  } catch (error) {
    return null;
  }
```