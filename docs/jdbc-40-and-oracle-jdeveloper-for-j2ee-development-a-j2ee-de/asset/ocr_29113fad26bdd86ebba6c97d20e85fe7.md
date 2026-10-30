In JDBC 4.0 specifications, the DriverManager class has added support to getConnection() and getDrivers() methods, for the Java SE (Service Provider) mechanism. By using these methods, JDBC drivers may be loaded automatically. The Class.forName() method is not required to be invoked. Loading drivers using the Java SE Service Provider mechanism will be discussed in the Automatic SQL Driver Loading section.

A JDBC connection is represented by a java.sql.Connection object, and may be obtained from a DriverManager by calling the overloaded static getConnection() methods. The getConnection() method is listed in following table:

| getConnection() Method | Description |
|---|---|
| getConnection(String url ) | Obtains a connection with the specified database URL. |
| getConnection(String url, Properties properties) | Username and password may be specified in the Properties Hashtable. |
| getConnection(String url, String user, String password) | Obtains a connection with a URL username, and password. |

For example, a connection with the Oracle database may be obtained as shown below:

```
String url="jdbc:oracle:thin:@localhost:1521:ORCL";
Connection connection = DriverManager.getConnection(url, "oe", "pw");
```

# Connection Interface

The Connection interface represents a connection with the database. SQL statements may be run in a connection session by using a Statement object. A Connection object is in auto-commit mode, by default. In the auto-commit mode, changes are committed to the database after an SQL statement is executed. The auto-commit mode can be modified by calling the setAutoCommit(boolean autoCommit) method. For example, auto-commit may be set to false:

```
connection.setAutoCommit(false);
```

If auto-commit is set to false, it would be required to commit changes by calling the commit() method:

```
connection.commit();
```

A Connection object can be set to read-only by calling the setReadOnly() method:

```
connection.setReadOnly(true);
```