
## 查询数据库表

在本节中，将查询在前几节中创建和修改的数据库表，并将结果显示在 HTML 表格中。创建一个 JSP，catalogQuery.jsp。为 JSP 选择 JSTL Core 1.1 和 JSTL SQL 1.1 标签库。如前几节所述，添加一个 setDataSource 标签以获取与 Oracle 数据库的连接。用于查询数据库的 SQL 标签库标签是 sql:query。在 JSP 页面中定位光标，并在 SQL 组件调色板中选择 Query 标签。

![](img/553e44bc30e70d0d8e58278a7ece8da8_108_0.png)

在“插入查询”窗口中，指定 var 属性，该属性是必需属性。var 属性指定 Query 标签返回的结果集的变量。选择“高级属性”选项卡。

![](img/553e44bc30e70d0d8e58278a7ece8da8_108_1.png)

在 sql 属性字段中指定 SQL 查询，然后单击“确定”按钮。

![](img/553e44bc30e70d0d8e58278a7ece8da8_109_0.png)

sql:query 标签被添加到 JSP 页面：

```
<sql:query  var="catalog" sql="SELECT * FROM CATALOG"/>
```

添加一个 <table> 来显示 Query 标签的结果集。添加表头。结果集中每一行都将对应添加一行。选择 JSTL 1.1 Core 组件库并向 <table> 添加 ForEach 标签。

![](img/553e44bc30e70d0d8e58278a7ece8da8_109_1.png)

## 使用 JSTL SQL 标签库连接数据库

c:forEach 标签的 var 属性指定与结果集中的一行相对应的变量。items 属性指定用于遍历 Query 标签返回的结果集中的行的 EL 绑定。sql:query 标签返回的结果集中的行是通过 ${catalog.rows} EL 表达式获取的。catalogQuery.jsp JSP 中的 c:forEach 标签如下：

```
<c:forEach var="journal" items="${catalog.rows}">
</c:forEach>
```

接下来，将行中每个列对应的值输出到 JSP 表格。JSP 表格值通过 c:out 标签添加。从 JSTL 1.1 Core 组件调色板为 c:forEach 标签添加每个列的 c:out 标签。

![](img/553e44bc30e70d0d8e58278a7ece8da8_110_0.png)

在“插入输出”窗口中，指定用于绑定列值的 EL 表达式。列值绑定通过结果集中的列名获取。要绑定列值，请单击“绑定”按钮。journal 是与结果集中的一行相对应的变量。例如，绑定 CatalogId 列的 EL 表达式是 ${journal.CatalogId}。在“绑定到数据”窗口的表达式文本区域中指定 EL 表达式，然后单击“确定”按钮。

![](img/553e44bc30e70d0d8e58278a7ece8da8_111_0.png)

在“插入输出”窗口中单击“确定”按钮。

![](img/553e44bc30e70d0d8e58278a7ece8da8_111_1.png)

类似地为结果集中的其他列添加 c:out 标签。下面列出了 catalogQuery.jsp：

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

要运行 **catalogQuery.jsp**，右键单击 JSP 并选择 **Run**：

![](img/553e44bc30e70d0d8e58278a7ece8da8_112_0.png)

通过 Query 标签获取的结果集会显示在 HTML 表格中。

| 目录ID | 期刊 | 出版社 | 版本 | 标题 | 作者 |
|---|---|---|---|---|---|
| catalog1 | Oracle Magazine | Oracle Publishing | Nov-Dec 2004 | Database Resource Manager | Kimberly Floss |
| catalog2 | Oracle Magazine | Oracle Publishing | Nov-Dec 2004 | From ADF UIX to JSF | Jonas Jacobi |
| catalog3 | Oracle Magazine | Oracle Publishing | March-April 2005 | Introduction to ADF | Muench, Steve |

## 摘要

