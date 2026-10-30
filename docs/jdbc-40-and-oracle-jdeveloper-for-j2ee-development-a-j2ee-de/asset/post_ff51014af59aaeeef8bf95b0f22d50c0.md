
In JDBC 3.0, JDBC drivers supported only Unicode character set. SQL: 2003 standard has added support for SQL types, NCHAR, NVARCHAR, LONGNVARCHAR, and NCLOB in which values are encoded using the National Character Set (NCS). The National Character Set SQL data type values were converted to the Unicode Character Set values with a JDBC 3.0 driver. The NCS data types can be more suitable if extensive character processing operations are required. Support for National Character Set database data types, NCHAR, NVARCHAR, LONGNVARCHAR, and NCLOB have been added in JDBC 4.0.

Setter methods, setNString(), setNCharacterStream(), and setNClob() have been added to the PreparedStatement and CallableStatement interfaces. Getter method, getNString(), getNCharacterStream(), and getNClob() have been added to the CallableStatement and ResultSet interfaces. Updater methods, updateNString(), updateNCharacterStream(), and updateNClob() have been added to the ResultSet interface. To create a NClob object, createNClob() method has been added to the Connection interface. Reader methods, readNString() and readNClob() have been added to the SQLInput interface to read the NCHAR, NVARCHAR, LONGNVARCHAR, and NCLOB values. Writer methods, writeNClob() and writeNString() have been added to the SQLOutput interface to write the NCHAR, NVARCHAR, LONGNVARCHAR, and NCLOB values.

In JDBC 4.0, support for BLOB and CLOB SQL data types have been enhanced. To create java.sql.Blob and java.sql.Clob objects, methods such as createBlob() and createClob() have been added to the Connection interface. In the PreparedStatement and CallableStatement interfaces, setBlob() method has been overloaded to set the Blob values from an InputStream, and setClob() method has been overloaded to set the Clob values from a Reader. In the ResultSet interface, the updater method, updateBlob(), has been overloaded to update a column from an InputStream and the updater method, updateClob(), has been overloaded to update a column from a Reader. To free resources in Blob and Clob objects, a method, free(), has been added to the Blob and Clob interfaces.

The setAsciiStream, setBinaryStream, and setCharacterStream methods in the CallableStatement and PreparedStatement interfaces have been overloaded. These interfaces have been overloaded to support the length parameter of type long in addition to the length parameter of type int. The length parameter specifies the length in bytes or characters of the InputStream or Reader object. Also, the setAsciiStream, setBinaryStream, and setCharacterStream methods in the CallableStatement and PreparedStatement interfaces have been overloaded with versions without the length parameter.

The setBlob() method in the CallableStatement and PreparedStatement interfaces has been overloaded with the other two methods to set parameter values from the InputStream object, one with a length parameter for the length of the binary stream, and the other without a length parameter. If the InputStream length does not match the specified length, an SQLException is generated. The setClob() method in the CallableStatement and PreparedStatement interfaces has been overloaded with other two methods to set parameter values from the Reader object, one with a length parameter for the number of characters in the Reader object and the other without a length parameter.

If the number of characters in the Reader object does not match the specified length, an SQLException is generated. Similar to the setter methods in the PreparedStatement/CallableStatement, the updateAsciiStream, updateBinaryStream, updateBlob, updateCharacterStream, and updateClob methods in the ResultSet interface have been overloaded. Unlike the setBlob and setClob methods of the PreparedStatement and CallableStatement interfaces, the updateBlob and updateClob methods of the ResultSet interface do not generate an SQLException, if the InputStream/Reader length does not match the specified length.

## SQL: 2003 XML Data Type Support

The SQL: 2003 standard supports a new data type, XML, for storing XML documents. With the XML data type, an XML document can be stored in a database table column similar to the other data types. JDBC 4.0 supports the SQL: 2003 standard. The java.sql.SQLXML object is the Java mapping for the database type, XML. Prior to the SQLXML Java data type, an XML type column value could be retrieved only as a String or CLOB, which did not include the functionality to access different nodes in an XML document.

