In the Test frame, click the Test Connection button to test the JDBC connection. Click the Finish button to configure the database connection. A connection node is added to the Connections | Database node in the Applications Navigator. The xsqlConnection is available as a jdbc/xsqlConnectionDS JNDI resource.

![](bbox=[0.345, 0.222, 0.655, 0.438])

A connection may also be configured in embedded OC4J server data-sources.xml configuration file, <JDeveloper10.1.3>\jdev\system\oracle.j2ee.10.1.3.34.12\embedded-oc4j\config\data-sources.xml, declaratively or using the Tools | Embedded OC4J Preferences | Global | Data Sources as explained in Chapter 2. <JDeveloper10.1.3> is the directory in which JDeveloper is installed. Add a managed-data-source element to the data-sources.xml file, specifying the connection configuration for a data source. The managed-data-source element includes the data source class used to obtain a data source object. The managed-data-source element is listed below:

```
<managed-data-source name='XSQLDataSource' connection-pool-name='Oracle Connection Pool' jndi-name='jdbc/XSQLDataSource'/>
<connection-pool name='Oracle Connection Pool'>  <connection-factory
factory-class='oracle.jdbc.pool.OracleDataSource'      user='OE'
password='pw'  url="jdbc:oracle:thin:@<host>:<port>:<SID>"   </
connection-factory>  </connection-pool>
```

<host> is the database host, localhost by default.
<port> is the port number, 1521 by default.
<SID> is the database SID, ORCL by default.
<host>, <port>, and <SID> values may be obtained from the tnsnames.ora file.