

要在 Anypoint Monitoring 中启用日志，您需要 Titanium 订阅。Anypoint Monitoring 会聚合日志文件，随后您可以对其进行管理、搜索、筛选或分析。日志可按应用程序、环境、日志级别、环境类型、类、时间戳、工作器等进行筛选。Anypoint Monitoring 允许您以表格或 JSON 格式查看日志，并且您可以使用高级搜索查询来搜索日志。

## 了解部署到 CloudHub 的 MuleSoft 应用程序的日志记录

每当应用程序部署到 CloudHub 时，它都会提供部署日志，包括该应用程序的系统日志和工作器日志。日志将保留 30 天（约 4 周半）或 100 MB 数据，以先达到的限制为准：

![图 14.30 - CloudHub 部署日志](image/B19922_14_30.jpg)

图 14.30 - CloudHub 部署日志

如前图所示，CloudHub 提供了下载 `Logs`（日志）、`Diagnostics`（诊断）和 `Mule Logs`（Mule 日志）的选项：

![图 14.31 - CloudHub 日志搜索](image/B19922_14_31.jpg)

图 14.31 - CloudHub 日志搜索

图 14.31 显示日志可以根据其周期和优先级进行搜索。

### 将 CloudHub 应用程序日志发送到外部系统

部署到 CloudHub 的应用程序可以使用 `log4j2.xml` 与 ELK 或 Splunk 等外部日志记录系统集成。如果我们想将日志发送到外部系统，需要禁用 CloudHub 日志设置。默认情况下，您将看不到禁用 CloudHub 日志的选项。要使其在 CloudHub 的应用程序设置中可见，您需要联系 MuleSoft 支持团队：

![图 14.32 - 在 CloudHub 上禁用 CloudHub 日志](image/B19922_14_32.jpg)

图 14.32 - 在 CloudHub 上禁用 CloudHub 日志

一旦该选项如图 14.32 所示变得可见，您可以勾选 `Disable CloudHub logs`（禁用 CloudHub 日志）以将日志发送到 Splunk 和 ELK 等外部系统。请注意，您还需要向 `log4j2.xml` 文件添加一个 appender（附加器）以将日志发送到外部系统。

要将日志发送到 Splunk，您可以向部署到 CloudHub 的应用程序的 `log4j2.xml` 文件添加 Splunk 或 HTTP appender：

这是 Splunk appender：

```xml
<SplunkHttp name="SPLUNK-CLOUD" source="mule-cloudhub-app" url="http://localhost:8088/"
            token="b3a34eabd-b5f4-471431-97d2-e5b234af5bae9e"            index="main"
            disableCertificateValidation="true">
            <PatternLayout pattern="[%d{MM-dd HH:mm:ss}] %-5p %c{1}            [%t]: %m%n" />
</SplunkHttp>
```

这是 HTTP appender：

```xml
<Http name="Splunk" url="http://localhost:8088/services/collector/raw">
  <Property name="Authorization" value="Splunk eert8e102-5f55-40ad-   a047-b55a82245419"></Property>
  <PatternLayout pattern="%-5p %d [%t] [event: %X{correlationId}] %c:  %m%n"></PatternLayout>
</Http>
```

要将日志发送到 ELK，您可以向部署到 CloudHub 的应用程序的 `log4j2.xml` 文件添加 socket appender。

这是 socket appender：

```xml
<Socket name="socket" host="${sys:elk.host}" port="${sys:elk.port}" protocol="TCP">
  <PatternLayout pattern="%-5p %d [%t] [event: %X{correlationId}] %c:  %m%n"></PatternLayout>
</Socket>
```

如果您禁用了 CloudHub 日志但仍可将日志发送到 CloudHub，请在 `log4j2.xml` 中使用 `Log4J2CloudhubLogAppender`。`Log4J2CloudhubLogAppender` 将日志数据发送到 CloudHub。这是 log4j2 CloudHub appender：

