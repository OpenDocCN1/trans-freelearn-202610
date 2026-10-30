
## Querying a Database Table

In this section, the database table created and modified in the previous sections will be queried, and the result is displayed in a HTML table. Create a JSP, catalogQuery.jsp. Select the JSTL Core 1.1 and JSTL SQL 1.1 tag libraries for the JSP. As in the previous sections, add a setDataSource tag for obtaining a connection with the Oracle database. The SQL taglib tag to query a database is sql:query. Position the cursor in the JSP page, and select Query tag in the SQL Component Palette.

![](img/553e44bc30e70d0d8e58278a7ece8da8_108_0.png)

In the Insert Query window, specify the var attribute, which is a required attribute. The var attribute specifies the variable for the result set returned by the Query tag. Select the Advanced Properties tab.

![](img/553e44bc30e70d0d8e58278a7ece8da8_108_1.png)

Specify the SQL query in the sql attribute field and click on the OK button.

![](img/553e44bc30e70d0d8e58278a7ece8da8_109_0.png)

The sql:query tag gets added to the JSP page:

```
<sql:query  var="catalog" sql="SELECT * FROM CATALOG"/>
```

Add a <table> to display the result set of the Query tag. Add the table headers. A row will be added corresponding to each row in the result set. Select the JSTL 1.1 Core component library and add a ForEach tag to the <table>.

![](img/553e44bc30e70d0d8e58278a7ece8da8_109_1.png)

## Connecting to a Database with JSTL SQL Tag Library

The var attribute of the c:forEach tag specifies the variable corresponding to a row in the result set. The items attribute specifies the EL binding to iterate over the rows in the result set returned by the Query tag. The rows in the result set returned by the sql:query tag are obtained with ${catalog.rows} EL expression. The c:forEach tag in the catalogQuery.jsp JSP is as follows:

```
<c:forEach var="journal" items="${catalog.rows}">
</c:forEach>
```

Next, output the values corresponding to each column in a row to the JSP table. The JSP table values are added with the c:out tag. Add a c:out tag for each column from the JSTL 1.1 Core Component Palette, to the c:forEach tag.

![](img/553e44bc30e70d0d8e58278a7ece8da8_110_0.png)

In the Insert Out window, specify the EL expression to bind a column Value. The column Value bindings are obtained with the column names in the result set. To bind a column Value, click on the Bind button. The journal is the variable corresponding to a row in the result set. For example, the EL expression to bind the CatalogId column is ${journal.CatalogId}. Specify the EL expression in the Expression text area in the Bind to Data window, and click on the OK button.

![](img/553e44bc30e70d0d8e58278a7ece8da8_111_0.png)

Click on the OK button in the Insert Out window.

![](img/553e44bc30e70d0d8e58278a7ece8da8_111_1.png)

Similarly add c:out tags for the other columns in the result set. The catalogQuery.jsp is listed below:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;
charset=windows-1252"/>
<title>catalogQuery</title>
</head>
<body><sql:setDataSource driver="oracle.jdbc.OracleDriver"
url="jdbc:oracle:thin:@localhost:1521:ORCL" user="oe"
password="pw"/><sql:query var="catalog" sql="SELECT * FROM
OE.CATALOG"/>
<table border>
<tr>
    <td><b>CatalogId</b></td>
    <td><b>Journal</b></td>
    <td><b>Publisher</b></td>
    <td><b>Edition</b></td>
    <td><b>Title</b></td>
    <td><b>Author</b></td>
</tr>
<c:forEach var="journal" items="${catalog.rows}">
<tr>
    <td>
        <c:out value="${journal.CatalogId}"/>
    </td>
    <td><c:out value="${journal.Journal}"/></td>
    <td><c:out value="${journal.Publisher}"/></td>
    <td><c:out value="${journal.Edition}"/></td>
    <td><c:out value="${journal.Title}"/></td>
    <td><c:out value="${journal.Author}"/></td>
