## 在 Spring Boot 中记录事件

在上一章中，我们讨论了**跨域资源共享（CORS）**在保护应用程序方面的概念、特性和实现。我们还学习了**JSON Web 令牌（JWT）**，以及如何通过创建身份验证端点来生成令牌。

本章将重点介绍在 Spring Boot 应用程序中记录事件。我们将讨论用于日志记录和配置 Spring Boot 的流行软件包、日志保存位置，以及在开发应用程序时如何处理日志。

在本章中，我们将涵盖以下主题：

- 开始使用 SLF4J 和 Log4j2
- 设置 SLF4J 和 Log4j2
- 使用日志

### 技术要求

本章代码的最终版本可参见以下链接：https://github.com/PacktPublishing/Spring-Boot-and-Angular/tree/main/Chapter-08/superheroes。

### 开始使用 SLF4J 和 Log4j2

**日志记录**被认为是开发应用程序最重要的方面之一。它的重要性往往被低估，更糟糕的是，我们忘记了将其应用到应用程序中。

事件日志记录在大多数科技行业中使用，尤其是那些提供企业应用程序的行业。它按照特定标准应用，以避免复杂的调试，并让我们更容易理解所阅读的代码。编写良好且结构化的日志可以使开发人员受益，尤其是在维护或调试另一位开发人员的代码时。记录可以加快调试过程，而不是费力地搜索错误，它会提供错误发生的位置和原因，以及应用程序中发生了什么的详细信息。

随着语言和框架的改进，日志记录也在不断发展；在后端开发中，已经开发了多个日志框架来提供更灵活的日志记录能力。我们将讨论的一些示例框架是用于 Java Spring Boot 的 SLF4J 和 Log4j2。在继续之前，让我们讨论日志框架的特性。

### 日志框架的特性

日志框架包含以下三个特性，用于我们在应用程序中显示和捕获事件：

- **记录器（Logger）**：获取消息和元数据
- **格式化器（Formatter）**：格式化从记录器检索到的消息
- **处理器（Handler）**：负责在调试控制台中打印消息，或为开发人员将它们插入数据库

日志框架还显示不同严重级别的消息，使开发人员能够快速识别发生了哪个事件。日志框架中的严重级别如下：

- **致命（FATAL）**：级别 1；被认为是可能导致应用程序暂停或终止的关键问题
- **错误（ERROR）**：应用程序中的运行时错误
- **警告（WARNING）**：通常显示已弃用 API 的日志
- **信息（INFO）**：显示应用程序运行时事件的日志
- **调试（DEBUG）**：显示应用程序流程信息的日志
- **跟踪（TRACE）**：显示应用程序流程更详细信息的日志

### 使用 SLF4J 记录日志

在 Java 中使用的流行日志框架之一是**Java 简单日志门面（Simple Logging Façade for Java）**，即 **SLF4J**。它是使用最广泛的框架之一，因为它允许用户仅使用一个依赖项即可使用任何日志框架，例如 Log4j、Logback、`java.util.logging` 包或 Java 自己的日志引擎 JUL。这意味着我们可以根据需要从一个日志框架切换到另一个日志框架。

使用 SLF4J 有几个优点：

- SLF4J 使我们能够在运行时或部署时从一个框架切换到另一个框架。
- SLF4J 有一个迁移工具，允许我们使用 Java 类库将现有项目从 Log4j 迁移到 SLF4J。
- SLF4J 支持参数化日志消息，以便为我们的日志绑定动态值。
- SLF4J 将应用程序与日志框架解耦。在开发应用程序时，我们无需担心正在使用哪个日志框架。

### SLF4J 的方法和类

SLF4J 提供了多个类和方法来显示带有严重级别的消息、分析执行时间，或直接返回记录器实例。让我们讨论这些提供的方法和类。

### Logger 接口

Logger 接口主要用于显示带有严重级别的消息或日志。它也是 SLF4J API 的入口点。

- `void debug(String message)`：在 **DEBUG** 级别记录消息
- `void error(String message)`：在 **ERROR** 级别记录消息
- `void info(String message)`：在 **INFO** 级别记录消息
- `void trace(String message)`：在 **TRACE** 级别记录消息
- `void warn(String message)`：在 **WARN** 级别记录消息

### LoggerFactory 类

LoggerFactory 类是 SLF4J 的实用类，通常用于使用 JUL 和 Log4j 等框架创建记录器。

Logger getLogger(String name) 生成具有指定名称的记录器对象。以下示例使用了 getLogger() 方法：

```
private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogClass.class);
```

### Profiler 类

Profiler 类主要用于识别应用程序中特定任务的执行时间，也被称为**简易分析器（poor man's profiler）**。

可以使用多种方法：

- `void start(string name)` 创建一个具有特定名称的新子秒表，并停止之前创建的秒表。
- `TimeInstrument stop()` 停止最近创建的子秒表和全局秒表，并返回当前执行时间。





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





成功配置 Logback 后，我们可以运行应用程序并在控制台中看到日志：

```
2022-03-29 12:04:06,487 INFO  [background-preinit] org.hibernate.validator.internal.util.Version: HV000001: Hibernate Validator 6.2.0.Final
2022-03-29 12:04:06,728 INFO  [restartedMain] org.springframework.boot.StartupInfoLogger: Starting SuperheroesApplication using Java 17 on LAPTOP-NEIIT559 with PID 9512 (C:\Users\Sei
2022-03-29 12:04:06,728 DEBUG [restartedMain] org.springframework.boot.StartupInfoLogger: Running with Spring Boot v2.6.2, Spring v5.3.14
2022-03-29 12:04:06,729 INFO  [restartedMain] org.springframework.boot.SpringApplication: No active profile set, falling back to default profiles: default
2022-03-29 12:04:06,805 INFO  [restartedMain] org.springframework.boot.logging.DeferredLog: Devtools property defaults active! Set 'spring.devtools.add-properties' to 'false' to disa
2022-03-29 12:04:06,805 INFO  [restartedMain] org.springframework.boot.logging.DeferredLog: For additional web related logging consider setting the 'logging.level.web' property to 'D
2022-03-29 12:04:06,376 INFO  [restartedMain] org.springframework.data.repository.config.RepositoryConfigurationDelegate: Multiple Spring Data modules found, entering strict repositio
2022-03-29 12:04:06,379 INFO  [restartedMain] org.springframework.data.repository.config.RepositoryConfigurationDelegate: Bootstrapping Spring Data JPA repositories in DEFAULT mode.
```

