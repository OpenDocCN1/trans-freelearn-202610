
![](img/553e44bc30e70d0d8e58278a7ece8da8_97_0.png)

## 创建数据库表

在本节中，我们将使用 JDeveloper 10.1.3 IDE 中的 JSTL 1.1 SQL 标签在 Oracle 数据库中创建数据库表。数据库表将使用 catalog.jsp JSP 创建，该 JSP 在“设置环境”部分中创建。下面所示的 JSTL SQL taglib 和 JSTL Core taglib 的 taglib 指令被添加到 catalog.jsp 中，因为在创建 JSP 时选择了 JSTL SQL 1.1 和 JSTL Core 1.1 taglib。

```
<%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
```

## 使用 JSTL SQL 标签库连接数据库

要访问 Oracle 数据库，请使用 SQL 标签 setDataSource 创建数据源。JDeveloper 提供了一个组件调色板，用于向 JSP 页面添加 JSTL SQL 标签。选择 JSTL 1.1 SQL 组件调色板。将光标定位在 JSP 页面中，并在 JSTL SQL 组件调色板中选择 SetDataSource 标签。

![](img/553e44bc30e70d0d8e58278a7ece8da8_98_0.png)

在 setDataSource 标签中，在 driver 属性中指定 JDBC 驱动程序 oracle.jdbc.OracleDriver 用于 Oracle 数据库。将连接 URL 指定为：jdbc:oracle:thin:@localhost:1521:ORCL 用于 Oracle 数据库。还请指定用户名为 OE，以及密码。要添加标签属性，请在 setDataSource 标签中按空格键并双击该属性。

![](img/553e44bc30e70d0d8e58278a7ece8da8_99_0.png)

添加了属性的 sql:setDataSource 标签如下所示：

```
<sql:setDataSource driver="oracle.jdbc.OracleDriver" url="jdbc:oracle:
thin:@localhost:1521:ORCL" user="OE" password="pw" />
```

## 使用 JSTL SQL 标签库连接数据库

接下来，使用 sql:transaction 标签创建一个事务以运行 SQL 语句、创建数据库表并向数据库表添加行。将光标定位在 JSP 页面中，并在 JSTL 1.1 SQL 组件调色板中选择 Transaction。

![](img/553e44bc30e70d0d8e58278a7ece8da8_100_0.png)

sql:transaction 标签提供 dataSource 和 isolation 属性，可以像 setDataSource 标签属性一样添加。将以下 sql:transaction 标签添加到 JSP 页面。

```
<sql:transaction>
</sql:transaction>
```

`sql:update` 标签用于创建数据库表并向数据库表添加行。`sql:update` 标签添加到 `sql:transaction` 标签内。将光标定位在打开的 `sql:transaction` 标签和关闭的 `sql:transaction` 标签之间，并在 JSTL 1.1 SQL 组件调色板中选择 Update 标签。

![](img/553e44bc30e70d0d8e58278a7ece8da8_101_0.png)

## 使用 JSTL SQL 标签库连接数据库

sql:update 可能包含 sql、dataSource、var 和 scope 属性。首先，使用 JSTL SQL 标签创建数据库表。SQL CREATE 语句在 sql:update 标签的 sql 属性中指定。要添加 sql 属性，请在 sql:update 标签中按空格键并在列出的属性中双击 sql 属性：

![](img/553e44bc30e70d0d8e58278a7ece8da8_102_0.png)

创建数据库表的 sql:update 标签如下：

```
<sql:update sql="CREATE TABLE OE.Catalog(CatalogId VARCHAR(25) PRIMARY KEY, Journal VARCHAR(25), Publisher VARCHAR(25), Edition VARCHAR(25), Title Varchar(45), Author Varchar(25))">
</sql:update>
```

接下来，使用 sql:update 标签向数据库表添加行。INSERT 语句在 sql:update 标签的 sql 属性中指定。

```
<sql:update sql="INSERT INTO OE.Catalog VALUES('catalog1', 'Oracle Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'Database Resource Manager', 'Kimberly Floss')">
</sql:update>
```

