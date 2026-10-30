

![](img/553e44bc30e70d0d8e58278a7ece8da8_73_0.png)

创建数据源，方法是选择 **全局 | 数据源** 节点，然后单击 **新建**，就像我们为创建连接池所做的那样。在 **创建数据源** 窗口中，将事务级别选择为受管数据源，指定数据源 **名称** 并单击 **确定**。数据源也可以配置为 **原生数据源**。受管数据源与原生数据源之间的区别在于 OC4J 服务器不会包装原生数据源。OC4J 服务器不会为原生数据源提供连接缓存。原生数据源不支持全局/分布式事务。分布式事务是指跨多个数据库服务器的事务。如果要配置原生数据源，请指定实现 `javax.sql.DataSource` 接口的数据源类。**连接池** 不应为原生数据源配置。

![](img/553e44bc30e70d0d8e58278a7ece8da8_73_1.png)

## 在 Oracle JDeveloper 中配置 JDBC

添加一个新的 **受管数据源**。指定数据源要关联的连接池名称为我们之前配置的 **连接池**。指定 JNDI 名称、用户名、密码、登录超时、事务级别，然后单击 **确定**。

![](img/553e44bc30e70d0d8e58278a7ece8da8_74_0.png)

受管数据源 gets configured and updates the `data-sources.xml` file using the **刷新** button. 受管数据源也可以通过直接修改目录 `C:\JDeveloper\jdev\system\oracle.j2ee.10.1.3.nn.nn\embedded-oc4j\config` 中的 `data-sources.xml` 配置文件以声明方式创建。`<JDeveloper10.1.3>` 是 JDeveloper 安装的目录。向 `data-sources.xml` 文件添加一个 `managed-data-source` 元素以指定 `DataSource` 对象的配置。`managed-data-source` 元素包括用于获取 `DataSource` 对象的数据源类。用于配置数据源 `MySQLDataSource`、使用 `MySQL` 数据库、使用 `jndi-name jdbc/MySQLDataSource` 的 `managed-data-source` 元素如下所示：

```
<managed-data-source name='MySQLDataSource'
connection-pool-name='MySQL Connection Pool'
jndi-name='jdbc/MySQLDataSource'/>
<connection-pool name='MySQL Connection Pool'>
<connection-factory factory-class='com.mysql.jdbc.jdbc2.optional.MysqlConnectionPoolDataSource'
user='root'
password=''
url="jdbc:mysql://localhost:3306/test">
</connection-factory>
</connection-pool>
```

`data-sources.xml` 文件中的元素基于 XML Schema http://www.oracle.com/technology/oracleas/schema/data-sources-10_1.xsd。根元素是 data-sources，data-sources 元素有一个或多个 managed-data-source、connection-pool 和 native-data-source 元素。managed-data-source 元素具有以下属性：name、user、password、login-timeout、tx-level、schema、connection-pool-name 和 jndi-name。每个 managed-data-source 元素都与一个连接池关联，该连接池在 connection-pool 元素中指定。connection-pool 元素具有 connection-factory 子元素和属性，这些属性在以下表格中讨论：

