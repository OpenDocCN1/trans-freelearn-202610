

- `void log()` 使用记录器记录当前时间仪器的详细信息。
- `void print()` 打印当前时间仪器的详细信息。

## SLF4J 的特性

SLF4J 有多个特性使其日志在调试时更有帮助。它提供了参数化日志的支持，允许我们在消息中显示动态值。另一个特性是分析概要，它常用于衡量应用程序中特定任务的内存和执行时间等不同属性。

让我们讨论每个特性的概念和实现。

## 参数化日志

要在 SLF4J 中使用参数化日志，我们将在消息中包含占位符 {}，以便在该位置传递值。

让我们看一下以下示例：

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

在前面的示例中，我们在消息中创建了一个参数来显示 name 变量的值。一旦我们执行应用程序，输出将如下所示：

```
INFO: Hello seiji, here is your log
```

参数化日志也支持消息中的多个参数，如以下示例所示：

```
public class LoggerExample {
    public static void main(String[] args) {

        //Creating the Logger object
```

4. **打印仪器的内容：** 我们执行 print() 方法以显示 Time 仪器的内容和信息。

现在我们已经学习了 SLF4J 的概念、特性和优点，我们将讨论一个名为 Log4j2 的框架。

## 使用 Log4j2 进行日志记录

**Log4j2** 是与 Java 一起使用的最常见的日志框架之一。由于 SLF4J 是日志框架的抽象，因此 Log4j2 可以与 SLF4J 一起使用。Log4j2 非常灵活，并提供不同的方式来存储日志信息以供调试；它还支持异步日志记录并以严重性级别显示日志，以便快速识别消息的重要性。

让我们讨论 Log4j2 的以下特性：

- Log4j2 Logger
- Log4j2 Appenders
- Log4j2 Layouts
- Log4j2 Markers
- Log4j2 Filters

## Log4j2 Logger

**Logger** 是应用程序用于创建 LogRecord 实例的主要特性。这意味着记录器负责分派消息。要创建 Log4j2 Logger，我们只需要以下代码：

```
Logger log = LogManager.getLogger(ExampleClass.class);
```

创建新的 Logger 后，我们现在可以使用它来调用多个方法，例如 info()，以分派消息。

## Log4j2 Appenders

**Appenders** 负责放置由 Logger 分发的日志。在 Log4j2 中，有大量 Appenders 帮助我们决定在哪里存储日志。

以下是 Log4j2 中可用的一些 Appenders：

- ConsoleAppender: 将日志写入控制台（System.out 或 System.err）。这是默认的 Appender。
- FileAppender: 使用 FileManager 将日志写入文件。
- **JDBCAppender**: 使用 JDBC 驱动程序将日志写入数据库。
- **HTTPAppender**: 将日志写入特定的 HTTP 端点。
- **KafkaAppender**: 将日志写入 Apache Kafka。
- **AsyncAppender**: 封装另一个 Appender 并使用另一个线程写入日志，使其成为异步日志记录。
- **SyslogAppender**: 将日志写入 syslog 目标。

您可以在以下链接访问 Log4j2 文档以了解其他可用的 Appenders：`https://logging.apache.org/log4j/2.x/manual/appenders.html`。

## Log4j2 Layouts

Appender 使用 **Layouts** 来格式化 LogEvent 的输出。Log4j2 有不同的 Layouts 可供选择来格式化我们的日志：

- **Pattern**: Log4j2 使用的字符串模式的默认 Layout 来显示日志。例如，`%d{HH:mm:ss} %msg%n;` 模式将给出以下结果：

```
14:25:30 Example log message
```

- **CSV**: 用于生成 CSV 格式日志的 Layout。
- **HTML**: 用于生成 HTML 格式日志的 Layout。
- **JSON**: 用于生成 JSON 格式日志的 Layout。
- **XML**: 用于生成 XML 格式日志的 Layout。
- **YAML**: 用于生成 YML 格式日志的 Layout。
- **Syslog**: 用于生成 syslog 兼容格式日志的 Layout。
- **Serialized**: 使用 Java 序列化将日志序列化为字节数组。

## Log4j2 Markers

**Markers** 是通常用于标记单个日志语句以确定我们是否需要对特定日志执行某些操作的<|message|>。例如，我们可以使用 IMPORTANT Marker 标记单个日志语句，这可以指示 Appender 需要将日志存储在不同的目标中。

让我们看一下如何创建和使用 Markers 的示例：

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

在前面的示例中，我们可以使用 MarkerFactory.getMarker() 方法创建一个新的 Marker。要使用新的 Marker，我们可以将其应用于特定的记录器，以指示对重要事件需要采取特定操作。

## Log4j2 Filters

Log4j2 Filters 是用于显示记录器的另一个有价值的特性。这使我们能够根据给定条件控制我们想要说或发布的日日志事件。在执行 Filter 时，我们可以将其设置为 ACCEPT、DENY 或 NEUTRAL 值。以下是我们可用于显示记录器的一些 Filters：

- **Threshold**: 使用严重性级别对日志事件进行过滤
- **Time**: 使用给定时间范围对日志事件进行过滤
- **Regex**: 基于给定的正则表达式模式过滤日志事件
- **Marker**: 基于给定的 Marker 过滤日志事件
- **Composite**: 提供组合多个过滤器的机制
- **Dynamic Threshold**: 使用严重性级别并包含附加属性对日志事件进行过滤

在以下部分，我们将在项目中配置日志框架。

## 设置 SLF4J 和 Log4j2

我们现在将在 Spring Boot 应用程序中实现多个日志框架，包括 **Logback** 和 **Log4j2**。请记住 SLF4J 已经包含在内。

## 配置 Logback

**Logback** 是 Spring 应用程序使用的默认记录器，因此使用它无需安装依赖项。`spring-boot-starter-logging` 依赖项在我们创建 Spring Boot 应用程序时已包含在内。我们需要采取的第一步是制作 Logback 配置文件。

在我们的项目中，在 `resources` 文件夹下，我们将添加 `logback-spring.xml` 文件。这就是我们放置 Logback 配置的地方。以下是一个示例配置：

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

在前面的 XML 中，定义了多个配置来格式化我们的日志事件。我们创建了两个 Appenders - `Console` 和 `RollingFile`。配置两个 appender 标签将创建 System.out 和文件输出中的日志。

我们还使用了一个模式来修改日志显示的外观和格式。在此示例中，我们使用了 `%black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %yellow(%C{1.}): %msg%n%throwable` 模式在 System.Out 中显示日志。它以黑色显示日期，以高亮显示严重性级别，以蓝色显示线程名称，以黄色显示类名，并显示分配给日志的消息。

