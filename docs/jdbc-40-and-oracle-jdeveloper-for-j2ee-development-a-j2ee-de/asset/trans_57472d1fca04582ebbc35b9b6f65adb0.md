
```
CREATE TABLE OE.Catalog(CatalogId VARCHAR(25), Journal VARCHAR(25),
    Publisher Varchar(25),Edition VARCHAR(25), Title Varchar(45),
    Author Varchar(25));
INSERT INTO OE.Catalog VALUES('catalog1', 'Oracle Magazine', 'Oracle
    Publishing', 'July-August 2005', 'Tuning Undo Tablespace',
    'Kimberly Floss');
INSERT INTO OE.Catalog VALUES('catalog2', 'Oracle Magazine', 'Oracle
    Publishing','March-April 2005', 'Starting with Oracle ADF ', 'Steve
    Muench');
```

从 JDeveloper IDE 创建到 Oracle 数据库的 JDBC 连接。要创建连接，请选择 Connections 选项卡，在 Connections Navigator 中选择 Database 节点，右键单击 Database 节点，然后选择 New Database Connection。在显示的 Create Database Connection (Type) 窗口中，指定 Connection Name 并将 Connection Type 设置为 Oracle(JDBC)。

![](img/553e44bc30e70d0d8e58278a7ece8da8_346_0.png)

单击 Next。在 Authentication 窗口中，指定 Username 和 Password 并单击 Next。在 Connection 窗口中，指定 Driver type、Host Name、JDBC Port、SID 并单击 Next。

![](img/553e44bc30e70d0d8e58278a7ece8da8_347_0.png)

在 **Test** 窗口中，单击 **Test Connection**。如果与数据库的连接已建立，则会显示成功消息。单击 **Finish** 完成连接配置。连接节点将被添加到 **Connections Navigator** 中。JDBC 连接 `JasperReportsConnection` 可作为 JNDI 资源 `jdbc/JasperReportsConnectionDS` 使用。该数据源将用于从数据库检索数据以生成 JasperReports 报表。

![](img/553e44bc30e70d0d8e58278a7ece8da8_347_1.png)

我们还需要下载并安装 Adobe Acrobat Reader 用于 PDF 报表，以及 MS Excel 用于 Excel 报表。

## 安装 JasperReports

在本节中，在 JDeveloper IDE 中创建 JasperReports 项目并创建 JasperReports 报表所需的库。首先，在 JDeveloper IDE 中选择 File | New。在 New Gallery 窗口中，在列出的 Categories 中选择 General，并在列出的 Items 中选择 Application。单击 OK 按钮。在 Create Application 窗口中指定 Application Name。单击 OK 按钮。在 Create Project 窗口中指定 Project Name 并单击 OK 按钮。JasperReports 应用程序和项目将被添加到 Applications-Navigator 中。

![](img/553e44bc30e70d0d8e58278a7ece8da8_348_0.png)

接下来，将生成 JasperReports 报表所需的库添加到项目中。在 Applications-Navigator 中选择 JasperReports 项目节点并选择 Tools | Project Properties。在 Project Properties 窗口中选择 Libraries 节点。使用 Add Library 按钮添加库，并使用 Add Jar/Directory 按钮添加 JAR/目录。添加以下表格中列出的库/JAR 文件。

| 库/Jar/Zip | 描述 |
|---|---|
| jasperreports-2.0.5.jar | JasperReports API. |
| Commons BeanUtils | JavaBeans 实用程序类 |
| Commons Collections | 集合框架扩展类 |
| Commons Digester 1.8 jar | 用于处理 XML 文档的类 |
| Commons Logging | 日志类 |
| poi-bin-3.0.2-FINAL-20080204.jar | 用于生成 Excel 文档的 Jakarta POI API |
| itext-2.1.0 | PDF 库 |
| Oracle XML Parser v2 | XML 解析器 API |

创建的库列在 **Libraries** 窗口中。在 **Project Properties** 窗口中单击 **OK** 按钮。

![](img/553e44bc30e70d0d8e58278a7ece8da8_349_0.png)

## 配置 JasperReports 配置文件

JasperReports 报表设计在 XML 配置文件中指定。本节中，XML 配置文件 `catalog.xml` 被配置为 PDF 报表。JasperReports 配置文件是基于 `jasperreport.dtd` DTD 的 XML 文件。配置文件的根元素是 `jasperReport`。JasperReports XML 文件中其他一些元素（带常用子元素和属性）在以下表格中列出：

