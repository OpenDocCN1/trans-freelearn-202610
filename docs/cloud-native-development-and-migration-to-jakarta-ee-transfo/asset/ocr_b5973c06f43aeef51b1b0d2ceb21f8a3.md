Here, we create two locations, a route specification, and a routing helper in the given part and use both in the remaining part of the unit test.

When you add this code to the application tracker project, your first unit test is done. All you need to do now is run the unit test.

# How to create integration tests

In this section, we are going to show you how to create integration tests. The applications we built today are not siloed anymore. Instead, the applications we develop in our day-to-day lives have lots of dependencies on internal and or even external systems. These dependencies all have their own versions, interfaces, and so on that need to keep working when we replace just a small or even a big piece of our application. It is important to know if your application still works after, for example, upgrading a software dependency, server, or operating system.

In the next sections, we are going to walk you through what integration tests are and how you can use them to test your application.

# What are integration tests?

To verify if your system still works, you need tests! With unit tests, we only test a small unit of code, not packages, modules, or even entire applications. They are not built with that in mind. To verify that bigger parts or entire applications work, you need integration tests.

Integration tests, as the name suggests, are about testing the integration of your application with that of others to see if the application you are testing behaves as expected when interacting with a different application. You can also use them to test how modules inside your application integrate with each other because ideally, you want to run your integration tests against a real running application. In the next session, we will show you how to run tests against a real, running Cargo Tracker application. To achieve this, we will use Testcontainers.

# What is Testcontainers?

Testcontainers is a piece of software developed by AtomicJar. It is an open source framework that provides an abstraction around spinning up and breaking down Docker containers for testing purposes. It gives you an easy and lightweight interface through which you can manage your containers. There is also a library that makes integration with JUnit 5 very simple. With Testcontainers, you can do the following:

+   - More easily test our application more realistically.
+   - Test how your systems integrate with real dependencies instead of having to use mocks or stubs. For example, you can spin up a real database.
+   - Make sure your test environment is portable. Every user will spin up the same Docker containers, making it easy for everyone to start using and extending the integration tests.