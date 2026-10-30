
## 创建 JSF 面板网格

右键单击 **catalog.jsp** 节点并选择 **运行**。输入字段中指定的初始值的面板网格显示如下：

![](img/553e44bc30e70d0d8e58278a7ece8da8_340_0.png)

要更新数据库表行，请指定该行的 Catalog Id 和输入字段值，然后单击 **提交** 按钮。输入字段 **Catalog Id** 被指定为 **必填** 字段。如果在 **Catalog Id** 字段中未指定输入值，则会生成验证错误。验证错误会显示在消息组件列中。

![](img/553e44bc30e70d0d8e58278a7ece8da8_341_0.png)

接下来，我们将演示与 Catalog Id 输入字段注册的验证器。指定不在 1 到 3 范围内的 Catalog Id 输入值并单击提交按钮。消息列中会生成验证错误。

![](img/553e44bc30e70d0d8e58278a7ece8da8_341_1.png)

## JDBC 4.0 版本

在其嵌入式 OC4J 服务器支持 JDBC 4.0 的 JDeveloper 版本中，我们可以将 JDBC 4.0 功能添加到 JSF 面板网格应用程序中。JDBC 4.0 提供了诸如增强的连接管理、使用 Java SE 服务提供者机制自动加载驱动程序、支持包装器模式以访问 JDBC API 的供应商特定扩展、语句池以及对 Java SE 链式异常设施和异常分类的支持等新功能。

对于 JSF 面板网格应用程序的 JDBC 4.0 版本，我们需要从 http://www.oracle.com/technology/software/tech/java/sqlj_jdbc/htdocs/jdbc_111060.html 下载 Oracle 数据库 11g JDBC 4.0 驱动程序 JAR 文件 ojdbc6.jar。JDBC 4.0 驱动程序需要 JDK 6.0。因此，我们需要在 JDeveloper 中将 J2SE 版本设置为 JDK 6.0。要设置 JDK 版本，请选择 工具 | 项目属性。在项目属性窗口中选择 库，并在 J2SE 版本 字段中指定 JDK 6.0 版本。

JDBC 4.0 驱动程序 JAR 文件 ojdbc6.jar 包含一个 META-INF/services/java.sql.Driver 文件，该文件指定了 Oracle JDBC 驱动程序类 oracle.jdbc.OracleDriver。java.sql.Driver 字段包含在 ojdbc6.jar 中，以便 Java SE 服务提供者机制加载 JDBC 驱动程序。在 backing bean 类中 commandButton1_action() 方法的 JDBC 4.0 版本中，删除使用 Class.forName() 方法加载 Oracle JDBC 驱动程序的那一行。

JDBC 4.0 使用 Connection 接口中的 isValid() 方法提供连接状态跟踪。连接池管理器使用 isValid 方法测试连接是否变得不可用。如果连接变得不可用，连接池管理器将使用 close() 方法关闭连接。连接状态跟踪减少了在连接池中的某些连接变得不可用时关闭连接池中的所有连接并重新初始化连接池的开销。

JDBC 4.0 提供的另一个连接管理功能是连接客户端标识。有时某些客户端连接会因消耗过多的 CPU 而拖慢应用程序。JDBC 的早期版本没有识别此类客户端连接的 provision。JDBC 4.0 中的 Connection 接口有两个新方法 setClientInfo 和 getClientInfo，用于设置和获取客户端信息属性。因此，Connection 对象由其客户端信息属性标识。数据库支持的不同客户端信息属性可以使用 DatabaseMetaData 接口的 getClientInfoProperties 方法获得。标准客户端信息属性是 ApplicationName、ClientUser 和 Hostname。

除了 PreparedStatements 池化之外，JDBC 4.0 还提供 Statement 池化。经常使用的 Statements 可以使用 setPoolable() 方法进行池化。isPoolable() 方法用于测试 Statement 对象是否可池化。

JDBC 4.0 在 Wrapper 接口中支持包装器模式，以访问 JDBC API 的供应商特定扩展。Oracle JDBC 扩展可在 oracle.jdbc.package 中获得。例如，要访问扩展 Statement 接口的 OracleStatement 接口中的非标准方法，请使用 isWrapperFor 方法测试 Statement 对象是否为 OracleStatement 接口的包装器。如果 Statement 对象是包装器，请使用 unwrap() 方法解包 OracleStatement 接口以创建 OracleStatement 对象。随后，可以调用 OracleStatement 对象的 defineColumnType 方法来定义列类型。定义列类型可节省 JDBC 驱动程序往返数据库查找列类型的开销。当运行 SQL 查询时，数据库表数据会被提取到 ResultSet 中的已定义列类型中。

