
## XML SQL Utility

We need to add some libraries and JAR files to the project. As we are using the JDBC 4.0 driver, we need to add the JDBC 4.0 driver JAR file to the project libraries. Download the Oracle Database 11g JDBC driver's JAR file, ojdbc6.jar from http://www.oracle.com/technology/software/tech/java/sqlj_jdbc/htdocs/jdbc_111060.html. Select Tools | Project Properties, and in the Project Properties window, select Libraries. Select the Add Library button to add Oracle XML SQL Utility and Oracle XML Parser v2 libraries. Select the Add Jar/Directory button to add <JDeveloper>/rdbms/jlib/xdb.jar and ojdbc6.jar, <JDeveloper> being the JDeveloper installation directory. The JAR files and libraries required for the XML SQL Utility are shown below:

![](img/553e44bc30e70d0d8e58278a7ece8da8_200_0.png)

Oracle database 11g JDBC 4.0 drivers require JDK 6.0. We need to set the J2SE Version to JDK 6.0, then download and install JDK 6.0 from http://java.sun.com/javase/downloads/index.jsp. Select Tools | Project Properties to set the J2SE version to JDK 6.0 and in the Project Properties window, select Libraries. The J2SE Version field specifies the J2SE version used for Java applications in JDeveloper. Click on the Change button in J2SE Version, and click on New in the Edit J2SE Definition window to define a new J2SE version definition. Select the J2SE Executable for JDK 6.0 in the Create J2SE window and click on OK. A J2SE definition for JDK 6.0 gets added to Edit J2SE Definition window. Select the J2SE 6.0 definition and click on OK in the Edit J2SE Definition window.

![](img/553e44bc30e70d0d8e58278a7ece8da8_201_0.png)

The JDK 6.0 J2SE definition gets added to the J2SE Version field in the Project Properties window. Now, click on OK.

Also install Oracle database 10g including sample schemas. Oracle database 11g JDBC drivers can be used with Oracle database 10g and all the JDBC 4.0 features can be advantageous if using an application server that supports JDBC 4.0. In the database create a database instance, and a database table, JOURNAL. The example XML document will be mapped to the database table. The SQL script used to create a database table in a sample schema, OE is illustrated below:

```
CREATE TABLE OE.JOURNAL (CATALOGID VARCHAR(255) PRIMARY KEY, JOURNAL_TITLE VARCHAR(255), PUBLISHER VARCHAR(255), EDITION VARCHAR(255), ARTICLE_SECTION VARCHAR(255), TITLE VARCHAR(255), AUTHOR VARCHAR(255));
```

## XML Document to SQL Database Mapping

If the XML document had been stored with XSU, only the element tags in the XML document would have got stored. The attributes in the XML document would not have got stored. Attributes in an XML document can be stored by mapping them to elements. The attributes can be converted to elements by applying an XSLT stylesheet to the XML document. This section discusses the procedure to store an XML document with attributes in a database table. The XML document is stored in a database table using the OracleXMLSave class, which is imported in the XMLToDatabase.java application.

Create a connection with the database. If we were using a JDBC 3.0 driver, we would have registered the Oracle JDBC driver with DriverManager using the registerDriver(Driver) method. JDBC 4.0 adds support for automatic driver registration using the Java SE Service Provider mechanism. The Oracle database 11g JDBC 4.0 drivers JAR file, ojdbc6.jar, includes a META-INF/services/java.sql.Driver file. For the Oracle JDBC driver, this file includes:

```
oracle.jdbc.OracleDriver
```

All the driver classes specified in the java.sql.Driver file becomes available to the DriverManager when a connection is requested from the DriverManager using the getConnection() method. Therefore, we will not be registering the Oracle JDBC driver with the DriverManager. Obtain a connection from DriverManager using the getConnection(String url, String user, String password) method:

```
Connection conn =
DriverManager.getConnection("jdbc:oracle:thin:@<host>:<port>:<SID>",
"<user>","<password>");
```

![](img/553e44bc30e70d0d8e58278a7ece8da8_202_0.png)

<host> is the database host.
<port> is the database port.
<SID> is the database SID.
<user> is the username to log in to the database.
<password> is the password to log in to the database.
Values for <host>, <port>, and <SID> can be obtained from the <Oracle 10g>/NETWORK/ADMIN/tnsnames.ora file.

An SQLException can be generated while obtaining a connection to the Oracle database. The SQLException generated may contain chained exceptions and chained causes. JDBC 4.0 has added support for the Java SE chained exception facility, which is also called the cause facility. Prior to JDBC 4.0, the getNextException() and getCause() methods had to be called recursively to retrieve the chained exceptions and causes. JDBC 4.0 has enhanced support for these chained exceptions using the for loop introduced in J2SE 6.0. SQLException interface implements the Iterable interface. The chained exceptions may be output using the enhanced for-each loop as shown below:

```
catch(SQLException exception)
{
    for(Throwable e : exception )
    {
        System.out.println("Error encountered: " + e);
    }
}
```

We need to create an OracleXMLSave class object to save an XML document to a database table, JOURNAL.

```
OracleXMLSave oracleXMLSave =new OracleXMLSave(conn, "JOURNAL");
```

JOURNAL is the database table created in the Setting the Environment section. Convert the attributes to element tags in the given example XML document. The attributes are converted to element tags by applying an XSLT to the XML document. Set an XSLT on the OracleXMLSave object using the setXSLT(Reader, String) method. The String parameter specifies the URL to be used to include import and external entities.

```
Reader xsltReader=new FileReader(new File("input.xsl"));
oracleXMLSave.setXSLT(xsltReader, null);
```

The XSLT, input.xslt, is used to convert the attributes to elements in the given XML document, as an example:

```
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
<xsl:output method="xml/text" omit-xml-declaration="no"/>
<xsl:template match="/">
<xsl:element name="catalog">
<xsl:apply-templates select="catalog/journal"/>
</xsl:element>
</xsl:template>
<xsl:template match="journal">
<xsl:element name="journal">
<xsl:element name="journal_title">
<xsl:value-of select="@title"/>
</xsl:element>
<xsl:element name="catalogId">
<xsl:value-of select="catalogId"/>
</xsl:element>
<xsl:element name="publisher">
<xsl:value-of select="@publisher"/>
</xsl:element>
<xsl:element name="edition">
<xsl:value-of select="@edition"/>
</xsl:element>
<xsl:apply-templates select="article"/>
</xsl:element>
</xsl:template>
<xsl:template match="article">
<xsl:element name="article_section">
<xsl:value-of select="@section"/>
</xsl:element>
<xsl:copy-of select="title"/>
<xsl:copy-of select="author"/>
</xsl:template>
</xsl:stylesheet>
```

The input.xslt transformation converts attributes, title and section in the XML document, to elements, journal_title and article_title. Set the tag for the row enclosing element. Setting the row tag to null implies that a row tag is not defined and the top-level elements of the document correspond to the database table rows. The row tag is set to journal. Each journal tag in the example, XML document corresponds to a database table row. Each row in the table represents a journal entry; the journal entry is the row enclosing element.

```
oracleXMLSave.setRowTag("journal");
```

Set the columns containing the primary key in the database table using the setKeyColumnList(String[] keyColNames) method. Set CATALOGID as a primary key column:

```
String [] keyColNames = new String[1];
keyColNames[0] = "CATALOGID";
oracleXMLSave.setKeyColumnList(keyColNames);
```

XSU maps the XML document elements to database table columns, based on element tag names. To match element tags that are case-insensitive set ignore case to true.

```
oracleXMLSave.setIgnoreCase(true);
```
