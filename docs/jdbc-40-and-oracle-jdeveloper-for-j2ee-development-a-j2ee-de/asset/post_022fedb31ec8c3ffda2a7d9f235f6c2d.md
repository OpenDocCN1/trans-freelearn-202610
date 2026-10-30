
the database. An XML document representation of the data in a Web RowSet may be obtained for data exchange. In this chapter the Web RowSet feature in Oracle 10g database JDBC driver is implemented in JDeveloper 10g. An example Web RowSet will be created from a database. The Web RowSet will be modified and stored in the database table.

In this chapter we will learn the following:

- Creating a Oracle Web RowSet object
- Adding a row to Oracle Web RowSet
- Reading a row from Oracle Web RowSet
- Updating a row in Oracle Web RowSet
- Deleting a row from Oracle Web RowSet
- Updating Database Table with modified Oracle Web RowSet

## Setting the Environment

We will use Oracle database to generate an updatable OracleWebRowSet object. Therefore, install Oracle database 10g including the sample schemas. Connect to the database with the OE schema:

```
SQL> CONNECT OE/<password>
```

Create an example database table, Catalog, with the following SQL script:

```
CREATE TABLE OE.Catalog(Journal VARCHAR(25), Publisher Varchar(25), Edition VARCHAR(25), Title Varchar(45), Author Varchar(25));
INSERT INTO OE.Catalog VALUES('Oracle Magazine', 'Oracle Publishing', 'July-August 2005', 'Tuning Undo Tablespace', 'Kimberly Floss');
INSERT INTO OE.Catalog VALUES('Oracle Magazine', 'Oracle Publishing', 'March-April 2005', 'Starting with Oracle ADF', 'Steve Muench');
```

Configure JDeveloper 10g for Web RowSet implementation. Create a project in JDeveloper. Select File | New | General | Application. In the Create Application window specify an Application Name and click on Next. In the Create Project window specify a Project Name and click on Next. A project is added in the Applications Navigator.

![](img/553e44bc30e70d0d8e58278a7ece8da8_243_0.png)

Next, we will set the project libraries. Select **Tools | Project Properties** and in the **Project Properties** window select **Libraries | Add Library** to add a library. Add the **Oracle JDBC** library to project libraries. If the Oracle JDBC drivers version prior to the Oracle database 10g (R2) JDBC drivers version is used, create a library from the Oracle Web RowSet implementation classes JAR file, `C:\JDeveloper10.1.3\jdbc\lib\ocrs12.jar`. The `ocrs12.jar` is required only for JDBC drivers prior to Oracle database 10g (R2) JDBC drivers. In Oracle database 10g (R2) JDBC drivers Oracle RowSet implementation classes are packaged in the `ojdbc14.jar`. In Oracle database 11g JDBC drivers Oracle RowSet implementation classes are packaged in `ojdbc5.jar` and `ojdbc6.jar`.

In the **Add Library** window select the **User** node and click on **New**. In the **Create Library** window specify a **Library Name**, select the **Class Path** node and click on **Add Entry**. Add an entry for `ocrs12.jar`. As Web RowSet was introduced in J2SE 5.0, if J2SE 1.4 is being used we also need to add an entry for the RowSet implementations JAR file, `rowset.jar`. Download the JDBC RowSet Implementations 1.0.1 zip file, `jdbc_rowset_tiger-1_0_1-mrel-ri.zip`, from `http://java.sun.com/products/jdbc/download.html#rowset1_0_1` and extract the JDBC RowSet zip file to a directory. Click on **OK** in the **Create Library** window. Click on **OK** in the **Add Library** window. A library for the Web RowSet application is added.

![](img/553e44bc30e70d0d8e58278a7ece8da8_243_1.png)

Now configure an OC4J data source. Select Tools | Embedded OC4J Server Preferences. A data source may be configured globally or for the current workspace. If a global data source is created using Global | Data Sources, the data source is configured in the C:\JDeveloper10.1.3\jdev\system\oracle.j2ee.10.1.3.36.73\embedded-oc4j\config \data-sources.xml file. If a data source is configured for the current workspace using Current Workspace | Data Sources, the data source is configured in the data-sources.xml file. For example, the data source file for the WebRowSetApp application is WebRowSetApp-data-sources.xml. In the Embedded OC4J Server Preferences window configure either a global data source or a data source in the current workspace with the procedure discussed in Chapter 2. A global data source definition is available to all applications deployed in the OC4J server instance. A managed-data-source element is added to the data-sources.xml file.

