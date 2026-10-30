
在 JDBC 3.0 中，JDBC 驱动程序仅支持 Unicode 字符集。SQL: 2003 标准增加了对 SQL 类型 NCHAR、NVARCHAR、LONGNVARCHAR 和 NCLOB 的支持，其中值使用国家字符集 (NCS) 编码。使用 JDBC 3.0 驱动程序时，国家字符集 SQL 数据类型值会被转换为 Unicode 字符集值。如果需要进行大量的字符处理操作，NCS 数据类型可能更合适。对国家字符集数据库数据类型 NCHAR、NVARCHAR、LONGNVARCHAR 和 NCLOB 的支持已在 JDBC 4.0 中添加。

Setter 方法 setNString()、setNCharacterStream() 和 setNClob() 已添加到 PreparedStatement 和 CallableStatement 接口。Getter 方法 getNString()、getNCharacterStream() 和 getNClob() 已添加到 CallableStatement 和 ResultSet 接口。Updater 方法 updateNString()、updateNCharacterStream() 和 updateNClob() 已添加到 ResultSet 接口。为了创建 NClob 对象，createNClob() 方法已添加到 Connection 接口。Reader 方法 readNString() 和 readNClob() 已添加到 SQLInput 接口，用于读取 NCHAR、NVARCHAR、LONGNVARCHAR 和 NCLOB 值。Writer 方法 writeNClob() 和 writeNString() 已添加到 SQLOutput 接口，用于写入 NCHAR、NVARCHAR、LONGNVARCHAR 和 NCLOB 值。

在 JDBC 4.0 中，对 BLOB 和 CLOB SQL 数据类型的支持得到了增强。为了创建 java.sql.Blob 和 java.sql.Clob 对象，createBlob() 和 createClob() 等方法已添加到 Connection 接口。在 PreparedStatement 和 CallableStatement 接口中，setBlob() 方法已被重载以从 InputStream 设置 Blob 值，setClob() 方法已被重载以从 Reader 设置 Clob 值。在 ResultSet 接口中，updater 方法 updateBlob() 已被重载以从 InputStream 更新列，updater 方法 updateClob() 已被重载以从 Reader 更新列。为了释放 Blob 和 Clob 对象中的资源，free() 方法已添加到 Blob 和 Clob 接口。

CallableStatement 和 PreparedStatement 接口中的 setAsciiStream、setBinaryStream 和 setCharacterStream 方法已被重载。这些接口已被重载以支持类型为 long 的 length 参数，除类型为 int 的 length 参数之外。length 参数指定 InputStream 或 Reader 对象的字节数或字符数。此外，CallableStatement 和 PreparedStatement 接口中的 setAsciiStream、setBinaryStream 和 setCharacterStream 方法已被重载为不带 length 参数的版本。

CallableStatement 和 PreparedStatement 接口中的 setBlob() 方法已被重载为另外两种方法，以从 InputStream 对象设置参数值，一种带有用于二进制流长度的 length 参数，另一种不带 length 参数。如果 InputStream 长度与指定的长度不匹配，则会生成 SQLException。CallableStatement 和 PreparedStatement 接口中的 setClob() 方法已被重载为另外两种方法，以从 Reader 对象设置参数值，一种带有用于 Reader 对象中字符数的 length 参数，另一种不带 length 参数。

如果 Reader 对象中的字符数与指定的长度不匹配，则会生成 SQLException。与 PreparedStatement/CallableStatement 中的 setter 方法类似，ResultSet 接口中的 updateAsciiStream、updateBinaryStream、updateBlob、updateCharacterStream 和 updateClob 方法已被重载。与 PreparedStatement 和 CallableStatement 接口的 setBlob 和 setClob 方法不同，如果 InputStream/Reader 长度与指定的长度不匹配，ResultSet 接口的 updateBlob 和 updateClob 方法不会生成 SQLException。

## SQL: 2003 XML 数据类型支持

SQL: 2003 标准支持一种新的数据类型 XML，用于存储 XML 文档。有了 XML 数据类型，XML 文档可以像其他数据类型一样存储在数据库表列中。JDBC 4.0 支持 SQL: 2003 标准。java.sql.SQLXML 对象是数据库类型 XML 的 Java 映射。在 SQLXML Java 数据类型之前，XML 类型列值只能作为 String 或 CLOB 检索，这不包括访问 XML 文档中不同节点的功能。

