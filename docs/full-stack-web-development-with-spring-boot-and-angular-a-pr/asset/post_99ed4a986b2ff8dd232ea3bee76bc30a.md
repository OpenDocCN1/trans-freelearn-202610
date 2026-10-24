
- `void log()` logs the details of the current time instruments with the logger.
- `void print()` prints the details of the current time instrument.

## Features of SLF4J

SLF4J has several features that make logs more helpful in debugging. It provides support for parameterized logging, which allows us to display dynamic values in our messages. Another feature is profiling, which is commonly used to measure different attributes such as the memory and execution time of specific tasks in an application.

Let’s discuss the concept and implementation of each feature.

## Parameterized logging

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

4. **Print the contents of the instrument:** We execute the print() method to display the contents and information of the Time instrument.

Now we have learned the concepts, features, and advantages of SLF4J, we will discuss a framework called Log4j2.

## Logging using Log4j2

**Log4j2** is one of the most common logging frameworks used with Java. Since SLF4J is an abstraction of logging frameworks, Log4j2 can be used with SLF4J. Log4j2 is very flexible and offers different ways to store log information for debugging; it also supports asynchronous logging and displays logs with a severity level to quickly identify the importance of messages.

Let's discuss the following features of Log4j2:

- The Log4j2 Logger
- Log4j2 Appenders
- Log4j2 Layouts
- Log4j2 Markers
- Log4j2 Filters

## The Log4j2 Logger

The **Logger** is the main feature used by our application to create LogRecord instances. This means the logger is responsible for dispatching the messages. To create a Log4j2 Logger, we only need the following code:

```
Logger log = LogManager.getLogger(ExampleClass.class);
```

After creating a new Logger, we can now use it to call several methods, such as info(), to dispatch messages.

## Log4j2 Appenders

**Appenders** are responsible for placing the logs dispatched by the Logger. In Log4j2, there are a wide range of Appenders that help us decide where to store our logs.

Here are some of the Appenders that are available from Log4j2:

- ConsoleAppender: Writes logs to the console (System.out or System.err). This is the default Appender.
- FileAppender: Writes logs to a file using FileManager.
- **JDBCAppender**: Writes logs to a database using a JDBC driver.
- **HTTPAppender**: Writes logs to a specific HTTP endpoint.
- **KafkaAppender**: Writes logs to Apache Kafka.
- **AsyncAppender**: Encapsulates another Appender and uses another thread to write logs, making it asynchronous logging.
- **SyslogAppender**: Writes logs to a syslog destination.

You can visit the Log4j2 documentation for other available Appenders at the following link: `https://logging.apache.org/log4j/2.x/manual/appenders.html`.

## Log4j2 Layouts

Appenders use **Layouts** to format the output of a LogEvent. Log4j2 has different Layouts we can choose from to format our logs:

- **Pattern**: The default Layout used by Log4j2 uses a string pattern to display the logs. For example, the `%d{HH:mm:ss} %msg%n;` pattern would give the following result:

```
14:25:30 Example log message
```

- **CSV**: The Layout for generating logs using CSV format.
- **HTML**: The Layout for generating logs in HTML format.
- **JSON**: The Layout for generating logs in JSON format.
- **XML**: The Layout for generating logs in XML format.
- **YAML**: The Layout for generating logs in YML format.
- **Syslog**: The Layout for generating logs into a syslog-compatible format.
- **Serialized**: Serializes the logs into a byte array using Java serialization.

## Log4j2 Markers

**Markers** are objects commonly used to mark a single log statement to identify whether we need to execute certain actions to specific logs. For example, we can mark a single log statement using the IMPORTANT Marker, which can indicate to the Appender that it needs to store the log in a different destination.

Let’s have a look at an example of how to create and use Markers:

```
public class Log4j2Marker {
    private static Logger LOGGER =
LoggerFactory.getLogger(Log4j2Marker.class);
private static final Marker IMPORTANT =
    MarkerFactory.getMarker("IMPORTANT");
public static void main(String[] args) {
    LOGGER.info("Message without a marker");
    LOGGER.info(IMPORTANT,"Message with marker"
}
}
```

In the preceding example, we can create a new Marker using the MarkerFactory.getMarker() method. To use the new Marker, we can apply it to a specific logger that indicates a particular action needed for significant events.

## Log4j2 Filters

Log4j2 Filters are another valuable feature for use in displaying loggers. This gives us the capability to control log events that we want to say or publish based on the given criteria. In executing a Filter, we can set it with the ACCEPT, DENY, or NEUTRAL values. Here are some of the Filters we can use to display loggers:

- **Threshold**: Applies filtering to log events using the severity level
- **Time**: Applies filtering to log events with a given time range
- **Regex**: Filters the log events based on a given regex pattern
- **Marker**: Filters the log events based on the given Marker
- **Composite**: Provides a mechanism to combine multiple filters
- **Dynamic Threshold**: Applies filtering to log events using the severity level and includes additional attributes

In the following section, we will configure the logging frameworks in our project.

## Setting up SLF4J and Log4j2

We will now implement several logging frameworks, including **Logback** and **Log4j2**, in our Spring Boot application. Remember that SLF4J is already included.

## Configuring Logback

**Logback** is the default logger used by Spring Applications, so no dependencies need to be installed to use it. The `spring-boot-starter-logging` dependency is already included once we create our Spring Boot application. The first step we need to take is to make our Logback configuration file.

In our project, under the `resources` folder, we will add the `logback-spring.xml` file. This is where we will place our Logback configuration. The following is an example configuration:

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

    <property name="LOGS" value="./logs" />
    <!--Please refer to the logback-spring.xml of
    the GitHub repo. Thank you. -->
    <!-- LOG everything at INFO level -->
    <root level="info">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </root>

    <logger name="com.example" level="trace"
        additivity="false">
        <appender-ref ref="RollingFile" />
        <appender-ref ref="Console" />
    </logger>

</configuration>
```

In the preceding XML, several configurations were defined to format our log events. We have created two Appenders - `Console` and `RollingFile`. Configuring the two appender tags will create logs in System.out and File Output.

We have also used a pattern that modifies the look and format of the log display. In this example, we have used the `%black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %yellow(%C{1.}): %msg%n%throwable` pattern to display the logs in System.Out. It shows the date in black, the severity level in highlight, the thread name in blue, the class name in yellow, and the message assigned to the logs.
