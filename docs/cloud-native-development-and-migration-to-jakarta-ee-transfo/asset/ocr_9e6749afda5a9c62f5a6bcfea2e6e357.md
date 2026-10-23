When you added the previous class from the example, you have a working (green) test. The next step would be to make the test fail (red) again. You could do that by adding a method call that you want the `RoutingHelper` class to have. The next example shows you how this is done by adding a method call inside the testing method:

```
```java
@Test
void goesToNetherlands_success() {
    RoutingHelper routingHelper = new RoutingHelper();
    routingHelper.goesToNetherlands();

}
```
```

When you add the method call, the test fails again because the method does not exist yet. Creating this method would be the next step. What we just did was go through the red-green cycle of TDD. We started with a failing test, fixed it, made the test pass, and made it fail again. This is the basis of TDD, and when you develop new features, you want to start with a failing test and try to make it pass. This would be the start of a new red-green cycle. TDD can be applied to any kind of code or test you are writing; this could be a unit test or an integration test. When you use TDD for integration tests, really try to keep the test execution of the test to be fast, because you are going to execute the test several times.

# How to create unit tests

Testing in Java is often done with unit tests. When people talk about test coverage or if you have tests for your code, they most often mean unit tests. Unit testing is a method to test the smallest unit of code. With Java, the smallest unit of code is often a single method. With unit testing, we just test one single method at a time. Sounds simple enough, right? Testing a single method is easy if it doesn’t depend on another class, package, database, or even a framework or entire application. That’s when unit testing often gets difficult and testing code becomes a huge mess, and nobody wants to touch it anymore. We want to prevent that, so we start at the basics and build our testing strategy up from there, starting with a single unit test.

# Tools and libraries required for unit testing

The unit tests that you are going to write in this chapter depend on two very popular testing libraries, JUnit and Mockito. For the unit tests themselves, we are going to use JUnit, and we’ll use Mockito to mock any dependencies the code has. These two libraries are very helpful and are almost essential for every project.

JUnit helps you to write the actual unit test. With its built-in annotations, it enables you to mark methods and connect them to different parts of the testing life cycle. A testing life cycle helps you to prepare object instances for your tests. The other essential library is Mockito. Mockito is a library that helps you to create mocks of existing classes in a straightforward way. As you will see later in the examples, mocks are created so that we don’t have to interact with real instances of classes and databases.