An XML type database column can be mapped to a Java data type with the help of SQLXML data type. In JDBC 4.0 specification, a java.sql.Connection object has the provision to create an SQLXML object that initially does not have any data. The data can be added with the setString() method or the setBinaryStream(), setCharacterStream(), and setResult() methods. An SQLXML object can be retrieved from a ResultSet or a CallableStatement by using the overloaded getSQLXML() method. The data in an SQLXML object can be retrieved by using the getString() method or the getBinaryStream(), getCharacterStream(), and getSource() methods. An SQLXML object can be stored in a database table column of type XML, which is similar to any other data type using the setSQLXML() method of the PreparedStatement interface.

SQL Server 2005 EXPRESS supports the XML data type whereas, SQL Server 2000 does not. IBM's DB2 UDB V 9 also supports the XML data type. To find out if a database supports the XML data type, obtain the database metadata from the Connection object:

```
DatabaseMetaData metadata= connection.getMetaData();
```

The data types are supported with the getTypeInfo() method, as shown below:

```
ResultSet rs=metadata.getTypeInfo();
```

Iterate over the data type result set and output the TYPE_NAME column, as shown below:

```
System.out.println("TYPE_NAME:"+rs.getString("TYPE_NAME")) ;
```

For SQL Server 2005 and IBM's DB2 UDB v9, the XML TYPE_NAME is output:

TYPE_NAME: XML

In the following subsections, the procedures to create an XML document, store it in a database that supports the XML data type, and retrieve it from the database will be discussed.

### Generating an XML Document

We will discuss the procedure to create and initialize an SQLXML object. Import the java.sql package, and the javax.xml.stream package:

```
import java.sql.*;
import javax.xml.stream.*;
```

The Java representation of an XML document in a database table is the SQLXML object. Create an SQLXML object from the Connection object with the createSQLXML() method, as shown below:

```
SQLXML sqlXML=connection.createSQLXML();
```

An SQLXML object can be initialized using one of the setString(), setBinaryStream(), setCharacterStream(), or setResult() methods. An SQLXML object can be initiated using the setResult() method and the StAXResult class. Create an XMLStreamWriter object from a StAXResult object, as shown below:

```
StAXResult staxResult = sqlXML.setResult(StAXResult.class);
XMLStreamWriter xmlStreamWriter = staxResult.getXMLStreamWriter();
```

The SQLXML object becomes non-writable after the setResult() method is invoked. Add the start of an XML document with the writeStartDocument(String,String) method, as shown below:

```
xmlStreamWriter.writeStartDocument("UTF-8","1.0");
```

The encoding and version of the XML document is specified in the writeStartDocument method. Add the start of an element with the writeStartElement (String localName) method, as shown below:

```
xmlStreamWriter.writeStartElement("catalog");
```

Add the element attributes by using the writeAttribute(String localName, String value) method. Add an element of text by using the writeCharacters(String text) method. Each start element would have a corresponding end element tag. Add an end element by using the writeEndElement() method. The writeEndElement() method does not specify the element name as the writeStartElement(String) method:

```
xmlStreamWriter.writeEndElement();
```

Add end of the document by using the writeEndDocument() method:

```
xmlStreamWriter.writeEndDocument();
```

A SQLXML object can also be initiated using the SAXResult class. Create a SAXResult object using the setResult() method of the SQLXML interface. Subsequently, obtain the ContentHandler result using the getHandler() method:

```
SAXResult saxResult = sqlXML.setResult(SAXResult.class);
ContentHandler contentHandler= saxResult.getHandler();
```

Specify the start of an XML document using the startDocument() method:

```
contentHandler.startDocument();
```

Specify the start of an element using the startElement(String uri,String localName,String qName,Attributes atts) method in which the parameter uri specifies the element namespace, parameter localName specifies the element local name, parameter qName specifies the element qualified name and parameter atts of type Attributes specifies the element attributes. An Attributes object can be created using the org.xml.sax.helpers.AttributesImpl class, which implements the Attributes interface. An attribute can be added to the AttributesImpl object using the addAttribute(String uri, String localName, String qName, String type, String value) method:

```
AttributesImpl attrs=new AttributesImpl();
attrs.addAttribute("","","journal","StringType","OracleMagazine");
contentHandler.startElement("","","catalog",attrs);
```
