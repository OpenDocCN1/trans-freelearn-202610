The following npm command will install an npm library called uuid to help us generate the UUID we need:

```
npm i uuid
```

We also need the types for the uuid library we installed:

```
npm i -D @types/uuid
```

The preceding npm command will install the uuid TypeScript types.

Now, we need a file for the fixtures in our Cypress test. A fixture is the fixed state of an object or array that's used as a baseline for running tests:

+   1. Go to the fixtures folder inside the cypress directory in your application. Create two JSON files and name them anti-heroes.json and user.json.
+   2. Copy the content of the file from https://github.com/PacktPublishing/Spring-Boot-and-Angular/blob/main/Chapter-15/superheroes/cypress/fixtures/anti-heroes.json and paste it into the anti-heroes.json file.
+   3. Next, copy the content of the file from https://github.com/PacktPublishing/Spring-Boot-and-Angular/blob/main/Chapter-15/superheroes/cypress/fixtures/user.json and paste it into the user.json file.

The preceding JSON objects are objects that we are going to use in our fixtures. We will use these as response bodies to send mock responses.

Now, let's update the commands.ts file in the support folder inside the cypress directory. Use the following code:

```
// @ts-check
///<reference path="../global.d.ts" />
/// <reference types="cypress"/>
import { v4 as uuidv4 } from "uuid";
Cypress.Commands.add("getCommand", (url: string, responseBody: Array<any>) => {
    cy.intercept("GET", url, {
        statusCode: 200,
        body: responseBody,
    });
});
```