![](bbox=[0.332, 0.147, 0.665, 0.356])

Figure 26.3: IDQL button shown in Documentum Server Manager

DQL queries can also be executed from within Documentum Administrator by clicking on the DQL button shown in the top right-hand side of the toolbar. This opens the Dql--Enter Query screen as shown in figure 26.4.

![](bbox=[0.189, 0.438, 0.81, 0.582])

Figure 26.4: DQL query editor in Documentum Administrator

# 26.1.3 DQL Examples

Let us take a simple example of creating a document object, setting its attribute values, linking it to a cabinet in the Docbase, and finally saving it. We will then retrieve this document object from the Docbase and delete it. You will come across a number of DQL queries while working on this simple example.

# 26.1.3.1 Creating a Document Object

Log in to Documentum Administrator as an administrator and open the DQL editor screen.

Refer to figure 26.5; enter the following query in the DQL text box and click on the Execute button:

```
DQL> create dm_document object set object_name = 'TestDocumentCreated_via_DQL',
    setfile 'C:\Test\testing_dql.xml' with content_format = 'xml'
```