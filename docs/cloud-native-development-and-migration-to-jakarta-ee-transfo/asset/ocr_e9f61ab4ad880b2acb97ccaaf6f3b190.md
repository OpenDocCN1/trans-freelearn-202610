# Creating a unit test

In this section, we will show you how to create a unit test. The Cargo Tracker application is intricate, meaning that you will need some time to fully understand what is going on inside the code. To speed up this process, we are first going to create a method that we can test. This way, you don’t have to know any details of the Cargo Tracker application and are able to focus on creating the unit test.

The method we want to use to teach you unit testing needs a class to hold it. Create a class named `RoutingHelper` inside the `org/eclipse/cargotracker/infrastructure/routing` package. Make sure the content of the `RoutingHelper` class looks like this:

```
```java
package org.eclipse.cargotracker.infrastructure.routing;

public class RoutingHelper {
}
```
```

We can’t create unit tests for this because this class does not have any methods inside of it. Let us create one now:

```
```java
boolean goesToNetherlands(RouteSpecification routeSpecification) {
    Location destination = routeSpecification.getDestination();
    return "Netherlands".equals(destination.getName());
}
```
```

This is just a simple method intended to teach you the basics of unit testing. The method in the previous example does only one thing—it checks if the destination of a `RouteSpecification` instance is equal to Netherlands. If the location equals the Netherlands, it returns true; if it is not the same, it will return `false`.

Let’s create a test class for the `RoutingHelper` class. Every class resides inside a directory structure. For the `RoutingHelper` class, this is `src/main/java/org/eclipse/cargotracker/infrastructure/routing/RoutingHelper.java`. It is common for tests to reside inside the test package but with the same directory structure.

The next step is to create a test class that will be used to hold the unit tests for this class. Create a file named `RoutingHelperTest` inside this directory structure: `src/test/java/org/eclipse/cargotracker/infrastructure/routing/RoutingHelperTest.java`. Notice the second directory inside both paths. The production code resides inside the main package, and the test code lives in the test package. This makes it easy to tell them apart, and you won’t accidentally call the testing code from your production code.

Make sure the content of the `RoutingHelperTest` class looks like the following:

```
```java
package org.eclipse.cargotracker.infrastructure.routing;
import static org.junit.jupiter.api.Assertions.*;
class RoutingHelperTest {
}
```
```