+   - Wrapper pattern
+   - Java SE chained exception facility

Connection state tracking is implemented by the connection pool manager and is used to identify connections that have become unusable in the connection pool. JDBC 4.0 provides the isValid() method in the Connection interface for connection state tracking. If the isValid() method returns false the connection pool manager closes the connection.

```
if (!connection.isValid())
connection.close();
```

Connection client identification is used to identify connections that might cause reduced performance due to excessive CPU usage. Connections are associated with application clients using client info properties. The Connection interface in JDBC 4.0 includes two new method setClientInfo() and getClientInfo() to set and get client info properties. Client info properties supported by a database may be retrieved using the getClientInfoProperties() method of the DatabaseMetaData interface as follows:

```
DatabaseMetaData metaData=connection.getMetaData();
ResultSet rsClientInfo=metadata.getClientInfoProperties();
```

The standard client info properties ApplicationName, ClientUser, and ClientHostname may be set on the Connection object as follows:

```
conn.setClientInfo("ApplicationName","OracleApp");
conn.setClientInfo("ClientUser","OracleUser");
conn.setClientInfo("ClientHostname","OracleHost");
```

PreparedStatement objects are pooled by default if the database supports statement pooling. JDBC 4.0 has also added the provision to pool Statement objects. Frequently used Statement objects are recommended to be pooled to avoid the overhead of closing and opening new Statement objects. Statement pooling is implemented using the isPoolable() and setPoolable() methods of the Statement object as follows:

```
if (stmt.isPoolable())
stmt.setPoolable(true);
```

The Wrapper interface in JDBC 4.0 provides access to nonstandard JDBC methods in vendor-specific extensions to JDBC API using the wrapper pattern. The Wrapper interface is extended by the following JDBC interfaces:

+   - CachedRowSet
+   - CallableStatement