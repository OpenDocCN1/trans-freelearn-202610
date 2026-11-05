# AOP is part of the Spring triangle

The following diagram — known as the Spring triangle — encompasses the key principles used by Spring Python:

![](bbox=[0.32, 0.22, 0.67, 0.38])

Using Spring Python's IoC container we have been able to take a simple WikiService object and layer on a CachingService through AOP. By using Dependency Injection, we have kept WikiService clear of any Spring Python dependencies. To read the details of this, we just look up the blue prints of our IoC container in WikiProductionAppConfig. Spring also utilized Portable Service Abstractions, such as DatabaseTemplate, to reduce the need to work with low level APIs. Later on in this book, we will revisit the Spring triangle to discuss this in more detail.

# Testing our aspects

If you're a professional software developer, you'll be feeling a little nervous at this point. We've written quite a bit of code, in a number of aspects, and things have 'just worked'. We all know this is rarely the case.

Aspects are just like any other piece of code, they need to be tested. To mitigate risks, and to keep to best practices, we should write automated tests for our aspects. There are different types of tests to pursue. For our caching example, it would be useful to isolate the caching functionality to make sure it meets our requirements; commonly referred to as unit testing. Also, ensuring that the right advice is being applied to the right functions is critical to confirming that AOP is working and this can be exercised using an integration test.