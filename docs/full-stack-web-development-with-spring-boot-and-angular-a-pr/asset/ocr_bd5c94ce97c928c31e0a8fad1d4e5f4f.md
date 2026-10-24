+   4. **Print the contents of the instrument:** We execute the print() method to display the contents and information of the Time instrument.

Now we have learned the concepts, features, and advantages of SLF4J, we will discuss a framework called Log4j2.

# Logging using Log4j2

**Log4j2** is one of the most common logging frameworks used with Java. Since SLF4J is an abstraction of logging frameworks, Log4j2 can be used with SLF4J. Log4j2 is very flexible and offers different ways to store log information for debugging; it also supports asynchronous logging and displays logs with a severity level to quickly identify the importance of messages.

Let's discuss the following features of Log4j2:

+   - The Log4j2 Logger
+   - Log4j2 Appenders
+   - Log4j2 Layouts
+   - Log4j2 Markers
+   - Log4j2 Filters

# The Log4j2 Logger

The **Logger** is the main feature used by our application to create LogRecord instances. This means the logger is responsible for dispatching the messages. To create a Log4j2 Logger, we only need the following code:

```
Logger log = LogManager.getLogger(ExampleClass.class);
```

After creating a new Logger, we can now use it to call several methods, such as info(), to dispatch messages.

# Log4j2 Appenders

**Appenders** are responsible for placing the logs dispatched by the Logger. In Log4j2, there are a wide range of Appenders that help us decide where to store our logs.

Here are some of the Appenders that are available from Log4j2:

+   - ConsoleAppender: Writes logs to the console (System.out or System.err). This is the default Appender.
+   - FileAppender: Writes logs to a file using FileManager.