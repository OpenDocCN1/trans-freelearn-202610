The create ... object DQL query creates an object in the Docbase of the specified object type (dm_document in our example). We have set the object_name attribute of the document object as TestDocumentCreated_via_DQL in the same create ... object query.

Note in particular the setfile keyword used in the above query. This associates a content file with the document object. The prerequisite is that you need to create a valid content file (testing_dql.xml in our example) and place it at a convenient location on the Content Server host before issuing the DQL. The with content_format clause sets a valid format for the content file that is associated with the document object.

Note that the format specified should be understood and recognized by the Content Server.

If the specified format (say xyz) is invalid and not recognized by Content Server, the following error message is seen on executing the query:

Error occurred during query execution :[DM_QUERY_F_UP_SAVE]fatal: "UPDATE: An error has occurred during a save operation." [DM_SYSOBJECT_E_INVALID_FORMAT]error: "The specified format (xyz) is invalid."

The create ... object statement returns the object ID of the newly created object once the DQL runs successfully. In our case running the above DQL query returned the object ID of the document object as 090015558000c972 as shown in figure 26.5.

![](bbox=[0.17, 0.43, 0.83, 0.65])

Figure 26.5: Running DQL query to create an object

# 26.1.3.2 Updating Attributes of a Document Object

An existing object's attributes can be modified by issuing the update ... object DQL query. In this example we will update the newly created document object by setting its title and keywords attributes as follows:

```
DQL> update dm_document object set title = 'File created via DQL' ,
    set keywords = 'first' where object_name = 'TestDocumentCreated_via_DQL'
```