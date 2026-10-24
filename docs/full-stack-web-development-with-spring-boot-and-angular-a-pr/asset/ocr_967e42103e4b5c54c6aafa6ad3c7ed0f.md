```
.collect(Collectors.toList());
}
```

In the preceding example, we have invoked info(String message). Every time the user calls the get anti-heroes endpoint, the log will be displayed. We can also invoke the following methods:

+   - trace(): Displays the log events on the TRACE level
+   - debug(): Displays the log events on the DEBUG level
+   - warn(): Displays the log events on the WARN level
+   - error(): Displays the log events on the ERROR level
+   - getName(): Retrieves the name of the logger
+   - isInfoEnabled(): Checks whether the logger is enabled on the INFO level
+   - isDebugEnabled(): Checks whether the logger is enabled on the DEBUG level
+   - isWarnEnabled(): Checks whether the logger is enabled on the WARN level
+   - isErrorEnabled(): Checks whether the logger is enabled on the ERROR level

# Annotations in Lombok

Now let's see how Lombok, a library in our Spring Boot application, can help us. Lombok can simplify our code by using annotations, but it also offers annotations for SLF4J and Log4j2 as follows:

+   - @log4j2: This annotation will generate a new Log4j2 instance in our class. The following example code will be generated:

```
public class LogExample {
    private static final org.SLF4J.Logger log =
        org.SLF4J.LoggerFactory.getLogger(
            LogExample.class);
}
```

+   - @slf4j: This annotation will generate a new SLF4J instance in our class. The following example code will be generated:

```
public class LogExample {
    private static final org.SLF4J.Logger log =
        org.SLF4J.LoggerFactory.getLogger(
            LogExample.class);
}
```