JDBC 4.0 提供了对 Java SE 链式异常设施的支持，也称为 cause 设施，用于检索链式异常。JDBC 4.0 还检索非 SQLException 链式异常。在面板网格应用程序的 JDBC 4.0 版本中，链式异常和链式原因使用 J2SE 5.0 中引入的增强 for-each 循环检索。

用于检索指定 Catalog ID 的字段值的 commandButton1_action() 方法的 JDBC 4.0 版本如下所示：

```
public String commandButton1_action() {
    // Add event code here...
    ResultSet rs=null;
    try{
        String url="jdbc:oracle:thin:@localhost:1521:ORCL";
        Connection connection =
        DriverManager.getConnection(url,"OE", "pw");
    connection.setClientInfo("ApplicationName","OracleApp");
    connection.setClientInfo("ClientUser","OracleUser");
    connection.setClientInfo("ClientHostname","OracleHost");
    Statement stmt=connection.createStatement(ResultSet.TYPE_SCROLL_INSENSITIVE, ResultSet.CONCUR_READ_ONLY);
    DatabaseMetaData metaData=connection.getMetaData();
    if(metaData.supportsStatementPooling()){
    if(stmt.isPoolable())
    stmt.setPoolable(true);
    }
    Class clazz = Class.forName("oracle.jdbc.OracleStatement");
    if(stmt.isWrapperFor(clazz)) {
    OracleStatement oracleStmt = (OracleStatement)stmt.unwrap(clazz);
    oracleStmt.defineColumnType(1, OracleTypes.VARCHAR);
```

```
oracleStmt.defineColumnType(2, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(3, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(4, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(5, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(6, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(7, OracleTypes.VARCHAR);
String query="SELECT * FROM OE.CATALOG WHERE CATALOGID="+inputText1.getValue();
        rs=stmt.executeQuery(query);
rs=stmt.executeQuery("SELECT * FROM OE.CATALOG");
outputText1.setValue(rs.getString("Journal"));
        outputText2.setValue(rs.getString("Publisher"));
        outputText3.setValue(rs.getString("Edition"));
        outputLink1.setValue(rs.getString("HRef"));
        outputText4.setValue(rs.getString("Title"));
        outputText5.setValue(rs.getString("Author"));
} rs.close();
stmt.close();
connection.close();
    }catch(SQLException e){for(Throwable e : e.getCause()) {
out.println("Error encountered: " + e);
}
        return null;
    }
```

## 摘要

JSF 面板网格是用于以网格布局在指定数量的列中显示一组 UI 组件的 UI 组件。在本章中，面板网格用于显示从数据库检索的目录数据。面板网格还用于转换和验证输入字段值。面板网格可以在 Create PanelGrid Wizard 中通过绑定行或绑定托管 bean 来创建。在面板网格应用程序的 JDBC 4.0 版本中，我们添加了 JDBC 4.0 功能自动驱动程序加载、客户端信息属性、语句池和包装器模式。

## 12 使用 JasperReports 创建报告

JasperReports 是一个用于准备演示报告的 Java 报告工具。JasperReports 报告是 HTML、PDF、Excel XLS、CSV 或 XML 报告。JasperReports 报告的数据是静态数据，或是通过 SQL 查询从数据库表检索的数据。JasperReports 设计用于包含在 Java/J2EE 应用程序中以生成动态演示报告。

在本章中，我们将在 JDeveloper 中创建 JasperReports 报告。JasperReports 使用 JDBC 连接到数据库并检索 JasperReports 报告的数据。在 JasperReports 配置文件（.xml 文件）中建立了与 Oracle 数据库 10g 的 JDBC 连接。我们将使用 JasperReports 创建 PDF 报告和 Excel 电子表格报告。在本章中我们将学习以下内容：

- 使用 JasperReports 创建 PDF 报告
- 使用 JasperReports 创建 Excel 电子表格

## 设置环境

下载开源 JasperReports 工具 JAR 文件 `jasperreports-2.0.5.jar`。下载 iText Java-PDF 库 `itext-2.1.0.jar`。下载 Jakarta-POI ZIP 文件 `poi-bin-3.0.2-FINAL-20080204.zip`。下载 `Commons Digester 1.8` 并将 ZIP 文件解压到一个目录中。安装 JDeveloper 10g IDE Studio Edition 完整安装。从以下位置下载并安装 Oracle 数据库：http://www.oracle.com/database/index.html 10g。

创建一个示例数据库表 `Catalog`，从中将生成 JasperReports PDF 报告和 Excel 报告。在 Oracle SQL *Plus 中创建数据库表。创建示例表的 SQL 脚本如下所示。
