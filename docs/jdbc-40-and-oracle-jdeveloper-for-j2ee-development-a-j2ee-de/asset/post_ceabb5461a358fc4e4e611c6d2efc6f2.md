
Categorization of the SQLExceptions facilitates the portability of error handling code. SQLTransientException and SQLNonTransientException classes have subclasses, which map to common SQLState class values. SQLState class provides JDBC application's return code information about the most recently executed SQL statement. The return code is sent by the database manager after the completion of each SQL statement. The SQLState class values are defined in the SQL: 2003 specification.

A SQLTransientException indicates that the operation that generates the exception could succeed, if retried. Subclasses of the SQLTransientException class are discussed in following table:

| SQL Exception | SQLState Class Value | Description |
|---|---|---|
| SQLTransientConnectionException | 08 | Represents that a connection operation that failed could succeed, if the operation is retried. |
| SQLTransactionRollbackException | 40 | Represents that a current Statement was rolled back. |
| SQLTimeoutException | Does not correspond to a standard SQLState. | Represents that a Statement has timed out. |

SQLNonTransientException indicates the operation, which generates the exception that will not succeed without the cause of the SQLException being rectified. Subclasses of the SQLNonTransientException are discussed in following table:

| SQL Exception | SQLState Class Value | Description |
|---|---|---|
| SQLFeatureNotSupportedException | 0A | Represents that a JDBC driver does not support a feature. |
| SQLNonTransientConnectionException | 08 | Represents that a connection operation that failed will not succeed if retried, without the cause of the exception being corrected. |
| SQLDataException | 22 | Represents various data errors including non allowable conversion and division by 0. |
| SQLIntegrityConstraintViolationException | 23 | Represents an integrity constraint exception. |
| SQLInvalidAuthorizationSpecException | 28 | Represents an authorization exception. |
| SQLSyntaxErrorException | 42 | Represents an error in the SQL syntax. |

The SQLRecoverableException indicates that the operation that throws the Exception can succeed, if the application performs some recovery steps and retries the entire transaction (or the transaction branch in the case of a distributed transaction). The recovery steps include at the least, closing the current connection and obtaining a new connection.

A new subclass of the SQLException class, SQLClientInfoException, has been added in the JDBC 4.0 specification. The SQLClientInfoException is generated, if one or more client info properties could not be set on a Connection. The SQLClientInfoException also lists the client info properties, which were not set. Some databases that do not allow multiple client info properties to be set atomically can generate the SQLClientInfoException exception after one or more client info properties have been set. The client info properties that were not set can be retrieved by using the getFailedProperties() method.

## Connection Management

Connection pooling improves the performance and scalability of the connections by providing a cache of the connections that are reusable across client sessions. Connection pooling reduces the overhead of opening, initializing and closing connections. One of the drawbacks of the connection pooling is that when a connection in a connection pool becomes stale and unusable, the application performance is reduced. JDBC 3.0 specification did not have the provision to track connection state. Connection state tracking has been added to the Connection interface in the JDBC 4.0 to find out if a connection is valid. The isValid(int timeout) method returns true, if the connection is valid. The isValid() method validates a connection with a SQL query, or another mechanism. If a connection is not valid, the connection can be closed, thus reducing the accumulation of unusable connections. The Connection object conn can be closed, if it is not in use:

```
if (!conn.isClosed())
if (!conn.isValid())
conn.close();
```

Connection state tracking and closing of invalid connections are implemented by the connection pool manager. Another drawback of connection pooling has been that one or more connections assigned from a connection pool in a web or application server can bog down an application. JDBC 3.0 does not have the provision to identify the connections that use the excess of CPU time. JDBC 4.0 has added the setClientInfo() and getClientInfo() methods to the Connection interface using which, client specific information can be specified on a Connection object, when a Connection is assigned to an application. Client specification information includes user name and application name. The DatabaseMetaData interface in JDBC 4.0 provides a new method, getClientInfoProperties(). Client info properties supported by a JDBC driver can be obtained using the `getClientInfoProperties()` method. When one or more connections bog down the application, the `getClientInfo()` method can be used to identify which connections could be causing the reduction in performance. Some standard client info properties that a JDBC driver can support are discussed in the following table:

