```
}
resultSet.close();
stmt.close();
connection.close();
}
%>
</table>
</body>
</html>
```

# Summary

WebLogic Server 9.x has a new feature, multi data source, which is a group of data sources with a JNDI name binding. A multi data source facilitates the maximum data sources that are available. WebLogic Server 8.1 also had multi data sources called MultiPools. Any pool or multipool is accessed via a DataSource. For 9.0 and the later versions, the DataSource to access the notion of a pool or multipool have been combined. In WebLogic Server 9.x, a connection pool configuration used in WebLogic Server 8.1 has been removed. The data source configuration in WebLogic Server 9.x provides enhanced connection request failover and load balancing between data sources. In a WebLogic server version that supports JDBC 4.0, the new features of JDBC 4.0 such as connection state tracking, Java SE chained facility, and the wrapper pattern to access non-standard resources can be availed.