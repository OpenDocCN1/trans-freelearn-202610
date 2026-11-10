![](bbox=[0.175, 0.147, 0.825, 0.508])

Figure 26.17: Properties of the document object

# 26.2.4.6 Obtaining a Reference to the Document Object in Docbase

You can issue the id API command to obtain the object ID of an object that satisfies a particular qualification. Say for example we issue the id API command with a qualification statement "dm_document object whose name is TestDocumentCreated_via_API" as follows:

```
API>id,c,dm_document where object_name = 'TestDocumentCreated_via_API'
090015558000c629
```

The server returns the object ID 090015558000c629 for the document object TestDocumentCreated_via_API.

Note that you need not enter the complete search qualification starting from select and specifying the from clause. Instead you simply need to enter the clause starting after the from keyword.