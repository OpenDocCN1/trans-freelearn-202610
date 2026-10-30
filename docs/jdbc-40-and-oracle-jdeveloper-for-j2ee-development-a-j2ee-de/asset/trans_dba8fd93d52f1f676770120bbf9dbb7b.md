
ReadRow.jsp JSP 如下所示：

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page session="true"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=windows-1252">
<title>Read Row with Web RowSet</title>
</head>
<body>
<form><h3>Read Row  with Web RowSet</h3>
<table>
<tr>
<td><a href="ModifyWebRowSet.jsp">Modify Web RowSet Page</a></td>
</tr>
</table>
</form>
<%
webrowset.WebRowSetQuery query=null;
query=( webrowset.WebRowSetQuery) session.getAttribute("query");
String rowRead=request.getParameter("rowRead");
String journalUpdate=request.getParameter("journalUpdate");
String publisherUpdate=request.getParameter("publisherUpdate");
String editionUpdate=request.getParameter("editionUpdate");
String titleUpdate=request.getParameter("titleUpdate");
String authorUpdate=request.getParameter("authorUpdate");
if((rowRead!=null))
{
int row_Read=Integer.parseInt(rowRead);
String[] resultSet=query.readRow(row_Read);
journalUpdate=resultSet[0];
publisherUpdate=resultSet[1];
editionUpdate=resultSet[2];
titleUpdate=resultSet[3];
authorUpdate=resultSet[4];
}
%>
<form name="query" action="ReadRow.jsp" method="post">
<table>
<tr>
<td>Database Row to Read:</td>
</tr>
<tr>
<td>
<input name="rowRead" type="text" size="25"
maxlength="50"/>
</td>
</tr>
<tr>
<td>Journal:</td>
</tr>
<tr>
<td>
<input name="journalUpdate" value='<%=journalUpdate%>'
type="text" size="50" maxlength="250"/>
</td>
</tr>
<tr>
<td>Publisher:</td>
</tr>
<tr>
<td>
<input name="publisherUpdate"
value='<%=publisherUpdate%>' type="text" size="50"
maxlength="250"/>
</td>
</tr>
<tr>
<td>Edition:</td>
</tr>
<tr>
<td>
<input name="editionUpdate" value='<%=editionUpdate%>'
type="text" size="50" maxlength="250"/>
</td>
</tr>
<tr>
<td>Title:</td>
</tr>
<tr>
<td>
<input name="titleUpdate" value='<%=titleUpdate%>'
type="text" size="50" maxlength="250"/>
</td>
</tr>
<tr>
<td>Author:</td>
</tr>
<tr>
<td>
<input name="authorUpdate" value='<%=authorUpdate%>'
type="text" size="50" maxlength="250"/>
</td>
</tr><tr>
<td>
<input class="Submit" type="submit" value="Apply"/>
</td>
</tr>
</table>
</form>
</body></html>
```

## 更新一行

接下来，我们将在 OracleWebRowSet 对象中更新一行。在 ReadRow JSP 中点击修改 Web RowSet 页面链接。在 ModifyWebRowSet JSP 中点击 Update Row 链接。在 UpdateRow JSP 中指定要更新的行并指定修改后的值。例如，更新第二行。点击 Apply。

![](img/553e44bc30e70d0d8e58278a7ece8da8_265_0.png)

UpdateRow JSP 调用 WebRowSetQuery Java 类的 updateRow() 方法。WebRowSetQuery 对象从 session 对象中检索：

```
WebRowSetQuery query=( webrowset.WebRowSetQuery)
    session.getAttribute("query");
