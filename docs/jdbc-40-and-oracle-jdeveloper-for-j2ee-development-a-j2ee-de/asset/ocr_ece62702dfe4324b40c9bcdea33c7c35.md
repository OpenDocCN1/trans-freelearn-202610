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
AttributesImpl.AttributesImpl() attrs=new AttributesImpl();
attrs.addAttribute("","","journal","StringType","OracleMagazine");
contentHandler.startElement("","","catalog",attrs);
```