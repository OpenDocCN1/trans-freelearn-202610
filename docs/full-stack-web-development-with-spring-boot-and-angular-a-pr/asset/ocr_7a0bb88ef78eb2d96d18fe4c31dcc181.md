cy is an object where you can chain different types of commands. We are using the visit command, which allows us to write the URL we will use to navigate to localhost:4200 once we run the test. cy.url asserts that the URL can be found on the login subpage. We are also testing mat-card-title through the data-cy= "auth-title" attribute, which has the word Login in this test.

As you can see, the first test is easy to write. The setup for writing the test is also easy. But before we run our first test, let's create a user for our test:

![](bbox=[0.28, 0.25, 0.73, 0.54])

Figure 15.5 – A user for E2E testing (Angular app login page)

In Figure 15.5, we are creating a user for our E2E testing. The user@cypress.com username is just a made-up email address you are not required to use. You can use whatever email address you want. We will use the user@cypress.com user to log into our app and use the web app like a real user would use our application.

Now, go to your Terminal and run the npm run test command to run Angular's E2E tests. Go to the Specs section of the Cypress dashboard to find the list of E2E specs: