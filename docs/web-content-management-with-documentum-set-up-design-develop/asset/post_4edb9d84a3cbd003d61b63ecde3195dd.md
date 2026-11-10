
## 26.2.4 API Method Examples

As we did for DQL, let's take the same example of creating a document object, setting its attribute values, linking it to a cabinet in the Docbase, and finally saving it. We will then retrieve this document object from the Docbase and delete it. You will come across a number of API methods while working on this simple example.

### 26.2.4.1 Creating a Document Object

Log in to Documentum Administrator as an administrator and open the Api Tester screen.

Refer to figure 26.14; run the create API command by entering the string create,c,dm_document in the Command box as shown in the Api Tester screen and clicking on the Execute button.

Make sure that you do not enter white spaces anywhere in the command string.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_442_0.png)

Figure 26.14: Running the API command create using Documentum Administrator

The create API method creates an object of the specified object type (dm_document in our example) and returns the object ID of the newly created object. As shown in the figure 26.14, the object ID of the newly created document object is 090015558000c629.

Note that the newly created object is not committed to the Docbase until you issue an explicit save API command as we will see later in this example.

### 26.2.4.2 Setting the Attributes of the Object

The set API method can be used to set particular values for single as well as repeating attributes of objects. Set the object_name attribute of the newly created document object as shown in figure 26.15.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_443_0.png)

Figure 26.15: Setting an attribute value via API command

Note that the method command and arguments are entered in the Command text box while the value of the argument is specified in the Data box as shown in figure 26.15. You must have noticed the peculiar letter l used in the command string: set,c,l,object_name.

This is an alias for the last created, retrieved, checked in/out, or fetched object ID. If you do not want to use the alias l, you can instead run the command as:

```
set,c,090015558000c629,object_name
```

Here 090015558000c629 is the object ID of the newly created document object in our example.

The other attributes, such as title, for the document object can be set accordingly as follows:

```
API>set,c,l,title
Data: File created via API commands
...
OK
```

You can set the repeating attributes (e.g. keywords) as follows:

```
API>set,c,l,keywords[0]
Data: document
...
OK
API>set,c,l,keywords[1]
Data: api
...
OK
```

This sets document and api as the two keywords for the document object at the first and second index positions respectively.

### 26.2.4.3 Associating a Content File with the Document Object

The setfile API method can be used to associate a content file with an object. The prerequisite is that the content file should be present on the machine (client) from where the setfile command is executed. Note that since in our case we are executing the API commands from the Documentum Administrator host as our client, the content file to be set should be locally present on the machine where Documentum Administrator has been installed.

Create a content XML file (say testing_api.xml) and place it at a convenient location on the Documentum Administrator host, for example: C:\Test.

Issue the following setfile command:

```
API>setfile,c,l,C:\Test\testing_api.xml,xml
...
OK
```

Notice the additional argument (xml) specified in the setfile command following the complete path to the content file. This is the name of a valid format for the content file and should be recognized by the Content Server.

Most people place the content file locally on the machine through which they access Documentum Administrator URL via a browser, rather than placing it on the Documentum Administrator machine. Under such scenarios, the following error is thrown by Content Server when you issue the setfile API command:

[DM_SYSOBJECT_E_CANT_ACCESS_FILE]error: "Cannot access file C:\Test\testing_api.xml due to Operating System error: The system cannot find the file specified."; ERRORCODE: 100; NEXT: null

### 26.2.4.4 Associating a Document Object with a Cabinet

Once you have created a document object and set its attributes and content file, the next step is to associate it with either a cabinet or a folder residing within a cabinet. We will use the link API command in our example to associate the newly created document object with a custom cabinet.

As a prerequisite, create a custom cabinet Testing_API_DQL in Web Publisher and then issue the following link command:

```
API>link,c,l,'/Testing_API_DQL'
...
OK
```

You can either link an object with a cabinet or folder using the folder path as we saw in our example, or by referring to the object ID of the cabinet or the folder. For example, if the object ID of Testing_API_DQL cabinet is 0c0015558000c62a, you can link the document object to this cabinet as follows: API>link,c,l,0c0015558000c62a

### 26.2.4.5 Saving the Document Object in the Docbase

Until and unless you explicitly issue the save API command, the newly created document object is not saved in the Docbase. Following is an example displaying how to use the save API:

```
API>save,c,l
...
OK
```

Once you have saved the document object, you can have a look at it by logging in to Web Publisher and browsing to the Testing_API_DQL cabinet. Figure 26.16 shows how the document object appears in Web Publisher.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_445_0.png)

Figure 26.16: Newly created document object as seen in Web Publisher

If you select the checkbox against the document object and choose the menu option File | View, you will be able to view the associated content file testing_api.xml.

In order to look at the properties of the document object, you can either click on the Info icon ⓘ or select the checkbox against the document object and choose the menu option View | Properties | Info. Figure 26.17 shows the various properties for the document object that we set using API commands in this example. Note in particular the Name, Title, and Keywords fields shown in figure 26.17 that we set using the API command set.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_446_0.png)

Figure 26.17: Properties of the document object

### 26.2.4.6 Obtaining a Reference to the Document Object in Docbase

You can issue the id API command to obtain the object ID of an object that satisfies a particular qualification. Say for example we issue the id API command with a qualification statement "dm_document object whose name is TestDocumentCreated_via_API" as follows:

```
API>id,c,dm_document where object_name = 'TestDocumentCreated_via_API'
090015558000c629
```

The server returns the object ID 090015558000c629 for the document object TestDocumentCreated_via_API.

Note that you need not enter the complete search qualification starting from select and specifying the from clause. Instead you simply need to enter the clause starting after the from keyword.

### 26.2.4.7 Setting Specific Attribute Information

Once you have obtained the object ID of the document object, getting the values of its various attributes is very simple. All you need to do is issue a get API command specifying the attribute whose value needs to be found out.

For example, in order to find out the value of the second keywords attribute for the retrieved document object, run the get API command as follows:

```
API>get,c,l,keywords[1]
api
```

The server returns the value api as the value contained at the second position in the keywords attribute. We have used an index position [1] in this case because keywords is a repeating attribute that stores values starting at index position 0 onwards. In the case of a single-valued attribute, you need not use any square brackets. For example, to find out the value of the title attribute, run the command as follows:

```
API>get,c,l,title
```

### 26.2.4.8 Viewing all Attributes and Values for an Object

In order to look at all the attributes and their values for a particular object in the Docbase, use the dump API command. Run the dump API command as follows:

```
API>dump,c,090015558000c629
...
```

We have specified the object ID of the document object (090015558000c629) as an argument in the dump API command. Figure 26.18 shows a snapshot listing a dump of the various attributes values of the document object we had created.
