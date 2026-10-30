
```
%> <tr>
<td><%= out.println(row.getAttribute(i++).toString()); %></td>
<td><%=out.println(row.getAttribute(i++).toString()); %></td>
<td><%=out.println(row.getAttribute(i++).toString()); %></td>
<td><%=out.println(row.getAttribute(i++).toString()); %></td>
<td><%=out.println(row.getAttribute(i++).toString()); %></td>
<td><%= out.println(row.getAttribute(i++).toString()); %></td>
<td><%=out.println(row.getAttribute(i).toString()); %></td>
</tr>
<%
    row = vo.next();
} %>
</table>
<%
    Configuration.releaseRootApplicationModule(applicationModule,
        true);
%>
</body>
</html>
```

In JSP TestClient.jsp the application module connects with the database with a JDBC URL. The application module may also be connected to the database with a data source. For a JDBC connection DBConnection1 in the Connections-Navigator a data source with JNDI name jdbc/DBConnection1DS is available. An application module is connected to a database with a data source with the connectToDataSource method of the Transaction interface:

```
transaction.connectToDataSource(null,"jdbc/DBConnection1DS","OE",
    "<password>", false);
```

To run the TestClient.jsp, right-click on the JSP node in the Applications Navigator and select Run. The output of the JSP is displayed in the default browser.

![](img/553e44bc30e70d0d8e58278a7ece8da8_404_0.png)

## JDBC 4.0 Version

ADF Business Components use JDBC to access the database. OC4J server embedded in JDeveloper 10g or JDeveloper does not support JDBC 4.0. The new features in JDBC 4.0 may be availed in a JDeveloper version that supports JDBC 4.0. To use JDBC 4.0 in ADF Business Components configure a database connection with the Oracle database 11g JDBC 4.0 drivers JAR file ojdbc6.jar. We also need to set JDK version to JDK 6.0. To set the JDK version select Tools | Project Properties. In the Project Properties window, select Libraries. In the J2SE Version field, click on Change to select a JDK 6.0 Java executable.

We would be able to use those JDBC 4.0 features that are implemented by the OC4J server. Connection state tracking is implemented by the connection pool manager and is used to track unusable connections. Prior to the new feature of connection state tracking a connection pool manager would typically close all the connections and reinitiate a connection pool if some of the connections in the connection pool became unusable. Using the isValid() method of Connection interface in JDBC 4.0, the connection pool manager is able to determine if a connection is still valid. If a connection is not valid, the connection pool manager closes the connection.

```
if (!connection.isValid())
connection.close();
```

## Summary

The ADF Business Components is a JDBC-based framework. The ADF Business components in JDeveloper may be configured with the Oracle database, as explained in this chapter or a third-party database by configuring a library for the JDBC driver for the database. A web application may be created with the ADF business components using the oracle.jbo package classes and interfaces. In a JDeveloper version that supports JDBC 4.0 we may also add the JDBC 4.0 features.

## 15 Hibernate

Hibernate is an open-source, JDBC-based object/ relational persistence and query service. Hibernate is used to map data representation in a Java object to a database. Hibernate supports several databases including DB2, MySQL, Oracle, and PostgreSQL. With Hibernate, Java data types are mapped to SQL data types. Hibernate generates the required SQL to create, update, and delete database tables. Hibernate is used for generating tables from a Java Bean class and adding, retrieving, updating and deleting data to the tables.

Hibernate is preferred over other database persistence technologies, such as Castor, TopLink, and Entity EJB, because of lesser complexity, greater flexibility, open-source architecture, and support for different databases without the requirement to provide vendor-specific SQL code in the data access layer. Hibernate also provides classes for Ant build tasks, which may be integrated into an Ant build file. In this chapter, we will integrate Hibernate with the JDeveloper 10.1.3 IDE and the Oracle database 10g for developing an object/ relational application.