```
<managed-data-source name='OracleDataSource' connection-pool-name='Oracle Connection Pool' jndi-name='jdbc/OracleDataSource'/>
<connection-pool name='Oracle Connection Pool'>
  <connection-factory factory-class='oracle.jdbc.pool.OracleDataSource' user='OE' password='pw' url="jdbc:oracle:thin:@localhost:1521:ORCL">
  </connection-factory>
</connection-pool>
```

Add a JSP, GenerateWebRowSet.jsp, to the WebRowSet project. Select File | New | Web Tier | JSP | JSP. Click on OK. Select J2EE 1.3 or J2EE 1.4 in the Web Application window and click on Next. In the JSP File window specify a File Name and click on Next. Select the default settings in the Error Page Options page and click on Next. Select the default settings in the Tag Libraries window and click on Next. Select the default options in the HTML Options window and click on Next. Click on Finish in the Finish window. Next, configure the web.xml deployment descriptor to include a reference to the data source resource configured in the data-sources.xml file as shown in following listing:

```
<resource-ref>
  <res-ref-name>jdbc/OracleDataSource</res-ref-name>
  <res-type>javax.sql.DataSource</res-type>
  <res-auth>Container</res-auth>
</resource-ref>
```

![](img/553e44bc30e70d0d8e58278a7ece8da8_245_0.png)

## Creating a Web RowSet

In this section we will create a Web RowSet from a database table and an XML document representation of the Web RowSet is generated. Create a Java class in JDeveloper with File | New | General | Java Class. In the Create Java Class window specify the class name, WebRowSetQuery, and package name and click on OK. A Java class, WebRowSetQuery.java gets added to the WebRowSet project. In the Java application first import the oracle.jdbc.rowset package classes. Create an OracleWebRowSet class object:

```
OracleWebRowSet webRowSet=new OracleWebRowSet();
```

Set the data source name to obtain a JDBC connection with the database. The data source name is configured in the data-sources.xml file:

```
webRowSet.setDataSourceName("jdbc/OracleDataSource");
```

Set the SQL query command for the OracleWebRowSet class object:

```
webRowSet.setCommand(selectQuery);
```

Variable selectQuery is the String value for the SQL statement that is to be used to query the database. SQL statement value is obtained from an input field in a JSP. Set the username and password to obtain a JDBC connection:

```
webRowSet.setUsername("OE");
webRowSet.setPassword("<password>");
```

Set the read only, fetch size, and max rows attributes of the OracleWebRowSet object:

```
webRowSet.setReadOnly(false);
webRowSet.setFetchSize(5);
webRowSet.setMaxRows(3);
```

Run the SQL command specified in the setCommand() method with the execute() method:

```
webRowSet.execute();
```

A Web RowSet is created consisting of the data retrieved from the database table with the SQL query. Generate an XML document from the Web RowSet using the writeXml() method;

```
OutputStreamWriter output=new OutputStreamWriter( new
FileOutputStream(new File("c:/output/output.xml")));
webRowSet.writeXml(output);
```

Oracle Web RowSet also provides readXml() methods to read an Oracle Web RowSet object in XML format using a Reader object or an InputStream object. If the readXml() methods are to be used set one of the following JAXP system properties:

- javax.xml.parsers.SAXParserFactory
- javax.xml.parsers.DocumentBuilderFactory

For example, set the SAXParserFactory property as follows:

```
System.setProperty("javax.xml.parsers.SAXParserFactory",
"oracle.xml.jaxp.JXSAXParserFactory");
```

WebRowSetQuery.java also has methods to read, update, delete, and insert a row in the database table, which will be discussed in the subsequent sections. WebRowSetQuery.java application is listed below:

```
package webrowset;
import oracle.jdbc.rowset.*;
import java.io.*;
import java.sql.SQLException;
public class WebRowSetQuery
{
```
