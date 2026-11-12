Asserting that the `Dashboard.initialise` and `Engine.start` methods have been called, as well as the value returned by the `Car.isRunning` is as expected, is also a little different from when calling the hand written mock class methods:

```
// Then
System.assertEquals(true, car.isRunning());
((Dashboard) mocks.verify(mockDashboard, 1)).initialise();
((Engine) mocks.verify(mockEngine, 1)).start();
}
```

Due to the explicit casting requirements of Apex, the last two lines do not quite match what you would see in an equivalent Java Mockito example. This can make the syntax harder to read. In Java Mockito it would look like:

```
mocks.verify(mockDashboard, 1).initialise();
mocks.verify(mockEngine, 1).start();
```

The assertion code is a mixture of traditional `System.assert` and calls to the `fflib_ApexMocks.verify` method, followed by a call to the methods being mocked. Once again this compiler-checked reference is preferable to late bound string references.

The `verify` method takes an instance of the mocked object and number. This number allows you to assert that the method was called once and only once. Once the `verify` method exits and the mocked method is called, the expected result passed to the `verify` method is checked. If it is not met, either because the method was not called, or it was called more than once, the framework actually has the mocked method use `System.assert` to halt the test with an exception message describing why.

Another example is the `whenUpdateRPMsCalledMessageIsDisplayed` test method in the `DashboardTestApexMocksDemo` class. This utilizes a more advanced form of asserting that the desired mocked method was called. This approach not only validates that it was called but with specific parameter values:

```
@IsTest
private static void whenUpdateRPMsCalledMessageIsDisplayed() {
    fflib_ApexMocks mocks = new fflib_ApexMocks();

    // Given
    Display mockDisplay = (Display) mocks.factory(Display.class);

    // When
    Dashboard dashboard = new Dashboard(mockDisplay);
```