用于创建数据库表并向数据库表中插入行的 **catalog.jsp** 页面在以下列表中显示：

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;
charset=windows-1252"/>
<title>catalog</title>
</head>
<body><sql:setDataSource driver="oracle.jdbc.OracleDriver"
url="jdbc:oracle:thin:@localhost:1521:ORCL" user="OE" password="pw"/>
<sql:transaction>
<sql:update sql="CREATE TABLE OE.Catalog(CatalogId VARCHAR(25)
PRIMARY KEY, Journal VARCHAR(25), Publisher VARCHAR(25),
Edition VARCHAR(25), Title Varchar(45), Author Varchar(25))">
</sql:update>
<sql:update sql="INSERT INTO OE.Catalog VALUES('catalog1', 'Oracle
Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'Database Resource
Manager', 'Kimberly
Floss')">
</sql:update>
<sql:update sql="INSERT INTO OE.Catalog VALUES('catalog2', 'Oracle
Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'From ADF UIX to JSF',
'Jonas Jacobi')">
</sql:update>
<sql:update sql="INSERT INTO OE.Catalog VALUES('catalog3', 'Oracle
Magazine',
'Oracle Publishing','March-April 2005','Starting with Oracle ADF',
'Steve Muench')">
</sql:update>
</sql:transaction>
<%out.println("Database Table Created");%>
</body>
</html>
```

## 使用 JSTL SQL 标签库连接数据库

通过在应用程序导航器中右键单击 JSP 节点并选择 Run 来运行 JSP。

![](img/553e44bc30e70d0d8e58278a7ece8da8_104_0.png)

数据库表被创建，并且行被添加到数据库表中。

## 更新数据库表

在本节中，将更新数据库表行。在 JSTLSQL 项目中创建一个 JSP catalogUpdate.jsp。如前一节所述，使用 setDataSource 标签为 JSP 页面设置数据源。UPDATE SQL 语句使用 sql:update 标签运行。从组件调色板向 JSP 页面添加一个 Update 标签。SQL 语句具有用于指定 UPDATE 语句的 IN 参数的占位符。将 UPDATE SQL 语句添加到 sql:update 标签的 sql 属性中。

```
<sql:update sql="UPDATE OE.CATALOG SET TITLE=?, AUTHOR=?
WHERE CATALOGID=?">
</sql:update>
```

IN 参数的值使用 `sql:param` 标签指定。将光标定位在 `sql:update` 语句中，并在 JSTL 1.1 SQL 组件调色板中选择 Param 标签。

![](img/553e44bc30e70d0d8e58278a7ece8da8_105_0.png)

## 使用 JSTL SQL 标签库连接数据库

param 值使用 sql:param 标签中的 value 属性设置。要添加 value 属性，请在 sql:param 标签中按空格键并双击 value 属性。

![](img/553e44bc30e70d0d8e58278a7ece8da8_106_0.png)

指定 param 值并类似地添加 sql:param 标签以指定与 UPDATE SQL 语句中 IN 参数对应的 param 值，如下所示：

```
<sql:param value="Introduction to ADF"/>
<sql:param value="Muench, Steve"/>
<sql:param value="catalog3"/>
```

catalogUpdate.jsp 如下所示：

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html;
charset=windows-1252"/>
    <title>catalogUpdate</title>
    </head>
    <body>
    <sql:setDataSource driver="oracle.jdbc.OracleDriver"
url="jdbc:oracle:thin:@localhost:1521:ORCL" user="oe"
password="pw"/>
    <sql:update sql="UPDATE OE.CATALOG SET TITLE=?, AUTHOR=?
WHERE CATALOGID=?">
    <sql:param value="Introduction to ADF"/>
    <sql:param value="Muench, Steve"/>
    <sql:param value="catalog3"/>
    </sql:update>
    </body>
    </html>
```

通过右键单击 JSP 并选择 Run 来运行 catalogUpdate.jsp。

![](img/553e44bc30e70d0d8e58278a7ece8da8_107_0.png)

数据库表已更新。
