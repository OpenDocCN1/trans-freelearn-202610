The SQLXML object becomes non-readable after calling the getBinaryStream() method. The next event is obtained by using the next() method, as shown below:

```
while(xmlStreamReader.hasNext())
{
    int parseEvent=xmlStreamReader.next();
}
```

The next() method returns an int value that corresponds to an XMLStreamConstants constant, which represents an event type. Some of the return values of the next() method are listed in following table:

| Event Type | Description |
|---|---|
| ATTRIBUTE | Specifies an attribute. |
| CDATA | Specifies a Cdata. |
| CHARACTERS | Text. |
| COMMENT | An XML document comment. |
| NOTATION_DECLARATION | Specifies a notation declaration. |
| START_DOCUMENT | Specifies the start of a document |
| START_ELEMENT | Specifies the start of an element. |
| END_ELEMENT | Specifies the end of an element. |
| ENTITY_DECLARATION | Specifies an entity declaration. |
| ENTITY_REFERENCE | Specifies an entity reference. |
| NAMESPACE | Specifies a namespace declaration. |
| SPACE | Specifies an ignorable white space. |

If the return value is ELEMENT, then the local name, prefix, and namespace can be obtained by using the getLocalName(), getPrefix(), and getNamespaceURI() methods, as shown below:

```
System.out.println("Element Local Name: "+xmlStreamReader.getLocalName());
System.out.println("Element Prefix: "+xmlStreamReader.getPrefix());
System.out.println("Element Namespace:"+xmlStreamReader.getNamespaceURI());
```

The attribute count in an element is obtained by using the getAttributeCount() method. Iterate over the attributes and obtain the attribute local name by using the getAttributeLocalName() method, the attribute value with the getAttributeValue() method, the attribute prefix with the getAttributePrefix() method, and the attribute namespace with the getAttributeNamespace() method: