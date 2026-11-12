
MockEngine mockEngine = new MockEngine();

// When
Car car = new Car(
    mockEngine.createStub(),
    mockDashboard.createStub());
car.start();

// Then
System.assert(car.isRunning());
System.assert(mockDashboard.initialiseCalled);
System.assert(mockEngine.startCalled);
}

The sample unit test code and mocks used for this unit test are implemented in the CarTestApexStubAPIDemo class.

The main difference from the Apex interface approach is the creation of the stub instances of the classes being mocked using the applicable createStub methods.

The following code shows the handleMethodCall method in the MockEngine class used in the preceding unit test. It illustrates how a mock response can be returned for the Engine.isRunning method:

```
public Object handleMethodCall(
    Object stubbedObject,
    String stubbedMethodName,
    Type returnType,
    List<Type> listOfParamTypes,
    List<String> listOfParamNames,
    List<Object> listOfArgs) {
    // Record method call
    if(stubbedMethodName == 'isRunning') {
        isRunningCalled = true;
        return true;
    } else if(stubbedMethodName == 'start') {
        startCalled = true;
    }
    return null;
}
```

The following code shows the handleMethodCall method from the MockDisplay class (see the DashboardTestApexStubAPIDemo class for full code) and the use of the listOfArgs parameter:

```
public Object handleMethodCall(
    Object stubbedObject,
    String stubbedMethodName,
    Type returnType,
    List<Type> listOfParamTypes,
    List<String> listOfParamNames,
    List<Object> listOfArgs) {

    // Record method call and parameter values
    if(stubbedMethodName == 'showMessage') {
        showMessageCalled = true;
        showMessageCalledWithMessage = (String) listOfArgs[2];
    }
    return null;
}
```

The preceding mock handler code monitors the showMessage method being called by the Dashboard updateRPMs method. It then stores parameter values for later assertion, in addition to the fact the method itself was called.

The following unit test code leverages this to test that the correct information is passed to the display when the updateRPMs method is called:

```
@IsTest
private static void whenUpdateRPMsCalledMessageIsDisplayed() {

    // Given
    MockDisplay mockDisplay = new MockDisplay();

    // When
    Dashboard dashboard = new Dashboard(mockDisplay.createStub());
    dashboard.updateRPMs(5000);

    // Then
    System.assert(mockDisplay.showMessageCalled);
    System.assertEquals('RPM:5000',
        mockDisplay.showMessageCalledWithMessage);
}
```

## Considerations when using the Apex Stub API

The following are some general notes and considerations for the Apex Stub API:

- The Apex `new` operator still creates the real type when used in your application code. Therefore, you still need to leverage a Dependency Injection convention, such as one of them described in this chapter, to inject the alternative implementation of any dependent class you wish to mock in your unit tests.

- Explicitly defined interfaces between the dependency boundaries of your classes are not strictly required, but might be a useful means of further enforcing SOC boundaries or for further configuration of your application.

- Not all methods and classes can be stubbed using the `Test.createStub` method. Examples include inner classes and get/set accesses. Also methods returning `Iterator`. For a full list consult the *Apex Developers Guide*.

- Static methods are also not able to be stubbed, this is by design, since these methods are not open to dependency injection. Although the Service layer design convention utilizes static methods, the actual implementation of those methods goes through the service factory to access the actual service implementation via instance methods. This can be seen in the `RaceService` included in the sample code.

- Other restrictions over what can be mocked, such as private methods and constructors, might seem like an omission. However, in principle you should be testing the public behavior of your classes in your unit test only anyway. Once you mock an object method the constructor used to instantiate the object is not significant, as your mocking code determines what each method returns.

- Keep in mind that, changes to method signatures can break the late bound references to parameters and return types made in your mock implementations. In this respect, explicit interface-based mock implementations have an advantage as a compilation error will occur if you make breaking changes. Mocking frameworks can help mitigate this risk as we will discuss in the next section.

## Using Apex Stub API with Mocking Frameworks

Implementing mock classes for small code bases or as an aid to get a better understanding of the concept of unit testing, as we have done in this chapter is fine. However, it can quickly become onerous and a maintenance overhead for larger code bases. The Apex Stub API was designed to handle any class shape for a reason.

The stubbedObject parameter can be used to determine from what type the method being called belongs. This allows for the creation of a single System.StubProvider implementation that can implement more advanced services that can be leveraged by unit tests without the need to write their own mock classes over and over. Playback of preconfigured responses to mocked methods and recording of each method call it receives for later assertion is an example of this.

The concept of a mocking framework is not new. There are many well established frameworks such as the popular Mockito framework for Java or Jasmine for JavaScript/NodeJS applications. However, mocking is a relatively new concept and capability in Apex, so the space is still emerging for Apex-based mocking frameworks.

FinancialForce.com has published an open source mocking framework known as ApexMocks. While it predates the arrival of the Apex Stub API it has been updated to work with this API. The advantage is that the code generator used to create the mock classes for use with Apex Mocks is no longer required; nor is the explicit use of Apex interfaces.

In the remainder of this chapter we will review the benefits of a mocking framework through the use of the ApexMocks framework (now included in the sample code of this chapter). We apply it first to our sample Car class model and then to unit test scenarios from the FormulaForce application. A full walkthrough is worthy of an entire book in my view. You can read more about ApexMocks through the various blog and documentation links on its README file, https://github.com/financialforcedev/fflib-apex-mocks/blob/master/README.md.

> Credit should be given to Paul Hardaker for the original conception and creation of ApexMocks. As an experienced Java developer coming to the platform and used to using Mockito he was quickly driven to find out whether a similar unit testing facility could be created on the platform. Thus, ApexMocks inherits much of its API design from Mockito. Since then the framework has gone from strength to strength and received several other submissions and support from the community including presentations at Dreamforce. Much of what I know about the area of unit testing is also due to Paul's tutoring. Thank you!

## Understanding how ApexMocks works

The whenCarStartCalledDashboardAndEngineInitialised unit test method in the CarTestApexMocksDemo test class uses the ApexMocks library. The structure of this unit test is similar to those we have looked at so far in this chapter. It creates the mocks, runs the method to test, and asserts the results.

The big difference is that you do not need to write any code for classes you wish to mock. Instead you use the ApexMocks framework to configure your mock responses and to assert which methods where recorded. Then through its integration with the Apex Stub API it creates a stub that wraps itself around any class you wish to mock.

The main class within the ApexMocks framework is fflib_ApexMocks. This class implements the System.StubProvider interface for you. It automatically echoes any mock responses to mocked methods called and records that they got called and with what parameters. This applies to all public methods on the classes you ask it to mock for you.

Much of the skill in using ApexMocks is knowing how to configure the mocked responses it gives when mocked methods are called and how to write matching logic to assert not only when a method was called, but how many times and with what parameters. This later facility is incredibly powerful and opens up some sophisticated options for verifying the behavior of your code.

The following code initializes the ApexMocks framework (every test method needs this):

```
@IsTest
private static void
whenCarStartCalledDashboardAndEngineInitialised() {

fflib_ApexMocks mocks = new fflib_ApexMocks();
```
