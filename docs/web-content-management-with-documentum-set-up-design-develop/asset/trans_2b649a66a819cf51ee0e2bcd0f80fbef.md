
请注意，XDQL 通常在 XSL 样式表中使用，只要您需要在网页上显示来自 Docbase 的实时（动态）数据。如果没有 XDQL，过时的数据将通过静态网页呈现在网站上。

![](img/b18244e6621ecc39cef03ff6c0e55e7c_238_0.png)

图 15.15：XDQL 的工作原理

## 15.5 XSL 样式表中的 XDQL 示例

让我们通过一个简单的示例来了解 XDQL 如何在演示文件（XSL 样式表）中动态获取 Docbase 数据。

以下示例模板文件将通过 XSL 样式表进行转换：

```
<?xml version="1.0" encoding="UTF-8"?>
<ROOTTAG>
  <TITLEOFPAGE/>
  <SHORTDESC/>
</ROOTTAG>
```

此示例中的 XSL 样式表将用于创建上述模板 XML 文件的 HTML 转换。转换后的文件（HTML 网页呈现）将显示使用上述模板创建的内容文件的名称（object_name 属性）。

创建 XSL 文件（如图 15.16 和 15.17 所示），并将其导入 Docbase 中的以下位置：/Site Manager/Presentations/Editor/Custom_Presentations。

## 创建演示文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE xsl:stylesheet (View Source for full doctype...)>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:fo="http://www.w3.org/1999/XSL/Format"
xmlns:java="http://xml.apache.org/xalan/java" xmlns:xalan="http://xml.apache.org/xalan" exclude-result-prefixes="xalan java fo">
<xsl:output method="html" encoding="ISO-8859-1" indent="yes" />
<xsl:param name="DMS_SESSION_ID" select="'default value'" />
<xsl:param name="DMS_INPUT_OBJECT_ID" />
<xsl:strip-space elements="*"/>
<xsl:template match="*|@|comment()|processing-instruction()|text()">
  <xsl:copy>
    <xsl:apply-templates select="*|@|comment()|processing-instruction()|text()" />
  </xsl:copy>
</xsl:template>
<!-- variable storing the DQL query -->
<xsl:variable name="dqlQuery">
select r_object_id, object_name from dm_document where r_object_id = '
<xsl:value-of select="$DMS_INPUT_OBJECT_ID" />
'
</xsl:variable>
<!-- variable storing the DQL query results -->
<xsl:variable name="query_results">
  <xsl:call-template name="XDQL">
    <xsl:with-param name="dql" select="string($dqlQuery)" />
  </xsl:call-template>
</xsl:variable>
<!-- variable storing the 'object_name' attribute from query results -->
<xsl:variable name="ObjectName">
  <xsl:for-each select="xalan:nodeset($query_results)/xdql/object">
    <xsl:value-of select="object_name" />
  </xsl:for-each>
</xsl:variable>
<!-- Matching content XML file root element -->
<xsl:template match="ROOTTAG">
<table border="0">
  <tr>
    <th>File name:</th>
  </tr>
</tr>
  <tr>
    <td>
      <br />
    </td>
  </tr>
</tr>
  <!-- Displaying the name of the file 'object name' attribute -->
```

图 15.16：使用 XDQL 的示例 XSL 文件

```
<!-- Displaying the name of the file ('object_name' attribute) -->
<td>
  <b>
    <u>
        <xsl:value-of select="$ObjectName" />
      </u>
    </b>
  </td>
</tr>
<tr>
  <td>
      <br />
    </td>
  </tr>
<tr>
  <!-- Displaying short description -->
  <td>
    <xsl:copy-of select="SHORTDESC" />
  </td>
</tr>
<tr>
  <td>
      <br />
    </td>
  </tr>
</table>
</xsl:template>
<!-- Template code for integrating DQL queries in XSL Starts -->
<xsl:template name="XDQL">
  <xsl:param name="dql" />
  <xsl:param name="rootTag" select="string('xdql')" />
  <xsl:param name="UpperTags" select="string('false')" />
  <xsl:param name="MaxRows" select="10" />
  <xsl:param name="RowIDAttrName" select="ID" />
  <xsl:param name="RowsetTag" select="Row" />
  <xsl:param name="UseGivenCaseTagNames" select="true" />
  <xsl:param name="UseNullAttrIndicator" select="false" />
  <xsl:variable name="xdql" select="java:com.documentum.xml.xdql.DfXmlQuery.new()" />
  <xsl:variable name="init" select="java:init($xdql)" />
  <xsl:variable name="param" select="java:setDql($xdql,$dql)" />
  <xsl:variable name="param1" select="java:setRootNode($xdql,$rootTag)" />
  <xsl:variable name="param3" select="java:includeContent($xdql,false())" />
  <xsl:variable name="setContentEncoding" select="java:setContentEncoding($xdql,string('dom'))" />
  <xsl:variable name="setContentFormat" select="java:setContentFormat($xdql,string('xml'))" />
  <xsl:variable name="execute" select="java:execute($xdql, 'DF_READ_QUERY', $DMS_SESSION_ID)" />
  <xsl:variable name="queryresult" select="java:getXMLDOM($xdql)" />
  <xsl:copy-of select="$queryresult" />