JSTL SQL 标签库提供对数据库的访问。JDeveloper 10.1.3 通过为 JSTL SQL 1.1 标签提供的组件调色板来促进 JSTL SQL 应用程序的开发，可从中将标签库标签添加到 JSP 应用程序。在本章中，您已学习如何在 JDeveloper 10.1.3 中使用 JSTL SQL 1.1 标签创建 Oracle 数据库表。您还学习了如何使用 JSTL SQL 1.1 标签更新数据库表和查询数据库表。

## 4 在 JBoss 应用服务器中配置 JDBC

JBoss 4.0 是一个开源应用服务器，默认配置为使用 **Hypersonic Database**（HSQLDB）。然而，一些 **Java 2 Platform Enterprise Edition**（J2EE）开发人员希望使用除 Hypersonic Database 之外的数据库来开发和部署应用程序。JBoss 应用服务器为 **Enterprise Java Beans**（EJB）持久化和 J2EE 应用程序提供数据源访问。在本章中，我们将了解如何配置 JBoss 以使用其他数据库。

JBoss 4.0 服务器使用 Java 数据库连接（JDBC）配置文件和数据源文件来将服务器配置为使用数据库。要使用除默认数据库 Hypersonic 之外的数据库，JDBC 配置文件必须进行修改，并且数据库的数据源文件必须提供给服务器。从 `http://labs.jboss.com/jbossas/downloads` 下载 JBoss 4.0 应用服务器 ZIP 文件，并通过将 ZIP 文件解压到目录中来安装 JBoss 服务器。在本章中，您将学习如何执行以下操作：

- 从 JDeveloper 部署 Web 应用程序到 JBoss 应用服务器
- 部署 JDBC 4.0 版本的 Web 应用程序到 JBoss 服务器
- 使用 JDBC 4.0 驱动程序配置 JBoss 服务器以使用 Oracle 数据库 10g
- 使用 JDBC 4.0 驱动程序配置 JBoss 服务器以使用 MySQL 数据库
- 使用 JDBC 4.0 驱动程序配置 JBoss 服务器以使用 PostgreSQL 数据库服务器
- 使用 JDBC 4.0 驱动程序配置 JBoss 服务器以使用 DB2 UDB 数据库
- 使用 JDBC 4.0 驱动程序配置 JBoss 服务器以使用 MS SQL Server

## 从 JDeveloper 部署 Web 应用程序到 JBoss

在本节中，我们将从 JDeveloper IDE 创建 Web 应用程序并将其部署到 JBoss 应用服务器。JDeveloper 具有内置功能，可配置与 JBoss 应用服务器的连接，并将 Web 应用程序部署到服务器。首先，通过选择 文件 | 新建 在 JDeveloper 中创建应用程序。在“新建库”窗口中，在“类别”中选择“常规”，在“项目”中选择“应用程序”，然后单击“确定”。在“创建应用程序”窗口中，指定应用程序名称，然后单击“确定”按钮。在“创建项目”窗口中，指定项目名称，然后单击“确定”。应用程序和项目将被添加到应用程序导航器。接下来，要添加 JSP，请在应用程序导航器中选择项目节点，然后选择 文件 | 新建。在“新建库”窗口中，在“类别”中选择“Web 层 | JSP”，在“项目”中选择“JSP”，然后单击“确定”。“创建 JSP 向导”启动。现在，单击“下一步”。在“Web 应用程序”窗口中选择 J2EE 1.4，然后单击“确定”。在 JSP 文件窗口中指定文件名，然后单击“下一步”按钮。在“错误页面选项”窗口中选择默认设置，然后单击“下一步”。在“标签库”窗口中单击“下一步”。在“HTML 选项”窗口中选择默认设置，然后单击“下一步”。在“完成”窗口中单击“完成”。现在，JSP 和 web.xml 将被添加到应用程序导航器。

![](img/553e44bc30e70d0d8e58278a7ece8da8_116_0.png)

接下来，使用以下 SQL 脚本在 MySQL 数据库中创建数据库表：

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