| XML 元素 | 描述 | 子元素 | 属性 |
|---|---|---|---|
| jasperReport | 根元素 | reportFont, parameter, queryString, field, variable, group, title, pageHeader, columnHeader, detail, columnFooter, pageFooter. | name, columnCount, pageWidth, pageHeight, orientation, columnWidth, columnSpacing, leftMargin, rightMargin, topMargin, bottomMargin. |
| reportFont | 报表级字体定义。 | Report level font definitions. | name, isDefault, fontName,size, isBold, isItalic, isUnderline, isStrikeThrough, pdfFontName, pdfEncoding, isPdfEmbedded |
| parameter | 用于生成报表时引用的对象引用。引用格式为 P${name} | parameterDescription, defaultValueExpression. | name,class |
| queryString | 指定用于从数据库检索数据的 SQL 查询。 | - | - |
| field | 包含在报表中的数据库表列。引用格式为 F${name} | fieldDescription | name,class |
| variable | 报表 XML 文件中使用的变量。引用格式为 V${name} | variableExpression, initialValueExpression | name,class |
| title | 报表标题。 | band | - |
| pageHeader | 页眉 | band | - |
| columnHeader | 指定报表中的不同列。 | band | - |
| detail | 指定列值。 | band | - |
| columnFooter | 列页脚 | band | - |
| pageFooter | 页脚。 | band | - |

报表中的 band 代表报表的一个区段。`band` 元素包含 `staticText` 和 `textElement` 元素。`staticText` 元素用于向报表添加静态文本，例如列标题。`textElement` 元素用于向报表添加动态生成的文本，例如从数据库表检索的列值。在 *生成 PDF 报表* 部分，从示例 `Catalog` 表生成 JasperReports PDF 报表。示例 JasperReports 配置 XML 文件 `catalog.xml` 的 `queryString` 指定了用于检索报表数据的 SQL 查询。

```
<queryString><![CDATA[SELECT CatalogId, Journal, Publisher, Edition, Title, Author FROM OE.Catalog]]></queryString>
```

`reportElement` 元素指定报表中使用的 `ARIAL_NORMAL`、`ARIAL_BOLD` 和 `ARIAL_ITALIC` 字体。PDF 报表具有列 `CatalogId`、`Journal`、`Publisher`、`Edition`、`Title`、`Author`。JasperReports 配置文件 `catalog.xml` 如下所示。

```
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE jasperReport PUBLIC "-//JasperReports//DTD Report Design//EN"
"http://jasperreports.sourceforge.net/dtds/jasperreport.dtd">
<jasperReport name="PDFReport" pageWidth="1250">
<!-- Specify report fonts. -->
<reportFont name="Arial_Normal" isDefault="true" fontName="Arial" size="15" isBold="false" isItalic="false" isUnderline="false" isStrikeThrough="false" pdfFontName="Helvetica" pdfEncoding="Cp1252" isPdfEmbedded="false"/>
<reportFont name="Arial_Bold" isDefault="false" fontName="Arial" size="15" isBold="true" isItalic="false" isUnderline="false" isStrikeThrough="false" pdfFontName="Helvetica-Bold" pdfEncoding="Cp1252" isPdfEmbedded="false"/>
<reportFont name="Arial_Italic" isDefault="false" fontName="Arial" size="12" isBold="false" isItalic="true" isUnderline="false" isStrikeThrough="false" pdfFontName="Helvetica-Oblique" pdfEncoding="Cp1252" isPdfEmbedded="false"/>
<!-- Specify report parameters. -->
<parameter name="ReportTitle" class="java.lang.String"/>
<!-- Specify SQL Query -->
<queryString><![CDATA[SELECT CatalogId, Journal, Publisher, Edition, Title, Author FROM Catalog]]></queryString>
<field name="CatalogId" class="java.lang.String"/>
<field name="Journal" class="java.lang.String"/>
<field name="Publisher" class="java.lang.String"/>
<field name="Edition" class="java.lang.String"/>
<field name="Title" class="java.lang.String"/>
```

```
<field name="Author" class="java.lang.String"/>
<!-- Specify report title.-->
<title>
    <band height="50">
        <textField>
            <reportElement x="350" y="0" width="200" height="50" />
            <textFieldExpression
                class="java.lang.String">$P{ReportTitle}
            </textFieldExpression>
        </textField>
    </band>
</title>
<!-- Specify page header.-->
<pageHeader>
    <band>
    </band>
</pageHeader>
<!-- Specify column headers.-->
<columnHeader>
    <band height="20">
        <staticText>
            <reportElement x="0" y="0" width="100" height="20"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Bold"/>
            </textElement>
            <text><![CDATA[CATALOG ID]]></text>
        </staticText>
        <staticText>
            <reportElement x="125" y="0" width="150" height="20"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Bold"/>
            </textElement>
            <text><![CDATA[JOURNAL]]></text>
        </staticText>
        <staticText>
            <reportElement x="300" y="0" width="150" height="20"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Bold"/>
            </textElement>
            <text><![CDATA[PUBLISHER]]></text>
        </staticText>
        <staticText>
            <reportElement x="475" y="0" width="150" height="20"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Bold"/>
            </textElement>
            <text><![CDATA[PUBLISHER]]></text>
        </staticText>
```
