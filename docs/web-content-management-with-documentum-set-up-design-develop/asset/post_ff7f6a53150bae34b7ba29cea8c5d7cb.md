
### 14.1.6 NEXTLINKURL Element

![](img/b18244e6621ecc39cef03ff6c0e55e7c_223_0.png)

Figure 14.20: Textline widget for NEXTLINKURL element

## 14.2 Final Template in Web Publisher Editor

Assuming that you have created the Rules file as per the specifications shown in the screenshots and checked-in the same in Docbase, let us create new News Article content by browsing the News_Articles category and choosing menu option File | New | Content.

After filling in the properties information for the new content, the Web Publisher editor opens up, displaying the News Article template as per the Rules defined in the Rules file we created. Figures 14.21 and 14.22 show the News Article template as seen in Web Publisher editor.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_224_0.png)

Figure 14.21: News Article Template

![](img/b18244e6621ecc39cef03ff6c0e55e7c_224_1.png)

Figure 14.22: News Article Template—continued

Refer to figure 14.22; note the repeating block widget added in the Next article links repeating block. You can duplicate the News article link name/URL fields, re-arrange multiple fields, and even delete newly added fields using the repeating block widget.

## 14.3 Miscellaneous Rules File Widgets

We have seen some Web Publisher Rules file widgets like textline, content, graphic, and repeating block. Here are some other widgets as they are seen in Web Publisher editor. A visual appeal leaves a lasting impact and will help you design your templates better.

### 14.3.1 Choice Widget

![](img/b18244e6621ecc39cef03ff6c0e55e7c_225_0.png)

Note that the delimiter in a choice widget is comma (,). This means that the string (Sarah, Timothy) would show up as two values, Sarah and Timothy, in the dropdown. The only workaround to such a scenario is to separate them with a semicolon or some other delimiter instead of a comma. Example: Sarah;Timothy.

### 14.3.2 Checkbox Widget

![](img/b18244e6621ecc39cef03ff6c0e55e7c_225_1.png)

### 14.3.3 Textselector Widget

![](img/b18244e6621ecc39cef03ff6c0e55e7c_225_2.png)

### 14.3.4 Xselector Widget

![](img/b18244e6621ecc39cef03ff6c0e55e7c_226_0.png)

Figure 14.26: Clicking on the Browse button in an Xselector widget

It may be worthwhile to quickly discuss some of the enhancements made to Web Publisher in Documentum release 5.3:

- Enhancements in Web Publisher editor with rich-text editing features and support for link management in content widget
- Browser drag-and-drop support allowing Internet Explorer users to drag items from their desktop to system folders and dragging items within Web Publisher
- Search improvements via a new improved search algorithm
- Support for specifying columns that should appear in folder lists, Inbox, search result pages, etc.

This chapter should have given you a good idea about what Rules file are and how they can be created and associated with template files. You should go through the Documentum Web Publisher administration manuals to learn more about Rules files, different widget types, and their numerous specific attributes in more detail. You can now continue your journey by moving over to the next chapter in order to create a Presentation file for our News Article template.

## 14.4 Summary

Rules files define the validation and behavior of the various XML elements in Web Publisher Template files. We started our tour by going through a detailed example of creating a Rules file in Web Publisher for our News Article template. We saw how to set preferences for invoking Web Publisher Rules Editor for creating Rules files.

We briefly discussed the following available Rules file widgets:

- Textline
- Content
- Choice
- Textselector
- Graphic
- Checkbox
- Xselector
- Repeating Blocks

Lastly, we saw how some of the Rules file widgets appear when viewed in Web Publisher editor.

## 15 Creating Presentation Files

We have created a Template file and associated it with a Rules file in the previous chapters. What now remains is the creation of a Presentation file (XSL) to format the data in the content file into a suitable rendition such as HTML, WML, etc. The website pages are nothing but the web renditions of content XML files via Web Publisher presentation files and so the design and creation of Presentation files is crucial in a web content management project.

Additionally, Presentation files separate the structure of content from its layout. While the templates and Rules files simply capture the structure of the content to be entered by content authors, the Presentation files focus on how the entered content is formatted for generating web renditions.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_228_0.png)

Figure 15.1: Creation of renditions via stylesheets

A rendition is created by Documentum whenever a content file is saved and checked into the Docbase. Behind the scenes, Web Publisher applies the Presentation file (XSL stylesheet) to the content XML file and generates the prescribed rendition. Presentation files in Web Publisher are of two kinds:

- **Editor Presentation files (XSL stylesheets)**: Used for formatting Web Publisher editor XML content and stored in the Docbase at the following location: Site Manager | Presentations | Editor.
- **External Presentation files (HTML wrappers)**: Used for external applications and stored in the Docbase at the following location: Site Manager | Presentations | External. External presentation files provide standard frames and wrappers to the content embedded within the <body></body> elements of HTML content files.

### 15.1 Creating a Presentation File

Creating Presentation files in Documentum is not a big deal and the following steps explain how they can be easily built and associated with template files.

1. Log in to Web Publisher as an administrator or a web developer user and go to Site Manager | Presentations | Editor. As we saw with Rules files, it is a good approach to create a new folder within the Editor folder to identify and separate specific Presentation files for your custom application.
2. Choose the menu option File | New | Folder. The new folder creation screen comes up as shown in the figure 15.2.

![](img/b18244e6621ecc39cef03ff6c0e55e7c_229_0.png)

Figure 15.2: New folder for storing Presentation files

3. Provide some valid name for your custom Presentation file folder (say Custom_Presentations) and click on the Finish button. You could also attach some ACL (Permission Set) to this folder as in the case of Rules files.

4. Create a valid/well-formed industry-standard XSL stylesheet in your favorite editor such as XML Spy®, Notepad, etc. and provide some meaningful name to it (say cust_news_art_pres.xsl for the News Article template). Save the newly created Presentation file within a convenient folder location on your machine's hard drive.

Figures 15.3 and 15.4 show a sample presentation file for the News Article template. Note that you could improve it (and there definitely is enough scope for improvement!) as per your specific business requirements.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE xsl:stylesheet [View Source for full doctype...]>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:fo="http://www.w3.org/1999/XSL/Format" xmlns:java="http://xml.apache.org/xslt/java" xmlns:xalan="http://xml.apache.org/xalan" exclude-result-prefixes="xalan java fo">
  <xsl:output method="html" encoding="ISO-8859-1" indent="yes" />
  <xsl:strip-space elements="*" />
  <xsl:template match="*|@*|comment()|processing-instruction()|text()">
    <xsl:copy>
      <xsl:apply-templates select="*|@*|comment()|processing-instruction()|text()" />
    </xsl:copy>
  </xsl:template>
  <!-- Matching content XML file root element -->
  <xsl:template match="NEWSART">
    <table border="0">
      <tr>
        <th>News web site:</th>
      </tr>
      <tr>
        <td>
          <br />
        </td>
      </tr>
      <tr>
        <!-- Capturing location path of inserted banner image -->
        <xsl:variable name="imagePath">
          <xsl:value-of select="BANNERIMG/img/@src" />
        </xsl:variable>
        <!-- Capturing alternative text of inserted banner image -->
        <xsl:variable name="imageAltText">
          <xsl:value-of select="IMGALTEXT" />
        </xsl:variable>
        <td>
          <img src="{$imagePath}" alt="{$imageAltText}" />
        </td>
      </tr>
      <tr>
        <td>
          <br />
        </td>
      </tr>
      <tr>
        <!-- Displaying news article date -->
```

Figure 15.3: Sample News Article Presentation file