```xml
<Log4J2CloudhubLogAppender name="CLOUDHUB"
            addressProvider="com.mulesoft.ch.logging.            DefaultAggregatorAddressProvider"
            applicationContext="com.mulesoft.ch.logging.            DefaultApplicationContext"
            appendRetryIntervalMs="${sys:logging.appendRetryInterval}"
            appendMaxAttempts="${sys:logging.appendMaxAttempts}"
            batchSendIntervalMs="${sys:logging.batchSendInterval}"
            batchMaxRecords="${sys:logging.batchMaxRecords}"
            memBufferMaxSize="${sys:logging.memBufferMaxSize}"
            journalMaxWriteBatchSize="${sys:logging.            journalMaxBatchSize}"
            journalMaxFileSize="${sys:logging.journalMaxFileSize}"
            clientMaxPacketSize="${sys:logging.clientMaxPacketSize}"
            clientConnectTimeoutMs="${sys:logging.            clientConnectTimeout}"
            clientSocketTimeoutMs="${sys:logging.clientSocketTimeout}"
            serverAddressPollIntervalMs="${sys:logging.            serverAddressPollInterval}"
            serverHeartbeatSendIntervalMs="${sys:logging.            serverHeartbeatSendIntervalMs}"
            statisticsPrintIntervalMs="${sys:logging.            statisticsPrintIntervalMs}">
            <PatternLayout pattern="%-5p %d [%t] [event:            %X{correlationId}] %c: %m%n"/>
</Log4J2CloudhubLogAppender>
```

## 了解部署到 CloudHub 2.0 的 MuleSoft 应用程序的日志记录

每当应用程序部署到 CloudHub 2.0 时，它将提供运行时日志和应用程序日志。在 CloudHub 2.0 中，应用程序日志和运行时日志会被合并。它们可以从 Runtime Manager GUI 下载。

这些日志将保留 30 天（约 4 周半）或 200 MB 数据，以先达到的限制为准。

如果在应用程序设置中未勾选 `Enable application logs`（启用应用程序日志），日志将不会转发到 Anypoint Monitoring 和 Anypoint Runtime Manager：

![图 14.33 - 在 CloudHub 2.0 中启用应用程序日志](image/B19922_14_33.jpg)

图 14.33 - 在 CloudHub 2.0 中启用应用程序日志

部署到 CloudHub 2.0 的应用程序可以通过在应用程序的 `log4j2.xml` 文件中配置相关 appender 来与外部日志记录系统集成，正如本章前面所讨论的那样。

## 了解部署到本地/混合环境的 MuleSoft 应用程序的日志记录

部署到本地 Mule 运行时的应用程序的日志可以在 `MULE_HOME/logs` 文件夹中找到，您可以在其中找到特定于应用程序的日志和运行时日志。也可以通过向 `log4j2.xml` 文件添加适当的 appender 将日志发送到外部系统。

正如我们已经讨论的，我们可以使用 Splunk 和 HTTP appender 将日志发送到 Splunk，并使用 socket appender 将日志发送到 ELK。

每当应用程序部署在混合环境中时，可以配置 Splunk 或 ELK 将事件跟踪或 API 分析发送到 Splunk 或 ELK。这可以在 Runtime Manager 的 Server Plugin（服务器插件）控制台中启用：

![图 14.34 - 为部署在混合环境中的本地应用程序启用日志记录](image/B19922_14_34.jpg)

图 14.34 - 为部署在混合环境中的本地应用程序启用日志记录

图 14.34 显示了 API 分析和事件跟踪，可为混合环境中的 Splunk 和 ELK 启用这些功能：

![图 14.35 - 为混合环境配置 Splunk 事件跟踪](image/B19922_14_35.jpg)

图 14.35 - 为混合环境配置 Splunk 事件跟踪

图 14.35 展示了如何为混合环境配置 Splunk 事件跟踪。

## 了解部署到裸金属系统/VM 上的 Runtime Fabric 的 MuleSoft 应用程序的日志记录

部署到 Runtime Fabric 的应用程序会为应用程序、Runtime Fabric 服务、API 代理和 Kubernetes 服务生成日志。托管在裸金属服务器或 VM 上的 Runtime Fabric 的日志在 OpsCenter 中可见。

您可以将 Runtime Fabric 配置为将日志转发到 Splunk、ELK、CloudWatch、Data Dog 或 Syslog 等外部日志记录系统，也可以配置为将日志转发到 Anypoint Monitoring。要将日志转发到 Anypoint Monitoring，您需要 Titanium 订阅。

可以通过在部署到 Runtime Fabric 的应用程序的 `log4j2.xml` 文件中配置 appender，将日志转发到 Splunk 或 ELK 等外部系统。我们在本章前面讨论了如何在 `log4j2.xml` 文件中配置 appender。

