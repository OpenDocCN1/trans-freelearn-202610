
## Preface

Data retrieval and storage are one of the most common components of J2EE applications. JDBC (Java DataBase Connectivity) is the Java API for accessing a Structured Query Language (SQL) relational database and adding, retrieving, and updating data in the database. JDBC 3.0 specification is the current specification implemented by most application servers. JDBC 4.0 specification became available in December 2006, but is not yet implemented by most application servers such as Oracle Application Server, WebLogic Server, JBoss Application Server, and WebSphere Application Server.

To connect with a SQL relational database, a JDBC driver is required; and most databases provide a JDBC 4.0 driver. We will use the Oracle JDeveloper 10.1.3 IDE to develop most of the J2EE applications in the book. We have chosen JDeveloper, because it includes an embedded J2EE server, the Oracle Containers for J2EE (OC4J) server, to run J2EE applications. JDeveloper also provides built-in support to connect to any of the commonly used application servers such as Oracle Application Server, WebLogic Server, JBoss Application Server, Tomcat server, and WebSphere Application Server and deploy applications to these servers. Another advantage of using JDeveloper is that JDeveloper provides built-in support for JDBC. JDeveloper also provides support for JSF; JSF user-interface components may be selected from a component palette and added to a J2EE application.

## What This Book Covers

In Chapter 1 we discuss the JDBC 4.0 specification. We discuss the commonly used interfaces and classes in the JDBC API. We also discuss the new features in JDBC 4.0 specification.

In Chapter 2 we configure JDBC in JDeveloper IDE. JDeveloper provides a built-in library for the Oracle database and may be configured with third-party databases also. We connect to MySQL database using the JDBC 4.0 driver for MySQL. We also discuss connection pooling and data sources and the new connection pooling and statement pooling features in JDBC 4.0. We run a web application in the embedded OC4J server to connect with the MySQL database and retrieve and display data from the database.

In Chapter 3 we discuss the JavaServer Pages Standard Tag Library (JSTL) SQL tags. JDeveloper 10.1.3 provides a Component Palette for JSTL SQL tags. We create a database table, add data to the database table, update the database table, and query the database table, all with the JSTL SQL tag library.

In Chapter 4, we discuss configuring JDBC in JBoss Application Server, one of the most commonly used open-source J2EE application server. We develop a web application to connect to MySQL database, and retrieve and display data from the database, in JDeveloper and deploy the web application to JBoss server. We also discuss the JDBC 4.0 version of the web application. We configure data sources in JBoss with Oracle database, MySQL database, DB2 database, PostgreSQL database, and SQL Server.

In Chapter 5 we configure JDBC in BEA's WebLogic Server 9.x. WebLogic Server 9 provides some new JDBC features such as additional connection pool properties, which improve data source performance, and support for multi-data sources. We configure a data source with Oracle database. We also develop a web application to retrieve and display data from Oracle database in JDeveloper and deploy the web application to WebLogic server. We also discuss the JDBC 4.0 version of the web application.

In Chapter 6 we configure JDBC in IBM's WebSphere application server. WebSphere has a built-in support to configure a JDBC Provider and data source with commonly used databases. We configure a JDBC Provider and data source with IBM's DB2 UDB database. We also develop a web application in JDeveloper to connect with IBM's DB2 UDB database and retrieve and display data from the database. We run the web application in WebSphere Application Server. We also discuss the JDBC 4.0 version of the web application.

In Chapter 7, we discuss Oracle's XML SQL Utility (XSU) to map an XML document to a database table and map a database table to an XML document. We also update and delete data in the database using the XML SQL Utility. We develop the XSU application in JDeveloper using the JDBC 4.0 driver for Oracle database.

In Chapter 8, we discuss the XSQL Pages Publishing Framework support in JDeveloper 10.1.3. We generate an XML document using an SQL query with XSQL. We also transform the output from an XSQL query using Extensible Stylesheet Language Transformations (XSLT). We also discuss the JDBC 4.0 features that may be availed of in a XSQL application.

In Chapter 9, we discuss Oracle JDBC's implementation of a new feature in JDBC 3.0 specification, Web RowSet. A Web RowSet object is an XML representation of a RowSet, which is a container for ResultSet data that may operate without being connected to the data source. Oracle Web RowSet is Oracle database 10g driver's implementation of Web RowSet. We develop an Oracle Web RowSet web application in JDeveloper to create, read, update, and delete a row in Oracle database. We also discuss the JDBC 4.0 version of the Oracle Web RowSet web application.

In Chapter 10 we create a JSF Data Table from Oracle database in JDeveloper. We display database data in a JSF data table using a static SQL query and a dynamically specified SQL query. We also discuss the JDBC 4.0 version of the JSF application to create a data table.

In Chapter 11, we discuss another JSF UI Component, Panel Grid, to display and update database data. We also use JSF validators and converters to convert and validate input data being stored in a database. We develop the JSF Panel Grid application in JDeveloper and also discuss the JDBC 4.0 version of the Panel Grid application.

In Chapter 12, we develop a PDF report and an Excel spreadsheet report with JasperReports reporting tool using JDBC to retrieve data for the report from a database. We develop the JasperReports web application in JDeveloper and also discuss the JDBC 4.0 version of the web application.

In Chapter 13, we create an Excel spreadsheet from database data using the Apache POI HSSF library. We create the report in JDeveloper using JDeveloper's built-in support to connect with a database. We also discuss the JDBC 4.0 version of the Apache POI application.

In Chapter 14, we discuss Business Component for Java (BC4J) layer of Oracle's Application Developer Framework (ADF). Business Components are based on JDBC and consist of view objects, entity objects, and application modules. We develop a web application using the Oracle ADF Model and Business Components API in JDeveloper.

In Chapter 15, we discuss Hibernate, an object/relational persistence and query service for Java. We create a database table, add data to the table, retrieve data from the table, update data in the table and delete table data, all using Hibernate. We will develop the Hibernate application in JDeveloper.

## Who is This Book for

This book is for J2EE developers. Most J2EE applications have a database component and the book is specially suited for database-based J2EE development.

## Conventions

In this book, you will find a number of styles of text that distinguish between different kinds of information. Here are some examples of these styles, and an explanation of their meaning.

There are three styles for code. Code words in text are shown as follows: "A managed-data-source element is added to the data-sources.xml file."

A block of code will be set as follows:

```
<resource-ref>
  <res-ref-name>jdbc/OracleDataSource</res-ref-name>
  <res-type>javax.sql.DataSource</res-type>
  <res-auth>Container</res-auth>
</resource-ref>
```

Any command-line input and output is written as follows:

```
CREATE TABLE OE.Catalog(Journal VARCHAR(25), Publisher Varchar(25),
  Edition VARCHAR(25), Title Varchar(45), Author Varchar(25));
```

New terms and important words are introduced in a bold-type font. Words that you see on the screen, in menus or dialog boxes for example, appear in our text like this: "In the Create Project window specify a Project Name and click on Next".

![](img/553e44bc30e70d0d8e58278a7ece8da8_14_0.png)

Important notes appear in a box like this.

![](img/553e44bc30e70d0d8e58278a7ece8da8_14_1.png)

Tips and tricks appear like this.

## Reader Feedback

Feedback from our readers is always welcome. Let us know what you think about this book, what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.
