# Assertions in JUnit

**Assertions** are the way to validate whether our tests are valid by checking the outcome of the written code. In JUnit, all assertions are under the `Assert` class, and some of the essential methods from Assert are as follows:

+   - `void assertTrue(boolean condition)`: Validates whether the condition is true
+   - `void assertFalse(boolean condition)`: Validates whether the condition is false
+   - `void assertNotNull(Object obj)`: Checks whether the object is not null
+   - `void assertNull(Object obj)`: Checks whether the object is null
+   - `void assertEquals(Object obj1, Object obj2)`: Checks whether two objects or primitives are equal
+   - `void assertArrayEquals(Array array1, Array array2)`: Validates whether two arrays are equal to each other

# Annotations

**Annotations** are meta tags that we add to methods and classes; this provides additional information to JUnit about which methods should run before and after the test methods and which will be ignored during the test execution.

Here are the annotations that we can use in JUnit:

+   - `@Test`: This annotation is used for a `public void` method to signify that the method is a test case that can be executed.
+   - `@Ignore`: This annotation is used to ignore a test case not being executed.
+   - `@Before`: This annotation is used for a `public void` method to run the method before each test case method. This is commonly used if we want to declare similar objects used by all test cases.
+   - `@After`: The annotation is used for a `public void` method to run the method after each test case method; this is commonly used if we want to release or clean several resources before running a new test case.
+   - `@BeforeClass`: The annotation allows a `public static void` method to run once before all of the test cases are executed.
+   - `@AfterClass`: The annotation allows a `public static void` method to run once all test cases are executed.