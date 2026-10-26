# Displaying an XML feed

The XML/XSL module is used to display a formatted XML data feed on a DNN page. There are many different kinds of publically available XML feeds and in this recipe will we see how to configure and format one.

# Getting ready

To use the XML/XSL module you need a URL to a local or remote XML feed. In this recipe we will use the feed of a popular technology news site call Digg. We will also create a stylesheet to describe how the XML feed should be displayed. This is called the XSL Transformation (XSLT) file.

# How to do it...

+   1. Before we can add an XML feed to our page, we must create a text file that describes how the information should be displayed.
+   2. Launch your normal Development Tool.
+   3. Select New File... from the File menu.

![](bbox=[0.115, 0.483, 0.884, 0.721])

+   4. Select General as the Category and choose XSLT File as the template.
+   5. Click on Open.
+   6. A template XSL file will appear in the editor. Modify the file by adding the following code to describe how to display the XML elements:

```
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
```