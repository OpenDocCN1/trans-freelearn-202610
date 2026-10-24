
As this is one of the essential processes, there should be little room for errors in testing, and manual testing is not enough, as this is prone to human errors and has a more significant chance of missing the existing issues in an application. This is where unit testing comes to the rescue – unit testing is automated testing that allows the developer to write tests for a single class or entity.

It is a form of **regression testing** that runs all of the tests to validate whether the code still passes the test cases after several changes or updates have been applied to the application code. Unit tests help maintain the quality of our applications, as they bring the following benefits:

- **Speed:** Unit testing will be less time-consuming compared to manual testing, as this is programmable and will deliver the results in a short period.
- **Cost reduction:** Unit testing is automated, which means fewer testers will be required for testing the application.
- **Fewer errors:** Unit testing will significantly reduce the number of errors made, as testing is not done manually by humans.
- **Programmable:** Unit tests can produce sophisticated tests that detect hidden information in the application.

Unit tests are widely used now in both frontend and backend development, especially in Java, because of their advantages and testing. There are already several testing frameworks available in Java, but we will discuss the first and most commonly used framework, **JUnit**.

## JUnit framework

JUnit is a regression testing framework mainly used for writing tests and assertions for single classes in a Java application; it promotes the idea of *first testing and then coding*, which states that we need to create test data for a piece of code to be tested before implementation. JUnit is also an open source framework, which makes it more reliable.

There is a large community supporting the framework, it uses assertions to test expected results and annotations to identify the methods for testing, and it can be efficiently utilized and integrated with Maven and Gradle projects.

Let’s discuss the features of JUnit that we will use for writing tests:

- **Fixtures:** These are objects that we can consider constants or the baseline for running tests. The primary use of fixtures is to ensure that variables that have the same value throughout testing will be used. There are two types of fixtures, and these are as follows:
  - `setUp()`: This method is executed *before* every test is invoked.
  - tearDown(): This method is executed *after* every test is invoked:

```
public class JavaTest extends TestCase {
    protected int value1, value2;
    // will run before testSubtract and testMultiply
    protected void setUp(){
        value1 = 23;
        value2 = 10;
    }
    public void testSubtract(){
        double result = value1 - value2;
        assertTrue(result == 13);
    }
    public void testMultiply(){
        double result = value1 * value2;
        assertTrue(result == 230);
    }
}
```

In the preceding code example, we can see that there are two test methods defined, which are testSubtract() and testMultiply(), before each method is called. The setUp() fixture will be called first to assign the values of the value1 and value2 variables.

- **Test suites:** Group several unit test cases and execute them together, using the @RunWith and @Suite annotations to run the tests. Let's have a look at the following example:

```
//JUnit Suite Test
@RunWith(Suite.class)
@Suite.SuiteClasses({
    TestOne.class, TestTwo.class
});
public class JunitTestSuite {
}
public class TestOne {
    int x = 1;
    int y = 2;
    @Test
    public void TestOne() {
        assertEquals(x + y, 3);
    }
}
```

```
}
public class TestTwo {
    int x = 1;
    int y = 2;
    @Test
    public void TestTwo() {
        assertEquals(y - x, 1);
    }
}
```

In the preceding code example, we can see that we have two defined classes with a method with the @Test annotation; the test methods will be executed together, as we have bundled them using the @Suite.SuiteClasses method.

- **Test runners:** Mainly used for running the test cases, we use the runClasses() method to run the test cases inside a specific class. Let’s have a look at a basic example here:

```
public class JUnitTestRunner {
    public static void main(String[] args) {
        Result result =
            JUnitCore.runClasses(TestJunit.class);

        for (Failure failure : result.getFailures()) {
            System.out.println(failure.toString());
        }

        System.out.println(result.wasSuccessful());
    }
}
```

- **Classes:** JUnit classes are mainly used for writing the tests for our application; these include the following:
  - **Assert:** Includes the set of assert methods
  - **Test case:** Includes the test cases that contain the fixtures for running multiple tests
  - **Test result:** Includes the methods to gather all of the results from an executed test case

## Assertions in JUnit

**Assertions** are the way to validate whether our tests are valid by checking the outcome of the written code. In JUnit, all assertions are under the `Assert` class, and some of the essential methods from Assert are as follows:

- `void assertTrue(boolean condition)`: Validates whether the condition is true
- `void assertFalse(boolean condition)`: Validates whether the condition is false
- `void assertNotNull(Object obj)`: Checks whether the object is not null
- `void assertNull(Object obj)`: Checks whether the object is null
- `void assertEquals(Object obj1, Object obj2)`: Checks whether two objects or primitives are equal
- `void assertArrayEquals(Array array1, Array array2)`: Validates whether two arrays are equal to each other

## Annotations

**Annotations** are meta tags that we add to methods and classes; this provides additional information to JUnit about which methods should run before and after the test methods and which will be ignored during the test execution.

Here are the annotations that we can use in JUnit:

- `@Test`: This annotation is used for a `public void` method to signify that the method is a test case that can be executed.
- `@Ignore`: This annotation is used to ignore a test case not being executed.
- `@Before`: This annotation is used for a `public void` method to run the method before each test case method. This is commonly used if we want to declare similar objects used by all test cases.
- `@After`: The annotation is used for a `public void` method to run the method after each test case method; this is commonly used if we want to release or clean several resources before running a new test case.
- `@BeforeClass`: The annotation allows a `public static void` method to run once before all of the test cases are executed.
- `@AfterClass`: The annotation allows a `public static void` method to run once all test cases are executed.

Let's have an example test with annotations and their sequence of execution:

```
public class JunitAnnotationSequence {

    //execute once before all test
    @BeforeClass
    public static void beforeClass() {
        System.out.println("beforeClass()");
    }
    //execute once after all test
    @AfterClass
    public static void afterClass() {
        System.out.println("afterClass()");
    }

    //execute before each test
    @Before
    public void before() {
        System.out.println("before()");
    }
    //execute after each test
    @After
    public void after() {
        System.out.println("after()");
    }
    @Test
    public void testMethod1() {
        System.out.println("testMethod1()");
    }
    @Test
    public void testMethod2() {
        System.out.println("testMethod2()");
    }
}
```

In the preceding code example, we have a `JunitAnnotationSequence` class that has several annotated methods. When we execute the test, we will have the following output:

```
beforeClass()
before()
testMethod1()
after()
before()
testMethod2()
after()
afterClass()
```

We can see in the preceding example that the methods annotated with `@BeforeClass` and `@AfterClass` are only called once and they are called at the start and end of the test execution. On the other hand, the methods annotated with `@Before` and `@After` are called at the beginning and the end of each test method.

We have learned about the basics of JUnit in unit testing; now, let’s discuss the concepts of AssertJ.

## Using AssertJ

We have just explored the concepts and features of JUnit in the last part, and we have learned that in JUnit alone, we can apply assertions using the Assert class, but we can make our assertions more fluent and flexible by using AssertJ. AssertJ is a library mainly used for writing assertions; its primary goal is to improve the readability of test code and make the maintenance of tests simpler.

Let’s compare how to write assertions in JUnit and AssertJ:

- JUnit checking whether the condition returns `true`:
  
  ```java
  Assert.assertTrue(condition)
  ```
  
- AssertJ checking whether the condition returns `true`:
  
  ```java
  Assertions.assertThat(condition).isTrue()
  ```

We can see in the preceding example that in AssertJ, we will always pass the object to be compared in the `assertThat()` method, and we will call the next method, which is the actual assertion. Let’s have a look at the different kinds of assertions we can use in AssertJ.
