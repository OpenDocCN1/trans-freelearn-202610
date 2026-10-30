
```
<nullable>1</nullable>
<signed>true</signed>
<searchable>true</searchable>
<column-display-size>25</column-display-size>
<column-label>AUTHOR</column-label>
<column-name>AUTHOR</column-name>
<schema-name></schema-name>
<column-precision>0</column-precision>
<column-scale>0</column-scale>
<table-name></table-name>
<catalog-name></catalog-name>
<column-type>12</column-type>
<column-type-name>VARCHAR2</column-type-name>
</column-definition>
</metadata>
<data>
    <row>
        <col>Oracle Magazine</col>
        <col>Oracle Publishing</col>
        <col>July-August 2005</col>
        <col>Tuning Undo Tablespace</col>
        <col>Kimberly Floss</col>
    </row>
    <row>
        <col>Oracle Magazine</col>
        <col>Oracle Publishing</col>
        <col>March-April 2005</col>
        <col>Starting with Oracle ADF</col>
        <col>Steve Muench</col>
    </row>
</data>
</RowSet>
```

In this section, the procedure to generate a Web RowSet from a database table was explained. In the following section the Web RowSet is modified and the modified data stored in the database table.

## Modifying a Database Table with Web RowSet

With ResultSet interface, to modify the data in the database, a JDBC connection with the database is required to insert, delete, or update a database table row. With a Web RowSet, the data may be modified in the OracleWebRowSet object, and a connection is required only to update the database table with the data in the Web RowSet after all the modifications have been made to the Web RowSet. In this section, the data in the Web RowSet is modified and the database table is updated with the modified Web RowSet. A JDBC connection is not required to modify the data in the example Web RowSet. An OracleWebRowSet object is generated as in the previous section.

Create a JSP, ModifyWebRowSet.jsp, to create and modify a Web RowSet from an SQL query. Also add JSPs CreateRow.jsp, ReadRow.jsp, UpdateRow.jsp, DeleteRow.jsp, and UpdateDatabase.jsp, which are listed later in this chapter. ModifyWebRowSet.jsp, the JSP used to create and modify a Web RowSet is listed as follows:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page session="true"%>
<html>
    <head>
        <title>Modify Database Table with Web RowSet</title>
    </head>
    <body>
        <h3>Modify Database Table with Web RowSet</h3>
        <%webrowset.WebRowSetQuery query=null;%>
        <%String selectQuery=request.getParameter("selectQuery");
        if(selectQuery!=null){
        query=new webrowset.WebRowSetQuery();
        query.generateWebRowSet(selectQuery);
        session.setAttribute("query", query);
        }
        %>
        <form name="query" action="ModifyWebRowSet.jsp" method="post">
            <table>
                <tr>
                    <td>Select Query:</td>
                </tr><tr><td>
                    <textarea name="selectQuery" rows="5"
                    cols="50"></textarea>
```

```
</td></tr><tr><td>
    <input class="Submit" type="submit" value="Apply Query"/>
    </td></tr>
<tr><td><a href="CreateRow.jsp">Create Row</a></td></tr>
<tr><td><a href="ReadRow.jsp">Read Row</a></td></tr>
<tr><td><a href="UpdateRow.jsp">Update Row</a></td></tr>
<tr><td><a href="DeleteRow.jsp">Delete Row</a></td></tr>
<tr><td><a href="UpdateDatabase.jsp">Update Database</a></td></tr>
</table>
</form>
</body>
</html>
```

The directory structure of the Web RowSet application is shown in the **Applications Navigator**. Run the `ModifyWebRowSet.jsp` JSP in JDeveloper. The JSP is displayed in a browser. Specify a SQL query to generate a Web RowSet. Click on **Apply Query** Subsequently we will modify the Web RowSet and update the database.

![](img/553e44bc30e70d0d8e58278a7ece8da8_256_0.png)

A Web RowSet is generated. We will use the Web RowSet object to create, read, update, and delete the result set obtained with the SQL query. In the ModifyWebRowSet.jsp, set the WebRowSetQuery object as a session object attribute:

```
session.setAttribute("query", query);
```

The OracleWebRowSet object of the ModifyWebRowSet object will be used in the Create, Read, Update, and Delete JSPs.

## Creating a New Row

Next, create a new row in the Web RowSet. Click on the Create Row link in the ModifyWebRowSet.jsp JSP.

![](img/553e44bc30e70d0d8e58278a7ece8da8_257_0.png)

The CreateRow.jsp is displayed. Specify the row values to add and click on Apply.

![](img/553e44bc30e70d0d8e58278a7ece8da8_258_0.png)

In the CreateRow.jsp, the input values are retrieved from the JSP and the insertRow() method of the WebRowSetQuery class is invoked. The WebRowSetQuery object is retrieved from the session object:

```
WebRowSetQuery query=( webrowset.WebRowSetQuery)
    session.getAttribute("query");
