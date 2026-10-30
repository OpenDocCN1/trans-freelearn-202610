
## 13

## 使用 Apache POI 创建电子表格

数据库表数据通常需要以 Excel 电子表格的形式呈现。Apache POI 项目提供了用于处理 Microsoft OLE 2 复合文档格式文件的 Java API，其中包括 XLS 和 DOC 文件。Apache POI 项目的 **Apache POI HSSF** 组件是一个用于访问和生成 Excel 工作簿和 Excel 电子表格的 Java API。Apache POI HSSF 不支持新的 Excel 2007 .xlsx OOXML 文件格式，该格式不是基于 OLE2 的。Excel 工作簿由电子表格组成，而 Excel 电子表格由行和单元格组成。电子表格的布局和字体也可以通过 Apache POI HSSF Java API 设置。

Apache POI HSSF 项目是用于创建 Excel 工作簿和 Excel 电子表格的 Java API。Apache POI HSSF 项目的实现在 `org.apache.poi.hssf.usermodel` 包中提供。使用 POI HSSF 项目生成的 Excel 电子表格中的数据可能是静态数据，也可能是从数据库动态检索的数据。本章将讨论从 Oracle 数据库表创建 Excel 电子表格的步骤。Excel 电子表格在 JDeveloper IDE 中生成。

## 设置环境

生成 Excel 电子表格需要 `org.apache.poi.hssf.usermodel` 包中的类。从 `http://www.apache.org/dyn/closer.cgi/poi/` 下载 Apache POI 库的 `poi-bin-3.0.2-FINAL-20080204.zip` 文件并将其解压到安装目录。安装 JDeveloper 10g (10.1.3)。安装包含示例模式的 Oracle 数据库 10g。创建数据库实例。创建一个示例数据库表，将从中生成 Excel 电子表格。创建示例表 `catalog` 的 SQL 脚本如下所示：

```sql
CREATE TABLE OE.Catalog(CatalogId VARCHAR(25), Journal VARCHAR(25),
    Publisher Varchar(25),Edition VARCHAR(25), Title Varchar(255),
    Author Varchar(25));

INSERT INTO OE.Catalog VALUES('catalog1', 'Oracle Magazine',
    'Oracle Publishing', 'March-April 2005', 'Starting with Oracle ADF'
    , 'Steve Muench');

INSERT INTO OE.Catalog VALUES('catalog2', 'Oracle Magazine',
    'Oracle Publishing','Jan-Feb 2005', 'Understanding Optimization',
    'Kimberly Floss');
```

接下来，我们将创建一个用于生成 Excel 电子表格的 JDeveloper 项目，并向项目中添加 Apache POI JAR 文件的库。此外，我们将使用 Oracle 数据库创建 JDBC 连接以检索用于 Excel 电子表格的数据。

首先，从 JDeveloper IDE 创建到 Oracle 数据库的 JDBC 连接。要创建连接，请选择 Connections 选项卡，在 Connections Navigator 中选择 Database 节点，右键单击 Database 节点，然后选择 New Database Connection。在显示的 Create Database Connection (Type) 窗口中，指定 Connection Name 并将 Connection Type 设置为 Oracle(JDBC)。

![](img/553e44bc30e70d0d8e58278a7ece8da8_366_0.png)

单击 **Next**。在 **Authentication** 窗口中，指定 **Username** 和 **Password**，然后单击 **Next**。在 **Connection** 窗口中，指定 **Driver type**、**Host Name**、**JDBC Port**、**SID**，然后单击 **Next**。

![](img/553e44bc30e70d0d8e58278a7ece8da8_367_0.png)

在 **Test** 窗口中，单击 **Test Connection**。如果与数据库的连接已建立，则会显示成功消息。单击 **Finish** 完成连接配置。连接节点将被添加到 **Connections Navigator**。

![](img/553e44bc30e70d0d8e58278a7ece8da8_367_1.png)

JDBC 连接 `ApachePOIConnection` 可作为数据源 `jdbc/ApachePOIConnectionDS` 使用。该数据源用于从数据库检索数据以生成 Excel 电子表格。接下来，创建一个用于生成 Excel 电子表格的 JDeveloper 项目。在 JDeveloper IDE 中选择 **File | New**。在 **New Gallery** 窗口中，在列出的 **Categories** 中选择 **General | Applications**，并在列出的 **Items** 中选择 **Application**。单击 **OK** 按钮。在 **Create Application** 窗口中，指定 **Application Name**，选择 **No Template Application Template**，然后单击 **OK** 按钮。在 **Create Project** 窗口中，指定 **Project Name** 并单击 **OK** 按钮。应用程序和项目将被添加到 **Applications Navigator.**

