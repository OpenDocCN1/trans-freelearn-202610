This <application-policy/> element configures the JBoss application server with the DB2 database. Additional modifications would be required, if the DB2 database is to be accessed from a CMP entity EJB. Modify the standardjbosscmp-jdbc.xml or jbosscmp-jdbc.xml as follows:

```
<jboss cmp-jdbc>
    <defaults>
        <datasource>java:/DB2DS</datasource>
        <datasource-mapping>DB2</datasource-mapping>
    </defaults>
</jboss cmp-jdbc>
```

We also have to modify standardjaws.xml or jaws.xml to set <datasource> and <type-mapping>.

```
<jaws>
    <datasource>java:/DB2DS</datasource>
    <type-mapping>DB2</type-mapping>
</jaws>
```

The above configuration changes allow us to use DB2 with JBoss.

# Configuring JBoss Server with SQL Server

SQL Server is a comprehensive data management and analysis solution to build, deploy, and manage enterprise applications. In this section, we will configure JBoss application server with SQL Server 2000 and SQL Server 2005. Copy the SQL Server JDBC 4.0 JAR file to JBoss server as we did with the other databases. I-net software's Merlia JDBC drivers support JDBC 4.0, where the Merlia driver ZIP file Merlia_Trial_7.02.zip may be obtained from http://www.inetsoftware.de/products/jdbc/mssql/merlia/. Extract the Merlia ZIP file to a directory, add Merlia.jar to the JBoss's installation /server/default/lib directory, and set the JAVA_HOME environment variable to JDK 6.0. We also have to copy the SQL Server data source file to the JBoss server deploy directory. Copy docs\examples\jca\mssql-ds.xml for non-transactional data source, or \docs\examples\jca\mssql-xa-ds.xml for transactional data source to the \server\default\deploy directory.

Modify the mssql-ds.xml configuration file to set the driver class and connection URL, and specify <driver-class/> as com.inet.tds.TdsDriver. For SQL Server 2000, specify <connection-url/> as jdbc:inetdae7:localhost:1433; for SQL Server 2005, specify <connection-url/> as jdbc:inetdae7:localhost:port. The variable port number for SQL Server 2005 is obtained from the SQL Server