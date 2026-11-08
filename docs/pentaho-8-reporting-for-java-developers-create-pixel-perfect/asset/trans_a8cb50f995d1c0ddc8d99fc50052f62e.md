
## 现有的 DataFactory 实现

现在你已经学习了 Pentaho Reporting Engine Data API 基础知识，在本节中，你将深入了解作为 Pentaho Reporting Engine 组成部分的 `DataFactory` 实现。为了更好地理解，你将学习常用的 `DataFactory` 实现，包括最常用且使用最多的工厂的工作示例。正如你在本书前几章中多次看到的那样，所有示例都可在 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库中找到；具体在 `Chapter 12 - Developing using data sources` 文件夹中。

![](img/9337ac94537bf7e092e1ea6531dce8fd_561_0.png)

> *在以下所有示例中，你都找不到最终报告的预览。这并不是因为布局不相关，而是本章更侧重于数据源配置。我们在过去章节中分享过的建议始终如一：考虑到你拥有完整的源代码，并且你的目标是学习 Pentaho Reporting，请尝试在你的开发环境中运行这些示例作为练习。*

## TableDataFactory

`org.pentaho.reporting.engine.classic.core.TableDataFactory` 类是用于将数据加载到 Pentaho Reporting Engine 的最简单的 `DataFactory` 形式。
`TableDataFactory` 管理一组现有的 `TableModel` 实例并使其可供报表引擎使用。

要构造 `TableDataFactory`，你可以使用默认构造函数或以下便捷构造函数：public TableDataFactory(final String name, final TableModel tableModel);

此便捷构造函数使用特定名称向 `TableDataFactory` 所知的 `TableModel` 实例列表添加一个 `TableModel` 实例。添加 `TableModel` 实例到 `TableDataFactory` 的另一种方法是使用以下 API 调用：public void addTable(final String name, final TableModel tableModel);

`addTable` 方法向 `TableDataFactory` 维护的命名 `TableModel` 实例列表添加一个额外的表：虽然没有定义 `DataFactory` CSV，但有一个名为 `CSVTableModelProducer` 的辅助类可用，其位于 `org.pentaho.reporting.engine.classic.core.modules.misc.tablemodel` 包中。此 `CSVTableModelProducer` 类将生成另一个类 `CSVTableModel`，然后你可以将其添加到 `TableDataFactory`。

## TableDataFactory 示例

使用 `TableDataFactory` 类进行 Pentaho Report 开发的示例可在 https://github.com/fcorti/pentaho-8-reporting-for-java-developers 仓库中找到；具体在 `chapter 12 - Developing using data sources` 文件夹中。在文件夹内部，你将找到 `pentaho-reporting-data-table` 项目，使用 `PentahoServlet` 类，其源代码如下：

// Loading the Table DataFactory.
DefaultTableModel tableModel = new DefaultTableModel(
    new Object[][] {
        {"Product A", 123},
        {"Product B", 234},
        {"Product C", 345},
        {"Product D", 456}
    },
    new String[] {"Product", "Cost"});
TableDataFactory dataFactory = new TableDataFactory();
dataFactory.addTable("default", tableModel);
report.setDataFactory(dataFactory);

要运行该项目，在本地机器上下载 `pentaho-reporting-data-source-table` 文件夹，打开终端，进入此文件夹，并按给定顺序执行以下命令：

```
mvn clean install
mvn package
java -jar target/dependency/jetty-runner.jar target/*.war
```

在浏览器中打开 `http://localhost:8080`，你应该会以 PDF 格式看到 Pentaho 报表作为结果。

## SQLReportDataFactory

`org.pentaho.reporting.engine.classic.core.modules.misc.datafactory.sql.SQLReportDataFactory` 类允许你轻松使用 JDBC 和 SQL 为报表填充数据。

要在环境中正确设置 `SQLReportDataFactory`，你首先需要创建一个 `ConnectionProvider` 实例。可供使用的有三个 `ConnectionProvider` 实现（`StaticConnectionProvider`、`DriverConnectionProvider` 和 `JndiConnectionProvider`）。`ConnectionProvider` 接口和三个实现位于与 `SQLReportDataFactory` 相同的包中。

`ConnectionProvider` 接口定义了单个 `createConnection` 方法，该方法应返回一个 JDBC 连接，供 `SQLReportDataFactory` 在生成结果集以填充报表时使用。

