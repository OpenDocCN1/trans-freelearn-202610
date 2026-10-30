The *Connection refused* exception is caused by one or more of the following configuration errors:

+   1. The database **SID** specified is incorrect.
+   2. The database instance has not been started. To start the database instance connect to SQL*Plus as SYSDBA.

```
C:\>sqlplus SYS/<pwd> AS SYSDBA
```

At the SQL prompt, start the database instance with the startup command.

```
SQL>startup
```

# Creating a Managed Data Source

In the previous section, a JDBC connection was obtained in the **Connections** navigator. A corresponding JNDI managed data source becomes available for the **Connections** navigator connection. A data source object is configured with a JNDI Name binding in the OC4J server integrated with JDeveloper. A data source may also be configured in the **Embedded OC4J Server Preferences** window directly, or may be configured declaratively by modifying the `data-sources.xml` file. We will discuss each of these methods for creating a data source. A data source may be configured at the **Global** level or the Current Workspace level. A **Global** data source is available to all applications while a Current Workspace data source is available only in the current workspace. To create a new **Global** data source, select **Tools | Embedded OC4J Preferences** and select the **Global | Data Sources** node, and click on the **New** button.

![](bbox=[0.268, 0.575, 0.73, 0.825])