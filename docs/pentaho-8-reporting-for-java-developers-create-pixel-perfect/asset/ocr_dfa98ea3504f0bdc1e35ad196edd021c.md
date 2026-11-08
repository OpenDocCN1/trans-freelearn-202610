# JndiConnectionProvider

The third available `ConnectionProvider` is `JndiConnectionProvider`. `JndiConnectionProvider` uses **Java's Naming and Directory Interface (JNDI)** to obtain a connection to a database. It has a default constructor, along with the following setter methods, that may be configured to connect to a JNDI data source. Only the connection path property is mandatory.

In the following table, some of the most relevant methods are introduced:

| Method | Description |
|---|---|
| `void setConnectionPath(String connectionPath)` | The `setConnectionPath` method should be set to the full JNDI path of your JDBC data source |
| `void setUsername(String username)` | The `setUsername` method is an optional field, which may be required when connecting to your data source |
| `void setPassword(String password)` | The `setPassword` method is an optional field, which may be required when connecting to your data source |

JNDI is available in many application servers. It may also be used in a regular Java application with the help of simple JNDI, available at http://www.osjava.org/simple-jndi. The following list shows the examples of JNDI connection strings in different environments, assuming that you've already configured the JNDI data sources in that environment:

+   - **Tomcat java:** `comp/env/jdbc/<DataSourceName>`
+   - **JBoss java:** `<DataSourceName>`