![](img/553e44bc30e70d0d8e58278a7ece8da8_368_0.png)

接下来，向项目添加 Apache POI JAR 文件的库。在 **Applications Navigator** 中选择 Apache POI 项目节点。选择 **Tools | Project Properties**。在 **Project Properties** 窗口中，选择 **Libraries** 节点。单击 **Add Library** 按钮创建新库。在 **Add Library** 窗口中单击 **New** 按钮添加新库。在 **Create Library** 窗口中指定 **Library Name** 并单击 **Add Entry** 按钮。

在 **Select Path Entry** 窗口中选择 `poi-3.0.2-FINAL-20080204.jar` 文件并单击 **Select** 按钮。所选 JAR 文件将被添加到 **Create Library** 窗口中的 **Class Path** 节点。在 **Create Library** 窗口中单击 **OK** 按钮。Apache POI 库被添加到 **Add Library** 窗口。在 **Add Library** 窗口中单击 **OK** 按钮。Apache POI 库已创建并添加到 **Libraries**。`poi-3.0.2-FINAL-20080204.jar` 文件也可以使用 **Add Jar/Directory** 按钮添加到 ApachePOI 项目。

![](img/553e44bc30e70d0d8e58278a7ece8da8_369_0.png)

## 创建 Excel 电子表格

在本节中，将从示例数据库表生成 Excel 电子表格。首先，创建一个 JSP 应用程序以生成 Excel 电子表格。在 **Applications Navigator** 中右键单击 ApachePOI 项目节点并选择 **New**。在 **New Gallery** 窗口中选择 **Web Tier | Java Server Pages**。在列出的 **Items** 中，选择 **JSP Page** 并单击 **OK** 按钮。在 **Create JSP Wizard** 中选择 **Web Application** 版本并单击 **Next** 按钮。在 **JSP File** 窗口中指定文件名并单击 **Next**。在 **Tag Libraries** 窗口中单击 **Next** 按钮。在 **HTML Options** 页面中选择默认选项并单击 **Next** 按钮。在 **Finish** 窗口中，单击 **Finish**。JSP 被添加到 **Applications Navigator** 中的 Apache POI 项目。

![](img/553e44bc30e70d0d8e58278a7ece8da8_369_1.png)

向 JSP `web.xml` 配置文件添加 JDBC 数据源 `jdbc/ApachePOIConnectionDS` 的资源引用，该数据源已在上一节中配置。向 `web.xml` 文件添加 `resource-ref` 元素。

```
<resource-ref>
    <res-ref-name>jdbc/ApachePOIConnectionDS</res-ref-name>
    <res-type>javax.sql.DataSource</res-type>
    <res-auth>Container</res-auth>
</resource-ref>
```

在 `catalog.jsp` 中，将从 Oracle 数据库表创建 Excel 电子表格。使用 Apache POI HSSF 项目生成 Excel 电子表格。Apache POI HSSF 包具有用于 Excel 电子表格不同组件的类。Apache POI HSSF 包中一些常用的类列在下表中。

| 类名 | 描述 |
|---|---|
| HSSFWorkbook | 表示 Excel 电子表格工作簿。 |
| HSSFSheet | 表示 Excel 电子表格。 |
| HSSFHeader | 指定电子表格页眉。 |
| HSSFRow | 指定电子表格行。 |
| HSSFCell | 指定电子表格单元格 |
| HSSFCellStyle | 指定单元格样式 |
| HSSFFont | 指定电子表格的字体 |
| HSSFChart | 表示图表 |
| HSSFDateUtil | 指定 Excel 日期 |
| HSSFPrintSetup | 指定 Excel 文档的打印设置 |

在 JSP 应用程序中，首先导入 Apache POI HSSF 包：

```
<%@ page import="org.apache.poi.hssf.usermodel.*, java.sql.*, java.io.*,javax.naming.InitialContext"%>
```

使用 `HSSFWorkbook` 类创建 Excel 电子表格工作簿：

```
HSSFWorkbook wb=new HSSFWorkbook();
```

接下来，使用 `createSheet()` 方法从 `HSSFWorkbook` 对象创建 Excel 电子表格：

```
HSSFSheet sheet1=wb.createSheet("sheet1");
```