图 8.1 – 使用 Logback 的日志事件

现在我们将在日志中使用 Log4j2 框架。

## 配置 Log4j2

我们也可以在我们的应用程序中使用不同的框架来记录日志事件。在本示例中，我们将使用 Log4j2 来处理日志：

+   1. 第一步是将 Log4j2 依赖添加到我们的 `pom.xml` 文件中。为此，我们将添加以下代码：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

+   2. 成功添加依赖后，我们必须排除 Spring Boot 应用程序中的 `spring-boot-starter-logging` 依赖，以便覆盖 Logback 并在日志事件中使用 Log4j2 框架。

为此，我们必须将以下 XML 代码添加到 `org.springframework.boot` 组下的依赖项中：

```
<exclusions>
    <exclusion>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-logging</artifactId>
    </exclusion>
</exclusions>
```

+   3. 在引入 Log4j2 依赖后，我们将向 `resources` 文件夹中添加一个名为 `log4j2-spring.xml` 的文件，并添加以下 XML 配置：

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
```

## Spring Boot 中的日志事件

一个示例是在用户尝试获取所有反英雄列表时创建一条日志。在 `AntiHeroController` 中，我们将添加以下代码来创建一个新的日志记录器实例：

```
private static final Logger LOGGER = LoggerFactory.
getLogger(AntiHeroController.class);
```

我们还必须注意，`LoggerFactory` 和 `Logger` 应该位于 SLF4J 依赖下。始终建议使用 SLF4J，因为它是日志框架的抽象，可以更轻松地在不同框架之间切换。

在这种情况下，我们的导入应如下所示：

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
```

一旦我们创建了新的日志记录器实例，就可以在方法中使用它，例如，如果我们想在用户尝试获取反英雄列表时显示日志。

为此，在 `getAntiHeroes()` 方法下，我们将添加以下代码：

```
public List<AntiHeroDto> getAntiHeroes(Pageable pageable) {
    int toSkip = pageable.getPageSize() *
                pageable.getPageNumber();
    //SLF4J
    LOGGER.info("Using SLF4J: Getting anti hero
                list - getAntiHeroes()");

    // Mapstruct is another dto mapper, but it's not
    // straightforward
    var antiHeroList = StreamSupport
        .stream(
            service.findAllAntiHeroes().spliterator(),
            false)
        .skip(toSkip).limit(pageable.getPageSize())
        .collect(Collectors.toList());

    return antiHeroList
        .stream()
        .map(this::convertToDto)
```

```
.collect(Collectors.toList());
}
```

在前面的示例中，我们调用了 `info(String message)`。每次用户调用获取反英雄端点时，都会显示日志。我们还可以调用以下方法：

-   - `trace()`：在 TRACE 级别显示日志事件
-   - `debug()`：在 DEBUG 级别显示日志事件
-   - `warn()`：在 WARN 级别显示日志事件
-   - `error()`：在 ERROR 级别显示日志事件
-   - `getName()`：获取日志记录器的名称
-   - `isInfoEnabled()`：检查日志记录器是否在 INFO 级别启用
-   - `isDebugEnabled()`：检查日志记录器是否在 DEBUG 级别启用
-   - `isWarnEnabled()`：检查日志记录器是否在 WARN 级别启用
-   - `isErrorEnabled()`：检查日志记录器是否在 ERROR 级别启用

## Lombok 中的注解

现在让我们看看 Lombok（我们 Spring Boot 应用程序中的一个库）如何帮助我们。Lombok 可以通过使用注解来简化代码，它还提供了用于 SLF4J 和 Log4j2 的注解，如下所示：

-   - `@Log4j2`：此注解将在我们的类中生成一个新的 Log4j2 实例。将生成以下示例代码：

```
public class LogExample {
    private static final org.SLF4J.Logger log =
        org.SLF4J.LoggerFactory.getLogger(
            LogExample.class);
}
```

-   - `@Slf4j`：此注解将在我们的类中生成一个新的 SLF4J 实例。将生成以下示例代码：

```
public class LogExample {
    private static final org.SLF4J.Logger log =
        org.SLF4J.LoggerFactory.getLogger(
            LogExample.class);
}
```

-   - 建议使用 `@Slf4j` 注解，因为它允许切换日志框架。

一旦我们在类中使用了这些注解，就不需要创建新的实例，可以直接在方法中使用 `log`：

```
//LOMBOK SLF4J
log.info("Using SLF4J Lombok: Getting anti-hero list - getAntiHeroes()");
```

## 总结

本章解释了日志记录器的概念和重要性，以及它们如何帮助开发人员调试和维护应用程序。本章介绍了 Log4j2，这是一个用于 Spring Boot 的第三方框架，提供了多项功能，如 **Appenders**、**Filters** 和 **Markers**，可以帮助开发人员对日志事件进行分类和格式化。本章还介绍了 SLF4J 的概念，它是日志框架的抽象，允许我们在运行时或部署时在不同框架之间切换。

在下一章中，我们将学习单元测试的概念以及在 Spring Boot 应用程序中的集成。
