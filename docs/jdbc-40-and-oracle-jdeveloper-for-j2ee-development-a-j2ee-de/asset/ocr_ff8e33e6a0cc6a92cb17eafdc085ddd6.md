A connection MySQLConnection in the Connections navigator is available as a data source with the JNDI Name binding jdbc/MySQLConnectionDS. To obtain a connection from the data source, add a resource-ref element to the web application in which a connection is to be obtained. In a Servlet or JSP application, a connection may be obtained with the data source JNDI Name.

```
InitialContext initialContext = new InitialContext();
javax.sql.DataSource ds = (javax.sql.DataSource)
initialContext.lookup("java:comp/env/jdbc/MySQLConnectionDS");
java.sql.Connection conn = ds.getConnection();
```

JavaEE 5 defines annotations to support resource injection. Resource injection is the injection of external resources, such as a data source in a JEE 5 application using the javax.annotation.Resource annotation. JDeveloper 11 will support resource injection with annotations to obtain a handle of a data source. For example, define a catalogDS resource of the javax.sql.DataSource type, as shown below:

```
private @Resource DataSource catalogDS;
```

The catalogDS field of type javax.sql.DataSource is annotated with the @Resource annotation. JNDI lookup is not required with resource injection, and the DataSource resource is also not defined in the web.xml deployment descriptor.

# JDBC Configuration Errors

You might get errors while configuring a JDBC connection. If you are using MySQL, and the connection URL is incorrect, or the MySQL database is not running, the following error message is generated:

```
Communications link failure
```

If you are using Oracle database, some possible connection configuration errors are listed below:

```
IO exception: The Network Adapter could not establish the connection
IO exception: Connection refused
```

The The Network Adapter could not establish the connection exception is caused by one or more of the following configuration errors:

+   1. The database host name, port number, or database instance name is wrong.
+   2. The database TNSListener has not been started. The TNSListener may be started with the lsnrctl utility.

```
C:\>lsnrctl start
```