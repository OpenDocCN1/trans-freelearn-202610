
```
Class clazz = Class.forName("oracle.jdbc.OracleStatement");
if(stmt.isWrapperFor(clazz)) {
OracleStatement oracleStmt = (OracleStatement)stmt.unwrap(clazz);
oracleStmt.defineColumnType(1, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(2, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(3, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(4, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(5, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(6, OracleTypes.VARCHAR);
oracleStmt.defineColumnType(7, OracleTypes.VARCHAR);
rs=stmt.executeQuery("SELECT * FROM OE.CATALOG");
}
} catch(SQLException e){for(Throwable e : exceptions ) {
out.println("Error encountered: " + e);
}
    return rs;
}
```

同样地，修改 JSF 后台 bean 中的 commandButton1_action 方法。

## 摘要

JSF UI 组件数据表可用于显示通过静态 SQL 查询或动态生成的 SQL 查询检索的数据库数据。JDeveloper 提供了 Create Data Table Wizard 来创建数据表。数据表可以通过将数据表绑定到 MBean 或使用 JSF API 生成。在支持 JDBC 4.0 的 JDeveloper 版本中，可以使用 JDBC 4.0 的新功能，例如连接状态跟踪、语句池、连接标识、对包装器模式的支持以及 Java SE 链式异常设施。

## 11

## 创建 JSF 面板网格

在上一章中，我们使用 JSF 数据表组件显示数据库数据。在本章中，我们将讨论 JSF 面板网格，以使用 JDBC API 从数据库添加和检索数据。我们还将在数据添加到数据库时验证数据。面板网格是一个 UI 面板组件，由 `javax.faces.HtmlPanelGrid` 类和 `h:panelGrid` 标签表示。JDeveloper 提供了 Create PanelGrid Wizard 来创建面板网格。

面板网格以指定的列数显示一组组件。生成面板网格时只需指定列数；行数不是必需的。行数基于网格中的组件数和网格中的列数。面板网格中的组件被添加到指定的列数中。在为一行分配了指定数量的列后，将开始新的面板网格行。例如，如果面板网格中指定的列数为三，且要添加到面板网格的组件数为十。前三个组件添加到第一行，接下来的三个添加到第二行。另三个组件添加到第三行，一个添加到第四行。

在本章中，将使用 Create PanelGrid Wizard 从数据库表生成面板网格。Create PanelGrid Wizard 提供了两种创建面板网格的方法。面板网格可以通过先创建空面板网格然后向其添加组件，或通过将托管 bean 绑定到面板网格来创建。我们将讨论这两种创建面板网格的方法。面板网格的数据将使用 JSF 后台 bean 中的 JDBC 进行检索/添加。在本章中，我们将学习以下内容：

- 使用 Create JSF Panel Grid Wizard 创建空面板网格并向面板网格添加 JSF UI 组件。
- 使用 Create JSF Panel Grid Wizard 通过绑定托管 bean 到面板网格来创建面板网格。
- 向面板网格添加 JSF 验证器。
- 向面板网格添加 JSF 转换器。

## 设置环境

安装 JDeveloper 10.1.3 IDE。安装包括示例模式的 Oracle 10g 数据库。在 SQL Plus 中，创建一个示例数据库表，将从该表生成面板网格。生成示例数据库表的 SQL 脚本如下：

```
CREATE TABLE OE.Catalog(CatalogId INTEGER PRIMARY KEY, Journal VARCHAR(25), Publisher VARCHAR(25), Edition VARCHAR(25), Title Varchar(45), Author Varchar(25), HREF VARCHAR(125));
INSERT INTO OE.Catalog VALUES('1', 'Oracle Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'Database Resource Manager', 'Kimberly Floss', 'http://www.oracle.com/technology/oramag/oracle/04-nov/o64tuning.html');
INSERT INTO OE.Catalog VALUES('2', 'Oracle Magazine', 'Oracle Publishing', 'Nov-Dec 2004', 'From ADF UIX to JSF', 'Jonas Jacobi','http://www.oracle.com/technology/oramag/oracle/04-nov/o64jsf.html');
INSERT INTO OE.Catalog VALUES('3', 'Oracle Magazine', 'Oracle Publishing', 'March-April 2005', 'Starting with Oracle ADF ', 'Steve Muench', 'http://www.oracle.com/technology/oramag/oracle/05-mar/o25window.html');
```