在 SQLXML 数据类型的帮助下，XML 类型数据库列可以映射到 Java 数据类型。在 JDBC 4.0 规范中，java.sql.Connection 对象具有创建初始不包含任何数据的 SQLXML 对象的 provision。数据可以通过 setString() 方法或 setBinaryStream()、setCharacterStream() 和 setResult() 方法添加。SQLXML 对象可以通过使用重载的 getSQLXML() 方法从 ResultSet 或 CallableStatement 检索。SQLXML 对象中的数据可以通过 getString() 方法或 getBinaryStream()、getCharacterStream() 和 getSource() 方法检索。SQLXML 对象可以使用 PreparedStatement 接口的 setSQLXML() 方法存储在类型为 XML 的数据库表列中，这与任何其他数据类型类似。

SQL Server 2005 EXPRESS 支持 XML 数据类型，而 SQL Server 2000 不支持。IBM 的 DB2 UDB V 9 也支持 XML 数据类型。要确定数据库是否支持 XML 数据类型，请从 Connection 对象获取数据库元数据：

```
DatabaseMetaData metadata= connection.getMetaData();
```

使用 getTypeInfo() 方法支持的数据类型，如下所示：

```
ResultSet rs=metadata.getTypeInfo();
```

遍历数据类型结果集并输出 TYPE_NAME 列，如下所示：

```
System.out.println("TYPE_NAME:"+rs.getString("TYPE_NAME")) ;
```

对于 SQL Server 2005 和 IBM 的 DB2 UDB v9，会输出 XML TYPE_NAME：

TYPE_NAME: XML

在以下小节中，将讨论创建 XML 文档、将其存储在支持 XML 数据类型的数据库中以及从数据库检索它的过程。

### 生成 XML 文档

我们将讨论创建和初始化 SQLXML 对象的过程。导入 java.sql 包和 javax.xml.stream 包：

```
import java.sql.*;
import javax.xml.stream.*;
```

数据库表中 XML 文档的 Java 表示形式是 SQLXML 对象。使用 createSQLXML() 方法从 Connection 对象创建 SQLXML 对象，如下所示：

```
SQLXML sqlXML=connection.createSQLXML();
```

SQLXML 对象可以使用 setString()、setBinaryStream()、setCharacterStream() 或 setResult() 方法之一进行初始化。SQLXML 对象可以使用 setResult() 方法和 StAXResult 类进行初始化。从 StAXResult 对象创建 XMLStreamWriter 对象，如下所示：

```
StAXResult staxResult = sqlXML.setResult(StAXResult.class);
XMLStreamWriter xmlStreamWriter = staxResult.getXMLStreamWriter();
```

调用 setResult() 方法后，SQLXML 对象变为不可写。使用 writeStartDocument(String,String) 方法添加 XML 文档的开始，如下所示：

```
xmlStreamWriter.writeStartDocument("UTF-8","1.0");
```

XML 文档的编码和版本在 writeStartDocument 方法中指定。使用 writeStartElement (String localName) 方法添加元素的开始，如下所示：

```
xmlStreamWriter.writeStartElement("catalog");
```

使用 writeAttribute(String localName, String value) 方法添加元素属性。使用 writeCharacters(String text) 方法添加文本元素。每个开始元素都有一个对应的结束元素标签。使用 writeEndElement() 方法添加结束元素。writeEndElement() 方法不指定元素名称，如 writeStartElement(String) 方法：

```
xmlStreamWriter.writeEndElement();
```

使用 writeEndDocument() 方法添加文档的结束：

```
xmlStreamWriter.writeEndDocument();
```

SQLXML 对象也可以使用 SAXResult 类进行初始化。使用 SQLXML 接口的 setResult() 方法创建 SAXResult 对象。随后，使用 getHandler() 方法获取 ContentHandler 结果：

```
SAXResult saxResult = sqlXML.setResult(SAXResult.class);
ContentHandler contentHandler= saxResult.getHandler();
```

使用 startDocument() 方法指定 XML 文档的开始：

```
contentHandler.startDocument();
```

使用 startElement(String uri,String localName,String qName,Attributes atts) 方法指定元素的开始，其中参数 uri 指定元素命名空间，参数 localName 指定元素本地名称，参数 qName 指定元素限定名称，参数 atts 类型为 Attributes 指定元素属性。Attributes 对象可以使用实现 Attributes 接口的 org.xml.sax.helpers.AttributesImpl 类创建。可以使用 addAttribute(String uri, String localName, String qName, String type, String value) 方法向 AttributesImpl 对象添加属性：

```
AttributesImpl attrs=new AttributesImpl();
attrs.addAttribute("","","journal","StringType","OracleMagazine");
contentHandler.startElement("","","catalog",attrs);
```