</tr>
</c:forEach>
</table>
</body>
</html>
```

To run the **catalogQuery.jsp**, right-click on the JSP and select **Run**:

![](img/553e44bc30e70d0d8e58278a7ece8da8_112_0.png)

The result set obtained with the Query tag gets displayed in a HTML table.

| CatalogId | Journal | Publisher | Edition | Title | Author |
|---|---|---|---|---|---|
| catalog1 | Oracle Magazine | Oracle Publishing | Nov-Dec 2004 | Database Resource Manager | Kimberly Floss |
| catalog2 | Oracle Magazine | Oracle Publishing | Nov-Dec 2004 | From ADF UIX to JSF | Jonas Jacobi |
| catalog3 | Oracle Magazine | Oracle Publishing | March-April 2005 | Introduction to ADF | Muench, Steve |

## Summary

The JSTL SQL tag library provides access to a database. JDeveloper 10.1.3 facilitates the development of a JSTL SQL application with a Component Palette for the JSTL SQL 1.1 tags, from which tag library tags may be added to a JSP application. In this chapter, you have learnt to create an Oracle database table using JSTL SQL 1.1 tags in JDeveloper 10.1.3. You have also learnt to update the database table and query the database table using JSTL SQL 1.1 tags.

## 4 Configuring JDBC in JBoss Application Server

JBoss 4.0 is an open-source application server configured to use **Hypersonic Database** (HSQLDB) by default. However, some **Java 2 Platform Enterprise Edition** (J2EE) developers would like to use databases other than Hypersonic Database to develop and deploy applications. The JBoss application server provides data source access for **Enterprise Java Beans** (EJB) persistence, and for J2EE applications. In this chapter, we will see how to configure JBoss to use other databases.

The JBoss 4.0 server makes use of Java Database Connectivity (JDBC) configuration files and data source files to configure the server with a database. To use the server with a database other than the default database, Hypersonic, the JDBC configuration files have to be modified and the data source file for the database has to be made available to the server. Download the JBoss 4.0 application server ZIP file from `http://labs.jboss.com/jbossas/downloads` and install JBoss server by extracting the ZIP file to a directory. In this chapter, you will learn how to do the following:

- Deploy a web application to JBoss Application Server from JDeveloper
- JDBC 4.0 version of the web application deployed to JBoss server
- Configure JBoss server with Oracle database 10g with JDBC 4.0 Driver
- Configure JBoss server with MySQL Database with JDBC 4.0 Driver
- Configure JBoss server with PostgreSQL database server with JDBC 4.0 Driver
- Configure JBoss server with DB2 UDB database with JDBC 4.0 Driver
- Configure JBoss server with MS SQL Server with JDBC 4.0 Driver

## Deploying a Web Application to JBoss from JDeveloper

In this section we will create a web application and deploy the web application to JBoss application server, all from JDeveloper IDE. JDeveloper has a built-in feature to configure a connection with JBoss application server, and deploy a web application to the server. First, create an application in JDeveloper by selecting File | New. In the New Gallery window select General in Categories and Application in Items and click on OK. In the Create Application window, specify an Application Name and click on the OK button. In the Create Project window, specify a project name and click OK. An application and a project get added to the Applications Navigator. Next, to add a JSP select the project node in the Applications Navigator and select File | New. In the New Gallery window select Web Tier | JSP in Categories and JSP in Items and click on OK. The Create JSP Wizard gets started. Now, click on Next. Select J2EE 1.4 in the Web Application window and click on OK. Specify a File Name in the JSP File window, and click on the Next button. Select the default setting in the Error Page Options window and click Next. Click on Next in the Tag Libraries window. Select the default settings in the HTML Options window, and click on Next. Click on Finish in the Finish window. Now, a JSP and web.xml gets added to Applications Navigator.

![](img/553e44bc30e70d0d8e58278a7ece8da8_116_0.png)

Next, create a database table in the MySQL database using the following SQL script:

```
```sql
CREATE TABLE Catalog( CatalogId VARCHAR(25)
PRIMARY KEY, Journal VARCHAR(25), Publisher VARCHAR(25),
    Edition VARCHAR(25), Title Varchar(45), Author Varchar(25));
INSERT INTO Catalog VALUES('catalog1', 'Oracle Magazine',
'Oracle Publishing', 'Nov-Dec 2004', 'Database Resource Manager',
'Kimberly Floss');
INSERT INTO Catalog VALUES('catalog2', 'Oracle Magazine', 'Oracle
Publishing', 'Nov-Dec 2004', 'From ADF UIX to JSP', 'Jonas Jacobi');
```
```