```

In the insertRow() method OracleWebRowSet object cursor is moved to the insert row:

```
webRowSet.moveToInsertRow();
```

Set the row values with the updateString() method:

```
webRowSet.updateString(1, journal);
webRowSet.updateString(2, publisher);
webRowSet.updateString(3, edition);
webRowSet.updateString(4, title);
webRowSet.updateString(5, author);
```

Add the row to the OracleWebRowSet:

```
webRowSet.insertRow();
```

A new row is added in the OracleWebRowSet object. A new row is not yet added to the database. CreateRow.jsp is listed as follows:

```
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page session="true"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=windows-1252">
<title>Create Row with Web RowSet</title>
</head>
<body>
<form><h3>Create Row with Web RowSet</h3>
<table>
<tr><td><a href="ModifyWebRowSet.jsp">Modify Web RowSet Page</a></td></tr>
</table>
</form>
<%
webrowset.WebRowSetQuery query=null;
query=(webrowset.WebRowSetQuery)
session.getAttribute("query");
String journal=request.getParameter("journal");
String publisher=request.getParameter("publisher");
String edition=request.getParameter("edition");
String title=request.getParameter("title");
String author=request.getParameter("author");
if(journal!=null||publisher!=null||edition!=null||title!=null||author!=null){
query.insertRow(journal, publisher, edition, title, author);
}
%>
<form name="query" action="CreateRow.jsp" method="post">
<table>
<tr>
<td>
<h4>Insert Row</h4>
</td>
</tr>
```

```
<tr>
    <td>Journal:</td>
</tr>
<tr>
    <td>
        <input name="journal" type="text" size="50"
            maxlength="250"/>
    </td>
</tr>
<tr>
    <td>Publisher:</td>
</tr>
<tr>
    <td>
        <input name="publisher" type="text" size="50"
            maxlength="250"/>
    </td>
</tr>
<tr>
    <td>Edition:</td>
</tr>
<tr>
    <td>
        <input name="edition" type="text" size="50"
            maxlength="250"/>
    </td>
</tr>
<tr>
    <td>Title:</td>
</tr>
<tr>
    <td>
        <input name="title" type="text" size="50"
            maxlength="250"/>
    </td>
</tr>
<tr>
    <td>Author:</td>
</tr>
<tr>
    <td>
        <input name="author" type="text" size="50"
            maxlength="250"/>
    </td>
</tr>
```

```
</tr>
<tr>
    <td>
        <input class="Submit" type="submit" value="Apply"/>
    </td>
</tr>
</table></form></body></html>
```

## Reading a Row

Next, we will read a row from the OracleWebRowSet object. Click on Modify Web RowSet link in the CreateRow.jsp. In the ModifyWebRowSet JSP click on the Read Row link. The ReadRow.jsp JSP is displayed. In the ReadRow JSP specify the Database Row to Read and click on Apply.

![](img/553e44bc30e70d0d8e58278a7ece8da8_261_0.png)

The second row values are retrieved from the Web RowSet:

![](img/553e44bc30e70d0d8e58278a7ece8da8_262_0.png)

In the ReadRow JSP the readRow() method of the WebRowSetQuery.java application is invoked. The WebRowSetQuery object is retrieved from the session object.

```
WebRowSetQuery query=( webrowset.WebRowSetQuery)
    session.getAttribute("query");
```

The String[] values returned by the readRow() method are added to the ReadRow JSP fields. In the readRow() method the OracleWebRowSet object cursor is moved to the row to be read.

```
webRowSet.absolute(rowRead);
```

Retrieve the row values with the getString() method and add to String[]. Return the String[] object.

```
String[] resultSet=new String[5];
resultSet[0]=webRowSet.getString(1);
resultSet[1]=webRowSet.getString(2);
resultSet[2]=webRowSet.getString(3);
resultSet[3]=webRowSet.getString(4);
resultSet[4]=webRowSet.getString(5);
return resultSet;
```
