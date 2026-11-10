There is yet another way of achieving the same behavior. You can go about declaring expected exceptions through JUnit's ExpectedException rule. First, let us give you a crash course in JUnit rules.

In earlier versions of the test framework, it was not possible to modify or augment JUnit's internal knowledge about a given test suite. Using the current version, we can use rules to modify test results, change a test in a certain way before running it, and add information to test results. We will use the @Rule annotation to inform JUnit which rules to apply to the underlying test.

By using ExpectedException in conjunction with @Rule, you can specify in-test expectations regarding the exceptions that should be thrown for a particular test method. It takes more typing than expected, however, as shown here, you have more control over what is expected. For instance, you can even specify the message what the expected exception should contain.

```
```java
@Rule
public ExpectedException thrown = ExpectedException.none();

@Test
public void testDivide() {
    thrown.expect(ArithmeticException.class);
    thrown.expectMessage("/ by zero"); //specify the expected message
    calculator.add(10);
    calculator.divide(0);
}
```
```

As for the second optional parameter of @Test, timeout, it causes a test method to fail if it takes longer than a specified amount of milliseconds to execute. To give you an example, let's say that we implemented testSquareRoot as an infinite loop, which fails after 1 second. Upon running the code below you should get a red bar along with a java.lang.Exception containing the following message: "test timed out after 1000 milliseconds".

```
```java
@Test(timeout=1000)
public void testSquareRoot() {
    while(true);
}
```
```