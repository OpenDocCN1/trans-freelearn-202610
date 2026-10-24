+   - `void log()` logs the details of the current time instruments with the logger.
+   - `void print()` prints the details of the current time instrument.

# Features of SLF4J

SLF4J has several features that make logs more helpful in debugging. It provides support for parameterized logging, which allows us to display dynamic values in our messages. Another feature is profiling, which is commonly used to measure different attributes such as the memory and execution time of specific tasks in an application.

Let’s discuss the concept and implementation of each feature.

# Parameterized logging

To use parameterized logging in SLF4J, we will include placeholders {} in the message where we want to pass the value.

Let’s have a look at the following example:

```
public class LoggerExample {
    public static void main(String[] args) {

        //Creating the Logger object
        Logger logger =
            LoggerFactory.getLogger(LoggerExample.class);
        String name = "seiji";

        //Logger using info level
        logger.info("Hello {}, here is your log", name);
    }
}
```

In the preceding example, we have created a parameter in our message to display the value of the name variable. Once we execute the application, the output will be as follows:

```
INFO: Hello seiji, here is your log
```

Parameterized logging also supports multiple parameters in messages, as in the following example:

```
public class LoggerExample {
    public static void main(String[] args) {

        //Creating the Logger object
```