```

在 updateRow() 方法中，OracleWebRowSet 对象光标移动到要更新的行：

```
webRowSet.absolute(rowUpdate);
```

使用 OracleWebRowSet 对象的 updateString() 方法更新行值：

```
webRowSet.updateString(1, journal);
webRowSet.updateString(2, publisher);
webRowSet.updateString(3, edition);
webRowSet.updateString(4, title);
webRowSet.updateString(5, author);
```

使用 updateRow() 方法更新 OracleWebRowSet 对象：

```
webRowSet.updateRow();
```

OracleWebRowSet 对象中的行已更新。数据库表中的行尚未更新。UpdateRow.jsp 如下所示：

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page session="true"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;
charset=windows-1252">
<title>Update Row in Database Table with Web RowSet</title>
</head>
<body>
<form><h3>Update Row in Database Table with Web RowSet</h3>
<table>
<tr>
<td><a href="ModifyWebRowSet.jsp">Modify Web RowSet
Page</a></td>
</tr>
</table>
</form>
<%
webrowset.WebRowSetQuery query=null;
query=( webrowset.WebRowSetQuery)
session.getAttribute("query");
String rowUpdate=request.getParameter("rowUpdate");
String journalUpdate=request.getParameter("journalUpdate");
String publisherUpdate=request.getParameter("publisherUpdate");
String editionUpdate=request.getParameter("editionUpdate");
String titleUpdate=request.getParameter("titleUpdate");
String authorUpdate=request.getParameter("authorUpdate");
    if((rowUpdate!=null))
{
    System.out.println(rowUpdate +"Row to Update");
    int row_Update=Integer.parseInt(rowUpdate);
    query.updateRow(row_Update, journalUpdate, publisherUpdate,
        editionUpdate, titleUpdate, authorUpdate);
}
%>
<form name="query" action="UpdateRow.jsp" method="post">
    <table>
        <tr>
            <td>Database Row to Update:</td>
        </tr>
        <tr>
            <td>
                <input name="rowUpdate" type="text" size="25"
                    maxlength="50"/>
            </td>
        </tr>
        <tr>
            <td>Journal:</td>
        </tr>
        <tr>
            <td>
                <input name="journalUpdate" type="text" size="50"
                    maxlength="250"/>
            </td>
        </tr>
        <tr>
            <td>Publisher:</td>
        </tr>
        <tr>
            <td>
                <input name="publisherUpdate" type="text" size="50"
                    maxlength="250"/>
            </td>
        </tr>
        <tr>
            <td>Edition:</td>
        </tr>
        <tr>
            <td>
                <input name="editionUpdate" type="text" size="50"
                    maxlength="250"/>
            </td>
        </tr>
        <tr>
            <td>Title:</td>
        </tr>
        <tr>
            <td>
                <input name="titleUpdate" type="text" size="50"
            maxlength="250"/>
        </td>
    </tr>
    <tr>
        <td>Author:</td>
    </tr>
    <tr>
        <td>
            <input name="authorUpdate" type="text" size="50"
            maxlength="250"/>
        </td>
    </tr>
    <tr>
        <td>
            <input class="Submit" type="submit" value="Apply"/>
        </td>
    </tr>
    </table>
    </form>
    </body>
</html>
```

## 删除一行

接下来，我们将从 OracleWebRowSet 对象中删除一行。在 UpdateRow JSP 中点击 **修改 Web RowSet** 链接。在 ModifyWebRowSet JSP 中点击 **删除行** 链接。在 **删除行** JSP 中指定要删除的行并点击 **应用**。例如，删除第三行。

![](img/553e44bc30e70d0d8e58278a7ece8da8_269_0.png)

在 DeleteRow JSP 中调用 WebRowSetQuery Java 类的 deleteRow() 方法。WebRowSetQuery 对象从 session 对象中检索：

```
WebRowSetQuery query=( webrowset.WebRowSetQuery)
    session.getAttribute("query");
```

在 deleteRow() 方法中，OracleWebRowSet 对象光标移动到要删除的行：

```
webRowSet.absolute(row);
```

使用 OracleWebRowSet 对象的 deleteRow() 方法删除该行。创建、更新和删除操作是在 OracleWebRowSet 对象上执行，而不是在数据库表上。DeleteRow.jsp 如下所示：

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page session="true"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;
charset=windows-1252">
<title>Delete Database Table Row with Web RowSet</title>
</head>
<body>
<form><h3>Delete Database Table Row with Web RowSet</h3>
<table>
<tr>
<td><a href="ModifyWebRowSet.jsp">Modify Web RowSet
Page</a></td>
    </tr>
</table>
    </form>
    <%
        webrowset.WebRowSetQuery query=null;
        query=(
            webrowset.WebRowSetQuery)session.getAttribute("query");
        String deleteRow=request.getParameter("deleteRow");
        if((deleteRow!=null)){
        int delete_Row=Integer.parseInt(deleteRow);
        query.deleteRow(delete_Row);
        }
    %>
    <form name="query" action="DeleteRow.jsp" method="post">
        <table>
            <tr>
                <td><h4>Delete Row</h4></td>
            </tr>
            <tr>
                <td>
                    <input name="deleteRow" type="text" size="25"
                        maxlength="50"/>
                </td>
            </tr>
            <tr>
                <td>
                    <input class="Submit" type="submit" value="Apply"/>
                </td>
            </tr>
        </table>
    </form>
    </body>
</html>
```

## 更新数据库表

接下来，我们将使用修改后的 OracleWebRowSet 对象更新数据库表。在 DeleteRow JSP 中点击 **修改 Web RowSet** 链接。在 ModifyWebRowSet JSP 中，点击 **更新数据库** 链接。在 UpdateDatabase.jsp 中，点击 **应用**。
