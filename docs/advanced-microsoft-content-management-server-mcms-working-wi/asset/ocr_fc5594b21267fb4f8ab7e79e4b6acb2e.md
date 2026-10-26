document properties, but only InfoPath and WordML files are XML and can be transformed with a content stylesheet. You specify the stylesheet you want to use in the transformation in the Placeholder Properties dialog we just encountered when adding a document. The Connector ships with default stylesheets that we've already used. These stylesheets are found at C:\Program Files\Microsoft Content Management Server\Server\IIS_CMS\WssIntegration\Dialogs\WssDocumentFinder\TemplateCatalog\. The default stylesheets included with the Connector are listed in the table below:

| Stylesheet Name | Description |
|---|---|
| AttachmentIcon.xsl | Displays the document as an icon attachment. Available only if the file attached is a document. |
| AttachmentIconProps.xsl | Displays the document as an icon attachment with its properties. Available only if the file attached is a document. |
| ImageTagOnly.xsl | Displays the image inline. Available only if the file attached is an image. |
| ImageAndProps.xsl | Displays the image inline with its properties. Available only if the file attached is an image. |

The table above is based on documentation found in the Microsoft Content Management Server 2002 Connector for SharePoint Technologies help file, included with the Connector.

The SharePoint Document Placeholder looks at a single XML file to determine what stylesheets are available for the different types of files that can be attached to the placeholder. This file, catalog.xml, can be found in the same directory as the stylesheets outlined above. The catalog.xml file contains the following information on each of the available styles:

+   - Types of files the stylesheet can be applied to, such as jpg, gif, contentxml, or all (noted with an *)
+   - Descriptive name shown in the dropdown of the Placeholder Properties dialog
+   - Description of the stylesheet
+   - URL specifying the location of the XSL stylesheet used in the transformation
+   - A sample rendering URL, which is an XML file used to mimic the XML feed sent to the placeholder when a document is attached, giving the user a preview of the final rendering

In addition, you can also use a specific library that the stylesheet can be applied to using the SpecificWebUrl and SpecificLibrary attributes in the type node of the catalog.xml file. Use the SpecificWebUrl attribute to indicate which WSS or SPS site and the SpecificLibrary attribute to indicate the name of the WSS or SPS library to which the stylesheet can be applied.

You can create your own catalog.xml file for a specific SharePoint Document Placeholder implementation in order to restrict or provide certain rendering options for that library. Simply create a new XML file that conforms to the catalog.xsd schema definition file (found in the same directory as the catalog.xml and stylesheets) and specify this file in the HTML of your MCMS template that implements the SharePoint Document Placeholder using the