The database persistence provided by Hibernate is based on a mapping file and a database properties file. The .hbm.xml mapping file consists of class definitions for database persistence, which specify the JavaBean properties to be mapped to a database table, the database schema name, the database table name, and the table columns corresponding to the JavaBean properties. The tables may be generated from the mapping file or prior to developing the Hibernate application.

The mapping file specifies the Java classes for database persistence, the different fields and field types in the Java objects, and the corresponding database table columns and column types. The Java classes for a database application may be generated from the mapping file or before developing the Hibernate application. The database properties file (a hibernate.properties file) specifies the database, the JDBC driver, and the connection URL for the database applications. The Ant task classes net.sf.hibernate.tool.hbm2java.HbmJavaTask and org.hibernate.tool.hbm2ddl.SchemaExportTask are used to map the mapping file to Java classes and database tables respectively. The Java classes are subsequently used to add/retrieve/update/delete data in the database tables. In this chapter, an example mapping file, (`catalog.hbm.xml`), consisting of properties for a journal catalog, is mapped to a database table, `OE.Catalog`, and to a Java class, `Catalog.java`.

## Hibernate and JDBC

Hibernate is a JDBC-based database persistence framework. A Hibernate application's Java to SQL mappings are defined in the `org.hibernate.cfg.Configuration` object. The mappings are compiled from XML mapping files. A XML mapping file is added to a `Configuration` object as follows:

```java
Configuration cfg = new
    Configuration().addResource("Catalog.hbm.xml");
```

Hibernate configuration properties, which include JDBC properties, are set using one of the following methods:

- 1. Set the configuration properties using `setProperties()` method of the `Configuration` object.
- 2. Specify the configuration properties in the `hibernate.properties` file.
- 3. Specify the properties as System properties.
- 4. Specify the properties in the `hibernate.cfg.xml` file.

A `SessionFactory` is used to create and pool connections. A `SessionFactory` is created from a `Configuration` object as follows:

```java
SessionFactory sessionFactory = cfg.buildSessionFactory();
```

When a `Session` object is created a JDBC connection is obtained from the connection pool.

```java
Session session = sessionFactory.openSession();
```

Hibernate creates and pools connections either using `java.sql.DriverManager` or a data source. If connections are to be created and pooled using a `DriverManager`, the JDBC connection properties discussed in the following table need to be specified.

| Property | Description |
|---|---|
| `hibernate.connection.driver_class` | JDBC Driver Class |
| `hibernate.connection.url` | JDBC Connection URL |
| `hibernate.connection.username` | Database User Name |
| `hibernate.connection.password` | Database User Password |
| `hibernate.connection.pool_size` | Optional Property. Maximum number of pooled connections. |
| `hibernate.dialect` | The Dialect for the database. For the Oracle database specify org.hibernate.dialect.OracleDialect |

To develop a Hibernate application in an application server, obtain connections from a data source with a JNDI name binding. The data source properties discussed in the following table configure a Hibernate application with a data source.

| Property | Description |
|---|---|
| hibernate.connection.datasource | Datasource JNDI name |
| hibernate.jndi.url | JNDI provider URL (optional) |
| hibernate.jndi.class | JNDI InitialContextFactory class (optional) |
| hibernate.connection.username | Database Username |
| hibernate.connection.password | Database Password |

JDBC connections obtained from a data source automatically participate in the container managed transactions of the application server. Arbitrary connection properties may be set using the hibernate.connection prefix. For example, connection property hibernate.connection.charSet sets a character set. Some of the other Hibernate, JDBC and connection configuration properties that may be set are discussed in the following table:

| Property | Description |
|---|---|
| hibernate.jdbc.fetch_size | Specifies JDBC Fetch Size |
| hibernate.jdbc.batch_size | Enables Batch Updates |
| hibernate.default_schema | Specifies schema name for unqualified table names. |
| hibernate.jdbc.use_scrollable_resultset | Enables Scrollable ResultSets |
| hibernate.connection.isolation | Specifies JDBC transaction isolation level. |
| hibernate.connection.autocommit | Enables autocommit for JDBC pooled connections. |