| Client Info Property | Description |
|---|---|
| ApplicationName | The name of the application that is using the connection. |
| ClientUser | The name of the user. |
| ClientHostname | The hostname on which the application is running. |

Similar to connection pooling, JDBC 4.0 provides `Statement` pooling to reduce the overheads of opening, initiating, and closing `Statement` objects. Frequently used `Statement` objects can be pooled using the `setPoolable(boolean poolable)` method. The `isPoolable()` method is used to check if a `Statement` object is poolable. The `Statement` object, `stmt` can be pooled, if poolable:

```
if(stmt.isPoolable())
stmt.setPoolable(true);
```

## Scalar Functions

Most databases support numeric, string, time, date, system, and conversion functions on the scalar values. SQL statements run using the `Statement` object, and can include the scalar functions using the JDBC escape syntax. JDBC 4.0 provides some new scalar functions, which are discussed in the following table:

| Scalar Function | Description |
|---|---|
| CHAR_LENGTH | Returns the length of a string expression. |
| CHARACTER_LENGTH |  |
| CURRENT_DATE | Returns the current date. |
| CURRENT_TIME | Returns the current time. |
| CURRENT_TIMESTAMP | Returns the current timestamp. |
| EXTRACT | Extracts a field from a datetime value. |
| OCTET_LENGTH | Returns the length of a string expression in octets (bytes). |

## JDBC 4.0 support in Oracle Database

Support for JDBC 4.0 specification is a JDBC driver feature, and not a database feature. Oracle Database 11g JDBC drivers support JDBC 4.0 specification. Add the ojdbc6.jar file to the CLASSPATH environment variable to use the JDBC 4.0 features. JDK 6.0 is required for JDBC 4.0 support. Oracle database 11g JDBC drivers can be used with the Oracle database 9i and the later versions. Oracle database 11g JDBC drivers support all the JDBC 4.0 features except the SQLXML Java data type that is used to access the SQL data type XML. Oracle database 11g JDBC drivers support the wrapper pattern to access non-standard Oracle JDBC resources. Oracle extensions to the JDBC are available in the oracle.jdbc package.

The oracle.jdbc.OracleStatement interface can be unwrapped using the unwrap() method to create a oracle.jdbc.OracleStatement object. As the Statement interface extends the Wrapper interface, create a Statement object from a Connection object, conn. Check if the Statement object is a wrapper for the oracle.jdbc.OracleStatement interface using the isWrapperFor() method. Obtain a OracleStatement object from the interface using the unwrap() method to use the methods of the OracleStatement interface:

```
Statement stmt = conn.createStatement();
Class class = Class.forName("oracle.jdbc.OracleStatement");
if(stmt.isWrapperFor(class))
{
OracleStatement oracleStmt = (OracleStatement)stmt.unwrap(class);
oracleStmt.defineColumnType(1, Types.NUMBER);
}
```

Oracle database 11g JDBC drivers support the enhanced chained exceptions in the JDBC 4.0 SQLException class. JDBC 4.0 has added a distinction between the permanent errors and transient errors. Permanent errors are errors that occur in the correct operation of the database system and continue to recur, until the cause of the error is removed. Transient errors are errors occurring due to the failure of some segment of the system, or due to timeouts, and these may not recur if the operation that generated the error is retried. Oracle database 11g JDBC drivers support the different categories of SQLException.

Oracle database 11g JDBC drivers support the ROWID SQL data type. Each table in an Oracle database has a ROWID pseudocolumn that identifies a row in a table. The SQL data type of the ROWID column is ROWID. Usually a rowid uniquely identifies a row in a database. But rows in different tables that are stored in a cluster may have the same rowid. Rowids should not be used as the primary key for a database table. If a row is deleted and reinserted using an Import or Export utility, its rowid may get modified. If a row is deleted, its rowid can be assigned to a row added later. The ROWID pseudocolumn can be used in the SELECT and WHERE clauses. Rowid values have the following applications:
