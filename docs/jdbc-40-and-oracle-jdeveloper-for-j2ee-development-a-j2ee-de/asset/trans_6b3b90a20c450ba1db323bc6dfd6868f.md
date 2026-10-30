
从 XML 文档 catalog.xml 创建一个 InputStream。

```
InputStream input=new FileInputStream(new File("catalog.xml"));
```

使用 insertXML(InputStream) 方法将示例 XML 文档映射到数据库表。insertXML() 方法被重载，输入可以指定为 Document 对象、InputStream 对象、Reader 对象、String 对象或 URL 对象：

```
oracleXMLSave.insertXML(input);
```

XSLT input.xslt 会应用于 XML 文档。XML 文档（其属性已转换为元素）会被存储到数据库中。使用 close() 方法关闭 OracleXMLSave 对象：

```
oracleXMLSave.close();
```

XMLToDatabase 应用程序在章节后面完整列出。可以通过注释掉不需要的方法来运行选定的操作。例如，要将 XML 文档映射到数据库表，请注释掉除 xmlToSQL() 方法之外的所有方法的调用。要运行 XMLToDatabase.java 应用程序，在 JDeveloper 中右键单击应用程序节点并选择 Run。

![](img/553e44bc30e70d0d8e58278a7ece8da8_205_0.png)

XML 文档 input.xml 被映射到 Oracle 数据库表 JOURNAL。

## 从 XML 文档更新数据库

在本节中，我们将使用 OracleXMLSave 类从 XML 文档更新数据库表，并使用 Connection 对象创建一个 OracleXMLSave 对象。JOURNAL 是需要修改的数据库表。

```
OracleXMLSave oracleXMLSave =new OracleXMLSave(conn, "JOURNAL");
```

使用 setRowTag(String) 方法设置行包围元素标签，并将行标签设置为 journal。还将忽略大小写设置为 true。

```
oracleXMLSave.setRowTag("journal");
oracleXMLSave.setIgnoreCase(true);
```

使用 setKeyColumnList(String[]) 方法设置键列，并将 CATALOGID 设置为键列。

```
String [] keyColNames = new String[1];
keyColNames[0] = "CATALOGID";
oracleXMLSave.setKeyColumnList(keyColNames);
```

指定要更新的列数组，并使用 setUpdateColumnList(String[]) 方法指定要更新的列。我们将使用 XML 文档修改 EDITION 列和 TITLE 列。

```
String[] updateColNames = new String[2];
updateColNames[0] = "EDITION";
updateColNames[1] = "TITLE";
oracleXMLSave.setUpdateColumnList(updateColNames);
```

数据库表中要更新的数据在 XML 文档中指定。XML 文档 catalog-update.xml 指定了一个 catalogId 为 "catalog2" 的 journal 元素。仅当表 JOURNAL 中行 catalogId 包含 catalog2 时，才会修改 EDITION 和 TITLE 列。catalog-update.xml 如下所示：

```
<?xml version="1.0" encoding="utf-8"?>
<catalog>
<journal>
<catalogId>catalog2</catalogId>
<journal_title>Oracle Magazine</journal_title>
<publisher>Oracle Publishing</publisher>
<edition>September-October 2005</edition>
<article_section>Developer</article_section>
<title>Creating Search Pages</title>
<author>Steve Muench</author>
</journal>
</catalog>
```

从 catalog-update.xml 创建一个 InputStream 对象。

```
InputStream input=new FileInputStream(new File("catalog-update.xml"));
```

使用 updateXML(InputStream) 方法更新数据库表 JOURNAL。
updateXML() 方法被重载，数据库表行可以从以下之一更新：Document 对象、InputStream 对象、Reader 对象、String 对象或 URL 对象。使用 close() 方法关闭 OracleXMLSave 对象。

```
oracleXMLSave.updateXML(input);
oracleXMLSave.close();
```

要更新数据库表 JOURNAL，请在 XMLToDatabase.java 应用程序中注释掉除 updateDatabase() 方法之外的所有方法调用。结果，数据库表 JOURNAL 得到更新。

## 在数据库中删除一行

我们将使用 XML SQL 实用程序删除数据库表行。可以通过在 XML 文档中指定对应于键列的元素标签来删除数据库表行。示例 XML 文档 catalog-delete.xml 指定了 catalogId 为 "catalog2"。我们将从表 JOURNAL 中删除主键列 CATALOGID 的值为 "catalog2" 的行，如 catalog-delete.xml 所示：