## StaticConnectionProvider

`ConnectionProvider` 的第一个实现是 `StaticConnectionProvider` 类，其在构造函数中接受一个现有的 `java.sql.Connection` 对象：

```
ConnectionProvider conn = new StaticConnectionProvider(sqlConn);
```

## DriverConnectionProvider

`ConnectionProvider` 的第二个实现是 `DriverConnectionProvider` 类，它允许你轻松配置连接。`DriverConnectionProvider` 有一个默认构造函数，必须通过 setter 方法进行配置。在下表中，介绍了其中一些最相关的方法：

| 方法 | 描述 |
|---|---|
| `void setDriver(String driver)` | `setDriver` 方法的 driver 值应设置为 JDBC 中使用的驱动程序类的完全限定名称。 |
| `void setUrl(String url)` | `setUrl` 方法的 URL 值应设置为用于连接到数据库的 JDBC URL。 |
| `void setProperty(String name, String value)` | `setProperty` 方法向 JDBC 配置属性添加一个附加属性。诸如 `user` 和 `password` 之类的属性通常通过属性列表传递，以便通过 `java.sql.DriverManager` 生成新连接。 |

![](img/9337ac94537bf7e092e1ea6531dce8fd_566_0.png)

*请注意，通过使用 `DriverConnectionProvider`，每次在 API 上调用 `createConnection` 方法时，你都会为底层数据库创建一个新连接。这不是好也不是坏，但在开发过程中你应该意识到这一点。*

## JndiConnectionProvider

第三个可用的 `ConnectionProvider` 是 `JndiConnectionProvider`。`JndiConnectionProvider` 使用 **Java 命名和目录接口 (JNDI)** 来获取数据库连接。它有一个默认构造函数，以及以下可用于连接到 JNDI 数据源的 setter 方法。只有连接路径属性是强制性的。

在下表中，介绍了其中一些最相关的方法：

| 方法 | 描述 |
|---|---|
| `void setConnectionPath(String connectionPath)` | `setConnectionPath` 方法应设置为 JDBC 数据源的完整 JNDI 路径 |
| `void setUsername(String username)` | `setUsername` 方法是一个可选字段，在连接到数据源时可能需要 |
| `void setPassword(String password)` | `setPassword` 方法是一个可选字段，在连接到数据源时可能需要 |

JNDI 可在许多应用服务器中使用。它也可以在普通 Java 应用中使用 simple JNDI 的帮助，网址为 http://www.osjava.org/simple-jndi。以下列表显示了在不同环境中的 JNDI 连接字符串示例，假设你已在该环境中配置了 JNDI 数据源：

- **Tomcat Java:** `comp/env/jdbc/<DataSourceName>`
- **JBoss Java:** `<DataSourceName>`

## 使用 SQLReportDataFactory

一旦创建了 `ConnectionProvider`，现在是设置 `SQLReportDataFactory` 实例的时候了。`SQLReportDataFactory` 包含两个构造函数：第一个构造函数接受 `ConnectionProvider`，第二个构造函数接受一个现有的 `connection`，它在内部使用 `StaticConnectionProvider` 来提供连接。

除了配置 `ConnectionProvider` 之外，你还必须为 `SQLReportDataFactory` 提供要执行的查询。使用 `setQuery` 方法配置命名查询：

```
void setQuery(final String name, final String queryString)
```

`setQuery` 方法接收查询名称和 SQL 查询本身。如前所述，本章示例中的主报表使用名称 `default` 来引用主查询。

要在查询中包含报表参数，你可以在 SQL 中使用以下格式指定变量：`${<PARAMETER NAME>}`。会生成一个 `PreparedStatement`，并且这些变量会根据 JDBC 规范进行适当参数化，以避免潜在的 SQL 注入攻击。除了传统的 SQL 语句之外，你还可以调用 SQL 存储过程。这些调用以字符串 `call` 开头，并被 `SQLReportDataFactory` 识别，以确保执行正确的 JDBC API 方法。

如前所述，对所有 `DataFactory` 实例提供的 `QUERY_LIMIT` 和 `QUERY_TIMEOUT` 参数也适用于 `SQLReportDataFactory`。这些值会传递给 `Connection` 对象。因此，底层 JDBC 驱动程序也必须支持这些属性才能生效。
