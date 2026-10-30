Create an InputStream from the XML document, catalog.xml.

```
InputStream input=new FileInputStream(new File("catalog.xml"));
```

Map the example XML document to a database table using the insertXML(InputStream) method. The insertXML() method is overloaded, and the input can be specified as a Document object, an InputStream object, a Reader object, a String object, or a URL object:

```
oracleXMLSave.insertXML(input);
```

The XSLT, input.xslt, gets applied to the XML document. The XML document, with the attributes converted to elements, gets stored in the database. Close the OracleXMLSave object using the close() method:

```
oracleXMLSave.close();
```

The XMLToDatabase application is listed completely, later in the chapter. Selected operations can be run by commenting out the methods that are not required. For example, to map an XML document to a database table, comment out invocations to all the methods except the xmlToSQL() method. To run an XMLToDatabase.java application, in JDeveloper right-click on the application node and select Run.

![](bbox=[0.178, 0.471, 0.822, 0.804])