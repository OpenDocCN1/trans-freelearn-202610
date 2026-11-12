As we can see in the following figure, the test passes and Karma shows a green SUCCESS status:

![](bbox=[0.066, 0.106, 0.915, 0.212])

Passing all the tests in Karma

Now would be a good moment to go back to the test and make it fail. What happens if you call the function on an empty array? What if the array is deeply nested? You can already see where this is going; our aim is to break the code.

> More information about the Karma test runner and its configuration can be found on Github at https://karma-runner.github.io/. More information about the Mocha test framework can be found on the Mocha Website at https://mochajs.org/. If you want to read more about the Chai assertion library, then take a look at http://chaijs.com/.

# Integration testing using Protractor and Selenium

Integration tests require all the components from the system that are under tests to work as if a user would interacting with the component. Therefore, we need to use a different test runner that can start, stop, and connect to browsers, open URLs, deal with asynchronous code, and extract information about the elements.

In this book, we will also use the Integration testing tool from the AngularJS team: Protractor. It is based on Webdriver and Selenium and therefore very powerful when interacting with different browsers and instances.

We can install Protractor by running the following command from the command line:

```
npm install -g protractor
```

Once Protractor is installed, we have to also install Webdriver. One can do this by running the following:

```
webdriver-manager update
```