
DatabaseMetaData metaData=connection.getMetaData();
if(metaData.supportsStatementPooling())
{
    if(stmt.isPoolable())
    stmt.setPoolable(true);
}
Class class = Class.forName("oracle.jdbc.OracleStatement");
if(stmt.isWrapperFor(class))
{
    OracleStatement oracleStmt = (OracleStatement)stmt.unwrap(class);
    oracleStmt.clearDefines();
    oracleStmt.defineColumnType(1, OracleTypes.NUMBER);
    oracleStmt.defineColumnType(2, OracleTypes.VARCHAR);
    oracleStmt.defineColumnType(3, OracleTypes.VARCHAR);
    oracleStmt.defineColumnType(4, OracleTypes.VARCHAR);
    oracleStmt.defineColumnType(5, OracleTypes.VARCHAR);
    oracleStmt.defineColumnType(6, OracleTypes.VARCHAR);
    oracleStmt.defineColumnType(7, OracleTypes.VARCHAR);
    ResultSet resultSet=oracleStmt.executeQuery("Select ROWID,
CATALOGID, JOURNAL, PUBLISHER, EDITION, TITLE, AUTHOR from Catalog");
%>
<table border="1" cellspacing="0">
    <tr>
        <th>CatalogId</th>
        <th>Journal</th>
        <th>Publisher</th>
        <th>Edition</th>
        <th>Title</th>
        <th>Author</th>
    </tr>
    <%
    while (resultSet.next())
    {
%>
<tr>
    <td><%=out.println(resultSet.getRowId("ROWID").toString());%></td>
    <td><%=out.println(resultSet.getString(1));%></td>
    <td><%=out.println(resultSet.getString(2));%></td>
    <td><%=out.println(resultSet.getString(3));%></td>
    <td><%=out.println(resultSet.getString(4));%></td>
    <td><%=out.println(resultSet.getString(5));%></td>
    <td><%=out.println(resultSet.getString(6));%></td>
</tr>
<%
}
resultSet.close();
stmt.close();
connection.close();
}
%>
</table>
</body>
</html>

## Summary

WebLogic Server 9.x 引入了一个新功能，多数据源，即一组具有 JNDI 名称绑定的数据源。多数据源可最大化可用数据源。WebLogic Server 8.1 也具有名为 MultiPools 的多数据源。任何池或多池都通过 DataSource 访问。对于 9.0 及更高版本，用于访问池或多池概念的 DataSource 已合并。在 WebLogic Server 9.x 中，WebLogic Server 8.1 中使用的连接池配置已被移除。WebLogic Server 9.x 中的数据源配置提供了增强的连接请求故障转移和数据源之间的负载均衡。在支持 JDBC 4.0 的 WebLogic Server 版本中，可以使用 JDBC 4.0 的新功能，例如连接状态跟踪、Java SE 链式设施以及用于访问非标准资源的包装器模式。

## 6 在 WebSphere Application Server 中配置 JDBC

WebSphere Application Server 6.1 是一个基于 J2EE 和 Web 服务技术的应用服务器，用于开发 J2EE 应用程序。WebSphere Application Server 用于开发 JDBC 应用程序。IBM DB2 9 提供了一些新功能，适用于开发 JDBC 4.0 应用程序。DB2 9 已添加对 SQL XML 数据类型的支持，可与 SQLXML Java 数据类型结合使用。它是 JDBC 4.0 中的一种新型数据类型，用于存储和检索 XML 文档。DB2 数据库服务器通过配置 JDBC Provider 从 WebSphere Application Server 提供访问，该术语是应用服务器中的 WebSphere 专有术语。

在本章中，我们将在 WebSphere Application Server 6.1 中配置数据源以连接到 DB2 9 数据库。我们将部署一个 Web 应用程序到 WebSphere Application Server，该应用程序包含一个 JSP，用于使用服务器中配置的数据源生成 HTML 表。我们将使用 DB2 JDBC Type 4 驱动程序创建到 DB2 9 数据库的 JDBC 连接。Type 4 驱动程序相对于 Type 2 驱动程序具有优势：它提供对 DB2 数据库的直接访问。本章中您将了解以下内容：

- 在 WebSphere Application Server 6.1 中使用 DB2 9 数据库创建 JDBC Provider。
- 在 WebSphere Application Server 6.1 中使用 DB2 9 数据库创建数据源。
- 从 JDeveloper 10g 部署 Web 应用程序到 WebSphere Application Server 6.1 以连接 DB2 数据库 9。
- Web 应用程序的 JDBC 4.0 版本。

## 设置环境

在从 WebSphere Application Server 6.1 配置与 DB2 9 数据库的 JDBC 连接之前，需要进行以下初步设置：

1. 从 http://www.ibm.com/developerworks/downloads/im/udb/?S_TACT=105AGX28&S_CMP=TRIALS 下载并安装 DB2 9 数据库。
2. 在 DB2 9 数据库服务器中创建示例数据库 SAMPLE。
3. 在数据库中创建包含期刊文章目录的示例表。创建示例表的 SQL 脚本如下所示：
4. 从 http://www14.software.ibm.com/webapp/download/preconfig.jsp?id=2005-01-19+10%3A47%3A53.647228&S_TACT=104CBW71&S_CMP= 下载 WebSphere Application Server 6.1 并安装 WebSphere Application Server。

```sql
CREATE TABLE DB2ADMIN.Catalog(CatalogId VARCHAR(25) PRIMARY KEY NOT NULL, Journal VARCHAR(25), Section VARCHAR(25), Edition VARCHAR(25), Title VARCHAR(75), Author VARCHAR(25))

INSERT INTO DB2ADMIN.Catalog VALUES('catalog1', 'developerWorks', 'Java Technology', 'Nov 2004', 'Getting started with enumerated types', 'Brett McLaughlin');

INSERT INTO DB2ADMIN.Catalog VALUES('catalog2', 'developerWorks', 'XML', 'Apr 2005', 'Transform Eclipse navigation files to DITA navigation files', 'Loretta Hicks');
```

## 配置 JDBC Provider

在本节中，我们将从 WebSphere Application Server 6.1 配置与 DB2 9 数据库的 JDBC 连接。从 First steps 控制台启动 WebSphere Application Server。从 First steps 控制台访问 WebSphere Administration 控制台，或使用 URL https://localhost:9043/ibm/console/logon.jsp。

![](img/553e44bc30e70d0d8e58278a7ece8da8_169_0.png)

在登录页面，指定 **用户 ID** 和 **密码**，然后单击 **登录**。管理控制台页面显示。管理控制台页面提供配置 JDBC、部署应用程序、设置环境变量和定义登录配置的功能。

选择 **资源 | JDBC | JDBC 提供程序** 节点以配置新的 JDBC 提供程序。JDBC 提供程序用于访问数据库。它包含数据库、JDBC 驱动程序类型和驱动程序类的设置。

![](img/553e44bc30e70d0d8e58278a7ece8da8_169_1.png)

## 配置 JDBC in WebSphere Application Server

选择范围 Scope，Server 以定义新的 JDBC 提供程序。范围定义 JDBC 提供程序可用的级别。WebSphere 架构包含三个级别：单元、节点和服务器。单元是包含一组节点的行政域。节点是一组管理服务器进程的 WebSphere Application Server。应用服务器是 WebSphere Application Server 配置中的主要运行时组件。如果要使 JDBC Provider 可用于节点中的所有应用服务器，请选择节点作为范围。如果要使 JDBC Provider 可用于单元中的所有节点，请选择单元作为范围。选择新建以配置新的 JDBC 提供程序。

![](img/553e44bc30e70d0d8e58278a7ece8da8_170_0.png)

在创建新的 JDBC 提供程序窗口中，选择 DB2 作为数据库类型。如果要配置连接的数据库未列出，请为数据库类型选择用户定义。选择提供程序类型为 DB2 Universal JDBC Driver Provider。对于 Windows 上的 DB2 数据库类型，提供程序类型 DB2 Universal JDBC Provider (XA) 用于分布式事务。如果选择的数据库类型为用户定义，则指定提供程序类型用户定义 JDBC Provider。选择实现类型，连接池数据源。如果选择用户定义数据库类型作为实现类型，则也指定用户定义。如果选择提供程序类型 DB2 Universal JDBC Provider (XA)，则选择实现类型 XA DataSource 并单击下一步。