</xsl:template>
<!-- Template code for integrating DQL queries in XSL Ends -->
</xsl:stylesheet>
```

图 15.17：使用 XDQL 的示例 XSL 文件——续

我们现在将讨论上述 XSL 样式表的结构：

- **Namespaces**：在演示文件的 `<xsl:stylesheet>` 标签中添加以下 Java 和 Xalan 命名空间。例如：
  ```xml
  <xsl:stylesheet version="1.0"
  xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
  xmlns:fo="http://www.w3.org/1999/XSL/Format"
  xmlns:java="http://xml.apache.org/xslt/java"
  xmlns:xalan="http://xml.apache.org/xalan" exclude-result-prefixes=
  "xalan java fo">
  ```
- **Parameters**：声明系统传递给样式表的参数。例如：
  ```xml
  <xsl:param name="DMS_SESSION_ID" select="'default value'"/>
  <xsl:param name="DMS_INPUT_OBJECT_ID"/>
  ```
- **XDQL classes**：使用 `<xsl:template>` 标签定义一个 XSL 模板，以实例化 `DFxmlQuery()` 类，传递 XDQL 参数，并在 XSL 样式表内部执行 DQL 查询。例如：
  ```xml
  <xsl:template name="XDQL">
  <xsl:param name="dql"/>
  <xsl:param name="roottag" select="string('dql')"/>
  <xsl:param name="UpperTags" select="string('false')"/>
  <xsl:param name="MaxRows" select="10"/>
  <xsl:param name="RowIDAttrName" select="ID"/>
  <xsl:param name="RowSetTag" select="Row"/>
  <xsl:param name="UseGivenCaseTagNames" select="true"/>
  <xsl:param name="UseNullAttrIndicator" select="false"/>
  <xsl:variable name="xdql" select="java:com.documentum.xml.xdql.DFxmlQuery.new()"/>
  <xsl:variable name="init" select="java:init($xdql)"/>
  <xsl:variable name="param" select="java:setDql($xdql,$dql)"/>
  <xsl:variable name="param1" select="java:setRootNode($xdql,$roottag)"/>
  <xsl:variable name="param3" select="java:includeContent($xdql,false())"/>
  <xsl:variable name="setContentEncoding" select="java:setContentEncoding($xdql,string('dom'))"/>
  <xsl:variable name="setContentFormat" select="java:setContentFormat($xdql,string('xml'))"/>
  <xsl:variable name="execute" select="java:execute($xdql, 'DF_READ_QUERY', $DMS_SESSION_ID)"/>
  <xsl:variable name="queryresult" select="java:getXMLDOM($xdql)"/>
  <xsl:copy-of select="$queryresult"/>
  </xsl:template>
  ```

DMS_SESSION_ID 是对当前 Docbase 会话的引用，而 DMS_INPUT_OBJECT_ID 是对当前对象 ID（r_object_id 属性）的引用。

建议查阅 com.documentum.xml.xdql.IDFxmlQuery 接口的 DFC Javadocs，以了解上文所示的 <xsl:template name="XDQL"> 中使用的众多方法的参数和返回类型。

变量：
定义一个变量来存储从样式表内部调用的 DQL 查询。
例如：
```xml
<xsl:variable name = "dqlQuery">select r_object_id, object_name from dm_document where r_object_id = '<xsl:value-of select="$DMS_INPUT_OBJECT_ID"/>' </xsl:variable>
```

此代码存储用于获取当前内容文件（即其对象 ID 与当前对象 ID DMS_INPUT_OBJECT_ID 匹配）的对象 ID（r_object_id 属性）和文件名（object_name 属性）的 DQL 查询字符串。

此外，通过调用预定义的 XDQL 模板并向其传递 DQL 查询字符串，定义一个变量来存储上述 DQL 查询的结果。例如：

```
<xsl:variable name = "query_results">
  <xsl:call-template name="XDQL">
    <xsl:with-param name="dql" select="string($dqlQuery)"/>
  </xsl:call-template>
</xsl:variable>
```

一旦将查询结果保存在变量中，就遍历它，检索所需的属性（在本例中为 object_name）并将其保存在变量中。

```
For example:
<xsl:variable name="ObjectName">
  <xsl:for-each select="xalan:nodeset($query_results)/xdql/object">
    <xsl:value-of select="object_name"/>
  </xsl:for-each>
</xsl:variable>
```

在 Web Publisher 中在 XSL 样式表中使用 XDQL 时，这就是需要处理的所有内容。在我们的示例中，显示内容文件的名称（object_name 属性）只需访问 objectName 变量的值即可。例如：

```
<!-- Displaying the name of the file (object_name attribute) -->
<td><b><u><xsl:value-of select="$objectName"/></u></b></td>
```

请注意，通过使用 $(variableName)，系统会在运行时替换变量中包含的值。

## 15.6 Create_Dynamic_Content 作业

使用 XDQL 查询的演示文件可以在以下情况下检索 Docbase 内容及其属性：

- 从模板创建新内容文件并在 Web Publisher 编辑器中保存
- 编辑并保存现有内容文件
- 对现有内容使用重新应用演示功能

然而，对于那些已发布并在网站上显示的活动内容文件，但其底层数据（属性或内容值）在 Docbase 中已被修改，会发生什么情况？内容作者是否必须再次编辑/保存并重新发布这些内容文件，以确保再次触发 XDQL 查询并在运行的网站上显示修改后的数据？答案是否定的。