## 通过绑定行创建面板网格

在本节中，我们将通过将行绑定到面板网格来创建面板网格。首先，我们将通过指定面板网格中的列数来创建空面板网格。随后，我们将从 JSF HTML 组件调色板向空面板网格添加 JSF UI 组件。添加到空面板网格的组件会自动按指定的列数添加。要创建 JSF 面板网格，只需指定列数即可。

在本节中，我们将生成输出标签、输入文本字段、输出文本字段和命令按钮的面板网格。我们将向 JSF 页面添加面板网格，并从组件调色板向面板网格添加 UI 组件。我们将从期刊目录的字段创建面板网格。Catalog 表中的示例期刊目录具有目录 id、期刊、出版商、版次、标题、作者和 url href 字段。在 JSF 应用程序中，各种输出文本字段的值是从 Oracle 数据库为指定的输入目录 id 检索的。首先，创建一个用于创建面板网格的项目。

![](img/553e44bc30e70d0d8e58278a7ece8da8_319_0.png)

接下来，使用 文件 | 新建 向项目添加 JSF 页面。在 新画廊 窗口中，在 类别 中选择 Web Tier | JSF，在 项目 中选择 JSF JSP。在 Create JSF JSP 向导中，在 Web 应用程序窗口中选择 J2EE 1.4 并单击 下一步。在 JSP 文件窗口中指定 文件名 并单击 下一步。在 组件绑定 窗口中选择 自动地在新的托管 bean 中公开 UI 组件。后台 bean 名称、类和包已预先指定。单击 下一步。

![](img/553e44bc30e70d0d8e58278a7ece8da8_319_1.png)

## 创建 JSF 面板网格

在 **Error Page Options** 窗口中，选择默认设置，即不使用错误页面，然后单击 **Next**。在 **Tag Libraries** 窗口中，默认库（JSF Core 1.0 和 JSF HTML 1.0）默认被选中。单击 **Next**。在 **HTML Options** 窗口中，选择默认设置并单击 **Next**。在 **Finish** 窗口中，单击 **Finish**。JSF 页面被添加到项目中。向 JSF JSP 页面添加 **JDeveloper** CSS 样式表。添加一个 **Heading 2** 标题 **JSF Panel Grid**。接下来，向 JSF 页面添加面板网格。将光标定位在 JSF 页面标题下方并在 JSF HTML 组件调色板中选择 Panel Grid。

![](img/553e44bc30e70d0d8e58278a7ece8da8_320_0.png)

在 **Create PanelGrid Wizard** 中，单击 **Next**。在 **PanelGrid Options** 窗口中选择 **Create empty panel grid** 单选按钮并指定面板网格中的 **Number of Columns**，示例 JSF 应用程序中为 2。单击 **Finish.**

![](img/553e44bc30e70d0d8e58278a7ece8da8_321_0.png)

空 JSF 面板网格被添加到 JSF 页面。接下来，我们将从组件调色板向面板网格添加组件。添加到空面板网格的组件按两列排列。将光标定位在面板网格中并在组件调色板中选择 Output Label。

![](img/553e44bc30e70d0d8e58278a7ece8da8_321_1.png)

## 创建 JSF 面板网格

向 JSF 页面添加了输出标签。使用右箭头键将光标定位到输出标签右侧并在组件调色板中选择 Input Text。

![](img/553e44bc30e70d0d8e58278a7ece8da8_322_0.png)

输入文本字段被添加到 JSF 页面。要向 JSF 页面添加另一个组件，使用右箭头键将光标定位到前一个组件右侧。接下来，添加一个输出标签。由于面板网格只有两列，输出标签被添加到新行中。

![](img/553e44bc30e70d0d8e58278a7ece8da8_322_1.png)
