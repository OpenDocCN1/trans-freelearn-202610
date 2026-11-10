The examples that we have seen till now have one thing in common: The value entered by the content creator in the template field is populated automatically in the object property via the XML Application. What if we need to do it the other way round? A typical business requirement could be that the content creator enters a value in a particular property field (say subject) and the same should be automatically populated in a particular field in Template file.

Documentum provides this feature via the two_way_dctm attr element in XML configuration files, as we will now see.

# 18.6 Using Two-Way Attribute Extraction

Let us create a Template file that allows content creators to enter the subject property in the document's property screen. We will create a custom XML Application that will populate this value in the SUBJECT element of the Template file.

+   1. Create and import a template in Web Publisher (say Two_way_Template.xml) and attach it to dm_document object type with Default Lifecycle. Following is the Template file Two_way_Template.xml:
+   2. Create a Rules file (say Two_Way_TemplateRule.xml) for the above template and associate the Template file with this Rules file. Following is the Rules file Two_Way_TemplateRule.xml:
+   3. Create an XML configuration file TwoWayXMLApp.xml and provide the name of the XML configuration file (without the .xml) in the Category field of the template TwoWayXMLApp.xml. This sets the a_category attribute of the template. The TwoWayXMLApp.xml file is shown below:

```
<?xml version="1.0" encoding="UTF-8"?>
<INFORMATION>
<SUBJECT/>
</INFORMATION>
```

```
<?xml version="1.0" encoding="UTF-8"?>
<rules>
    <tagcontent tag_name="SUBJECT">
        <textline charlength="150" instruction="Enter a subject here" label="Subject:">
        </textline>
    </tagcontent>
</rules>
```