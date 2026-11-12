Unit Testing requires an understanding of some other patterns such as Dependency Injection and mocking. Dependency Injection can be implemented in a number of ways, using constructors, factories and setter methods. The approach you use of Dependency Injection will depend on your personal preference and/or the preference of the libraries you're using. Multiple approaches to dependency injection can be mixed if needed.

One approach to implementing mocking is to use Apex interfaces. Utilizing multiple implementations of interfaces that represent the boundaries between the dependencies of your code base allows you to implement the real code and test code separately.

Mocking is most effective when the code being tested is not aware which implementation is being used. The Apex Stub API allows for the creation of dynamic mocks for classes without using Apex interfaces. This is good for existing code bases or scenarios where using Apex interfaces don't make sense or adds overhead.

ApexMocks is a mocking framework library that reduces the work involved in building mock objects, removing the need to physically code them. Based on Mockito it offers advanced facilities to configure mocked method responses or throw exceptions to help test error handling. This framework also records the mock methods invoked and provides a sophisticated matching syntax to assert what methods were called.

The Apex Commons open source library used in this book utilizes the factory pattern to provide built in methods for injecting mock implementations of services, unit of work, domain, and selector layers.

In the end unit testing and its associated prerequisites make it a difficult concept to master and requires a lot of work up front. Start with a few simple examples at first and continue leveraging your existing Apex testing approaches until you're ready to expand. Once you have mastered the API's and concepts, it becomes second nature and can help open up access to other development coding practices such as test-driven development.

In the final chapter of this book, we will take a step back and look at how to scale your development around multiple developers and setup continuous and automated execution of your tests whenever changes are made.