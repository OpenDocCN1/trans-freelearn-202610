
To enable logs in Anypoint Monitoring, you require a Titanium subscription. Anypoint Monitoring aggregates the log files, which you can then manage, search, filter, or analyze. Logs can be filtered by application, environment, log level, environment type, class, timestamp, worker, and so on. Anypoint Monitoring allows you to see logs in tabular or JSON format, and you can use advanced search queries to search the logs.

## Understanding logging with MuleSoft applications deployed to CloudHub

Whenever an application is deployed to CloudHub, it will provide deployment logs, including system and worker logs for that application. The logs will be retained for 30 days (about 4 and a half weeks) or 100 MB of data, whichever limit comes first:

![Figure 14.30 - CloudHub deployment logs](image/B19922_14_30.jpg)

Figure 14.30 - CloudHub deployment logs

As shown in the preceding figure, CloudHub provides the option to download `Logs`, `Diagnostics`, and `Mule Logs`:

![Figure 14.31 - CloudHub logs search](image/B19922_14_31.jpg)

Figure 14.31 - CloudHub logs search

Figure 14.31 shows that logs can be searched based on their period and priority.

### Sending CloudHub application logs to an external system

Applications that are deployed to CloudHub can be integrated with external logging systems such as ELK or Splunk using `log4j2.xml`. We need to disable the CloudHub logs settings if we want to send logs to an external system. By default, you will not see the option for disabling CloudHub Logs. To make it visible in the application settings of CloudHub, you need to contact the MuleSoft support team:

![Figure 14.32 - Disabling CloudHub logs on CloudHub](image/B19922_14_32.jpg)

Figure 14.32 - Disabling CloudHub logs on CloudHub

Once the option is visible, as shown in Figure 14.32, you can check `Disable CloudHub logs` to send logs to external systems such as Splunk and ELK. Note that you also need to add an appender to the `log4j2.xml` file to send logs to an external system.

To send logs to Splunk, you can add a Splunk or HTTP appender to the `log4j2.xml` file of the application that’s being deployed to CloudHub:

Here’s the Splunk appender:

```xml
<SplunkHttp name="SPLUNK-CLOUD" source="mule-cloudhub-app" url="http://localhost:8088/"
            token="b3a34eabd-b5f4-471431-97d2-e5b234af5bae9e"            index="main"
            disableCertificateValidation="true">
            <PatternLayout pattern="[%d{MM-dd HH:mm:ss}] %-5p %c{1}            [%t]: %m%n" />
</SplunkHttp>
```

Here’s the HTTP appender:

```xml
<Http name="Splunk" url="http://localhost:8088/services/collector/raw">
  <Property name="Authorization" value="Splunk eert8e102-5f55-40ad-   a047-b55a82245419"></Property>
  <PatternLayout pattern="%-5p %d [%t] [event: %X{correlationId}] %c:  %m%n"></PatternLayout>
</Http>
```

To send logs to ELK, you can add a socket appender to the `log4j2.xml` file of the application that’s being deployed to CloudHub.

Here’s the socket appender:

```xml
<Socket name="socket" host="${sys:elk.host}" port="${sys:elk.port}" protocol="TCP">
  <PatternLayout pattern="%-5p %d [%t] [event: %X{correlationId}] %c:  %m%n"></PatternLayout>
</Socket>
```

If you disable CloudHub logs but can still send logs to CloudHub, use `Log4J2CloudhubLogAppender` in `log4j2.xml`. `Log4J2CloudhubLogAppender` sends log data to CloudHub. Here’s the log4j2 CloudHub appender:

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

## Understanding logging with MuleSoft applications deployed to CloudHub 2.0

Whenever an application is deployed to CloudHub 2.0, it will provide runtime and application logs. In CloudHub 2.0, application and runtime logs are merged. They can be downloaded from the Runtime Manager GUI.

These logs are retained for 30 days (about 4 and a half weeks) or 200 MB of data, whichever limit comes first.

If `Enable application logs` is unchecked in the settings of the application, logs will not be forwarded to Anypoint Monitoring and Anypoint Runtime Manager:

![Figure 14.33 - Enable application logs in CloudHub 2.0](image/B19922_14_33.jpg)

Figure 14.33 - Enable application logs in CloudHub 2.0

Applications that are deployed to CloudHub 2.0 can be integrated with an external logging system by configuring the relevant appender in the `log4j2.xml` file of the application, as discussed earlier in this chapter.

## Understanding logging with MuleSoft applications deployed to on-premises/hybrid environments

Logs for applications that are deployed to an on-premises Mule runtime can be found in the `MULE_HOME/logs` folder, where you can find application-specific logs and runtime logs. It is also possible to send logs to external systems by adding the appropriate appender to the `log4j2.xml` file.

As we have already discussed, we can use Splunk and an HTTP appender to send logs to Splunk, and a socket appender for sending logs to ELK.

Whenever an application is deployed in a hybrid environment, it is possible to configure Splunk or ELK to send event tracking or API analytics to Splunk or ELK. This can be enabled in the Server Plugin console in Runtime Manager:

![Figure 14.34 - Enabling logging for an application that’s been deployed on-premises in a hybrid environment](image/B19922_14_34.jpg)

Figure 14.34 - Enabling logging for an application that’s been deployed on-premises in a hybrid environment

Figure 14.34 shows API analytics and event tracking, which can be enabled for Splunk and ELK for the hybrid environment:

![Figure 14.35 - Configuring Splunk event tracking for a hybrid environment](image/B19922_14_35.jpg)

Figure 14.35 - Configuring Splunk event tracking for a hybrid environment

Figure 14.35 shows how to configure Splunk event tracking for a hybrid environment.

## Understanding logging with MuleSoft applications deployed to Runtime Fabric on bare-metal systems/VMs

Applications deployed to Runtime Fabric generate logs for applications, Runtime Fabric services, API proxies, and Kubernetes services. Logs for Runtime Fabric hosted on bare-metal servers or VMs are visible in OpsCenter.

You can configure Runtime Fabric to forward logs to an external logging system such as Splunk, ELK, CloudWatch, Data Dog, or Syslog, and it can be configured to forward logs to Anypoint Monitoring. To forward logs to Anypoint Monitoring, you require a Titanium subscription.

Logs can be forwarded to external systems such as Splunk or ELK by configuring an appender in the `log4j2.xml` file of the application that’s deployed to Runtime Fabric. We discussed how to configure the appender in the `log4j2.xml` file earlier in this chapter.