```
<?xml version="1.0" encoding="utf-8"?>
<catalog>
<journal>
<catalogId>catalog2</catalogId>
</journal>
</catalog>
```

我们必须创建一个 OracleXMLSave 对象，设置行包围标签并将忽略大小写设置为 true，如上一节所述。

```
OracleXMLSave oracleXMLSave =new OracleXMLSave(conn, "JOURNAL");
oracleXMLSave.setRowTag("journal");
oracleXMLSave.setIgnoreCase(true);
```

使用 setKeyColumnList(String[] keyColumns) 方法设置键列，并将 CATALOGID 设置为键列。

```
String [] keyColNames = new String[1];
keyColNames[0] = "CATALOGID";
oracleXMLSave.setKeyColumnList(keyColNames);
```

从 catalog-delete.xml 文档创建 InputStream。

```
InputStream input=new FileInputStream(new File("catalog-delete.xml"));
```

使用 deleteXML(InputStream) 方法删除数据库表行。deleteXML() 方法被重载，数据库表行可以使用以下之一删除：Document 对象、InputStream 对象、Reader 对象、String 对象或 URL 对象。使用 close() 方法关闭 OracleXMLSave 对象。

```
oracleXMLSave.deleteXML(input);
oracleXMLSave.close();
```

注释掉 XMLToDatabase.java 应用程序中除 deleteRow() 方法之外的所有方法调用，以使用 catalog-delete.xml 从 JOURNAL 表中删除一行。结果，catalog-delete.xml 中指定的数据库表行将被从 JOURNAL 表中删除。

## SQL 数据库到 XML 文档映射

我们将使用 XML SQL 实用程序将数据库表 JOURNAL 映射到 XML 文档。OracleXMLQuery 类用于将数据库表转换为 XML 文档。在生成的 XML 文档中，会为数据库表的每一列创建相应的 XML 元素。元素属性不会被创建，若要创建它们，请对使用 OracleXMLQuery 类从数据库表创建的 XML 文档应用 XSLT。

本节讨论创建包含元素和元素属性的 XML 文档的步骤。导入 OracleXMLQuery 类并创建 OracleXMLQuery 类对象：

```
OracleXMLQuery query = new OracleXMLQuery(conn, "SELECT CATALOGID, JOURNAL_TITLE, PUBLISHER, EDITION, ARTICLE_SECTION, TITLE, AUTHOR FROM JOURNAL");
```

变量 conn 是用于查询数据库的 JDBC 连接。SELECT SQL 语句指定了从数据库表 JOURNAL 选择数据的查询。对 OracleXMLQuery 对象应用 XSLT 以生成包含元素属性的 XML 文档。使用 setXSLT(Reader, String) 方法在 OracleXMLQuery 对象上设置 XSLT。String 参数指定外部实体的 URL。

```
Reader xsltReader=new FileReader(new File("output.xsl"));
query.setXSLT(xsltReader, null);
```

XSLT output.xsl 如下所示：

```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0"
                xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
    <xsl:output method="xml"/>
    <xsl:template match="/ROWSET">
        <xsl:element name="CATALOG">
            <xsl:apply-templates select="journal"/>
        </xsl:element>
    </xsl:template>
    <xsl:template match="journal">
        <xsl:element name="journal">
            <xsl:attribute name="title">
                <xsl:value-of select="JOURNAL_TITLE"/>
            </xsl:attribute>
            <xsl:attribute name="publisher">
                <xsl:value-of select="PUBLISHER"/>
            </xsl:attribute>
            <xsl:attribute name="edition">
                <xsl:value-of select="EDITION"/>
            </xsl:attribute>
            <xsl:element name="catalogId">
                <xsl:value-of select="CATALOGID"/>
            </xsl:element>
            <xsl:element name="article">
                <xsl:attribute name="section">
                    <xsl:value-of select="ARTICLE_SECTION"/>
                </xsl:attribute>
                <xsl:copy-of select="TITLE"/>
                <xsl:copy-of select="AUTHOR"/>
            </xsl:element>
        </xsl:element>
    </xsl:template>
</xsl:stylesheet>
```