| 元素 | 描述 | 默认值 |
|---|---|---|
| name | 连接池名称。 | -1 |
| min-connections | 连接池中的最小连接数。 | 0 |
| max-connections | 连接池中打开连接的最大数量。值为 0 或更小表示没有最大限制。 | 0 |
| initial-limit | 创建或重新初始化连接池时的初始连接数。指定连接池的初始大小可减少达到最佳大小的启动时间。 | 0 |
| used-connection-wait-timeout | 当连接池达到最大连接数且池中所有连接都在使用时，连接池会等待指定时间以供客户端释放连接回连接池。 | 60 |
| inactivity-timeout | 当连接池中的连接非活动时间达到非活动超时期限（以秒为单位）时，该连接将被移除。 | 60 |
| login-timeout | 指定数据源等待建立与数据库连接的时间（以秒为单位）。值为 0 表示默认系统超时。 | 0 |
| connection-retry-interval | 指定在重试与数据库连接之前等待的时间间隔（以秒为单位）。 | 1 |
| max-connect-attempts | 指定重试连接到数据库的次数。 | 3 |
| validate-connection | 如果设置为 true，则在将连接提供给客户端时，使用 validate-connection-statement 属性指定的 SQL 语句验证与数据库的连接。 | False |
| validate-connection-statement | 当连接从连接池提供给客户端时运行的语句。 | None |
| num-cached-statements | 指定为连接缓存的语句的最大数量。值大于 0 可启用语句缓存。 | 0 |
| time-to-live-timeout | 默认情况下未启用，此元素指定已使用连接在关闭并返回连接池之前可保持活动状态的最长时间（以秒为单位）。当连接关闭时，语句句柄也会关闭。 | -1 |
| abandoned-connection-timeout | 仅在 Oracle 数据库中受支持且默认情况下未启用，此元素指定已使用连接的非活动超时。 | -1 |
| disable-server-connection-pooling | 禁用应用服务器的连接池。该元素与在驱动程序内部支持连接池的 JDBC 驱动程序一起使用。使用隐式连接缓存的 Oracle JDBC 驱动程序时，其值不使用。 | False |
| property-check-interval | 仅与 Oracle 数据库一起使用。此元素指定在应用超时之前的时间间隔（以秒为单位）。 | 900 |
| lower-threshold-limit | 连接限制的下限阈值。默认为 20%。 | 20% |

如果使用原生数据源，则连接缓存主要以编程方式执行。OracleDataSource 使用方法 getConnectionCacheName()、setConnectionCacheName(String cacheName)、getConnectionCacheProperties() 和 setConnectionCacheProperties(java.util.Properties cp) 提供连接缓存。或者，可以使用连接缓存管理器类 OracleConnectionCacheManager 来指定连接缓存。OracleConnectionCacheManager 类提供 getCacheManagerInstance()、createCache(String cacheName, javax.sql.DataSource ds, Properties cacheProperties)、

`createCache(javax.sql.DataSource ds, Properties cacheProperties)`、`removeCache(String cacheName, int mode)`、`refreshCache(String cacheName, int mode)` 和 `getCacheProperties(String cacheName)` 方法来配置连接缓存。对于原生数据源，还可以在 `data-sources.xml` 文件中的 `native-data-source` 元素中使用 `property` 元素以声明方式指定连接缓存。可以在原生数据源中指定的连接缓存属性在以下表格中讨论：

| 连接缓存属性 | 类型 | 描述 |
|---|---|---|
| connectionCacheName | String | 缓存名称。 |
| connectionCachingEnabled | Boolean | 指定是否启用隐式连接缓存。 |
| fastConnectionFailoverEnabled | Boolean | 指定是否启用快速连接故障转移。要启用快速连接故障转移，还应启用隐式连接缓存。快速连接故障转移提供连接缓存中可用连接的负载均衡。此外，使用快速连接故障转移时，无效连接将被关闭。 |

受管数据源是首选数据源，因为连接池由 OC4J 服务器实现，开发人员无需实现连接池。在下一节中，我们将开发一个 Web 应用程序以从 MySQL 数据库检索数据。删除本节中配置的受管数据源 MySQLDataSource 和连接池 MySQLConnectionPool，方法是选择 **工具 | 嵌入式 OC4J 服务器首选项** 窗口中的受管数据源/连接池，然后选择 **删除** 按钮。使用 **立即刷新** 按钮更新 `data-sources.xml` 配置文件

## 从 Web 应用程序连接数据库

如果使用 Java 应用程序连接数据库，可以使用 `DriverManager` 类的 `getConnection()` 方法获取 JDBC 连接。如果要在 Web 应用程序中获取 JDBC 连接，则必须在应用程序服务器中配置具有 JNDI 命名服务绑定的数据源，例如嵌入在 JDeveloper IDE 中的 OC4J。配置在 `data-sources.xml` 中的数据源对象可在 JDeveloper 中的 Servlet 或 JSP 中使用。在本节中，我们将使用上一节中配置的受管数据源连接到 MySQL 数据库。首先，我们需要在 JDeveloper 中使用 **文件 | 新建** 创建一个项目。在 **新建库** 窗口中，在类别中选择 **常规 | 应用程序**，并在

