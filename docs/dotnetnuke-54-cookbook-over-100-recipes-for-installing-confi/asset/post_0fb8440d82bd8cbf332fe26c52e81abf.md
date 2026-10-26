
## Advanced Tricks with Existing Modules

```html
marginTop: '0',
marginLeft: '0',
top: '0',
left: '0',
width: '120px',
height: '60px',
padding: '5px'
}, 400);
});
});
</script>
<div class="imagebar">
<ul class="image">
<li><img alt="" src="/dotnetnuke/Portals/0/aspnet.gif" /></li>
<li><img alt="" src="/dotnetnuke/Portals/0/redgate.gif" /></li>
<li><img alt="" src="/dotnetnuke/Portals/0/WH4L.gif" /></li>
<li><img alt="" src="/dotnetnuke/Portals/0/exacttarget.gif" /></li>
<li><img alt="" src="/dotnetnuke/Portals/0/maximumasp.gif" /></li>
<li><img alt="" src="/dotnetnuke/Portals/0/telerik.gif" /></li>
</ul>
</div>
```

11. Click on Save to see the results. The jQuery code defines a function that displays a larger thumbnail of the image when the mouse hovers over the image, then removes it when the mouse leaves.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_313_0.png)

### How it works...

jQuery is a set of powerful JavaScript libraries with many features beyond the scope of this book. In addition to the simple example in this recipe, almost all jQuery functions can be added in the HTML module in the same way, including jQuery galleries, menu systems, and so on. To learn more about jQuery and how to write your own jQuery programs, visit the jQuery site at http://www.jquery.com.

### Using the HTML module with replacement tokens

The HTML module allows the substitution of dynamic content to replace special text tokens. With tokens you can include information from the DNN database in your HTML content.

Here are some of the tokens that we will use in this recipe:

| Token | Description |
|---|---|
| `[Portal:Email]` | E-mail of the primary administrator. |
| `[Portal:FooterText]` | The site copyright text. |
| `[Portal:HomeDirectory]` | The home directory of the portal. |
| `[Portal:PortalName]` | The name of the portal. |
| `[User:DisplayName]` | The display name of the current user. |
| `[User:Email]` | The e-mail of the current user. |
| `[Tab:Description]` | Description of the current page. |
| `[Tab:TabName]` | Name of the current page. |
| `[DateTime:Now]` | Current date and time. |

> To see more examples of Token replacements, there's a good list on the Data Springs website: `http://www.datasprings.com/resources/ArticlesInformation/DNNTextHTMLTokenReplacementOptions.aspx`.

### Getting ready

To follow along with this recipe you should be familiar with the basic content management tasks from *Chapter 2, Managing Users and Site Setup* (such as *Adding a module to a page*).

### How to do it...

1. Log in as Portal Administrator (`sysadmin` in our examples).
2. Add the HTML module to a new or existing page.
3. From the **HTML Module** menu, select **Settings** (or click on the Settings icon).
4. Scroll down to the **HTML Module Settings**:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_315_0.png)

5. Make sure **Replace Tokens** is checked.
6. Click on **Update** to save your changes.
7. Select **Edit Content** from the module menu (or click on the **Edit Content** icon).
8. When the HTML editor is displayed, click on the **HTML** button at the bottom. This will let us edit the HTML directly and avoid any automatic formatting.
9. Type the following HTML into the editor:

```html
<p>Site Status Report ([DateTime:Now])</p>
<hr />
<p><strong>Current User:</strong></p>
<p>Display Name: [User:DisplayName]</p>
<p>Email: [User:Email]</p>
<p><strong>Current Page:</strong></p>
<p>Name: [Tab:TabName]</p>
<p>Description: [Tab:Description]</p>
<p><strong>Portal Information:</strong></p>
<p>Name: [Portal:PortalName]</p>
<p>Contact: [Portal:Email]</p>
```

10. Click on **Save** to save your changes.
11. When the page is viewed, the tokens will be substituted with the current values from the portal settings:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_316_0.png)

### How it works...

Text token substitution gives you the ability to provide dynamic content from the normally static HTML module. Tokens are simple to use and give you direct access to values stored in the DNN database. That said, there are some limits on the use of tokens. Token replacement, which is done on-the-fly, conflicts with page caching which wants to preserve copies of the page for quick retrieval. This can kill performance if overused.

### Using the HTML module with Flash

As the HTML module can display the same kind of code as a normal HTML file, we can embed Flash animation in the module just as we would in a normal web page. In this recipe we will see how to add a .swf file to the HTML module using the editor.

### Getting ready

To follow along with this recipe you should be familiar with the basic content management tasks from *Chapter 2, Managing Users and Site Setup* (such as *Adding a module to a page*). You also need a sample Flash .swf file. For this example, we will use a sample Flash from the Adobe site at http://www.adobe.com.

### How to do it...

1. First we need a SWF file to use as an example. Open a browser to the following URL: http://www.adobe.com/jp/events/cs3_web_edition_tour/swfs/perform.swf and download the SWF file to a temporary folder.
2. Next, log in as Portal Administrator (`sysadmin` in our examples).
3. Add the HTML module to a new or existing page.
4. Select Edit Content from the module menu (or click on the Edit Content icon).
5. Place your cursor in the editor and click on the Flash Manager icon on the editor toolbar.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_317_0.png)

6. When the Flash Manager dialog is displayed, click on the Select button and navigate to the temporary folder where the sample SWF file was saved.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_317_1.png)

7. Click on Upload to load the SWF file.

8. Back on the Flash Manager dialog, set the width to 600 and the height to 400.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_318_0.png)

9. Leave the other options as default and click on Insert.
10. Click on Save to save your changes.
11. The Flash animation will now display on your page.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_318_1.png)

### There's more...

In this recipe we saw how to display a SWF file in the HTML module. But there are other Flash formats that you can use. However, these depend on the user's browser having the proper plugins, so as administrator it is your decision whether to use other formats on your site.

### Displaying an XML feed

The XML/XSL module is used to display a formatted XML data feed on a DNN page. There are many different kinds of publicly available XML feeds and in this recipe we will see how to configure and format one.

### Getting ready

To use the XML/XSL module you need a URL to a local or remote XML feed. In this recipe we will use the feed of a popular technology news site called Digg. We will also create a stylesheet to describe how the XML feed should be displayed. This is called the XSL Transformation (XSLT) file.

### How to do it...

1. Before we can add an XML feed to our page, we must create a text file that describes how the information should be displayed.
2. Launch your normal Development Tool.
3. Select New File... from the File menu.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_319_0.png)

4. Select General as the Category and choose XSLT File as the template.
5. Click on Open.
6. A template XSL file will appear in the editor. Modify the file by adding the following code to describe how to display the XML elements:

```xml
<?xml version="1.0" encoding="utf-8"?>
<xsl:stylesheet version="1.0"
    xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns:msxsl="urn:schemas-microsoft-com:xslt"
    exclude-result-prefixes="msxsl"
>

<xsl:param name="TITLE"/>

<xsl:template match="rss">

    <xsl:for-each select="channel/item">
        <br>
            <strong>
                <xsl:value-of select="title"/>
            </strong>
        <br></br>

        <!-- only display markup for description if it's
                present -->
        <xsl:value-of select="description"
                disable-output-escaping="yes"/>

        <a href="{link}" target="_main">
            [Click Here for More...]
        </a>

        </br>
        <br></br>
    </xsl:for-each>
</xsl:template>

<xsl:template match="description">
    <br>
        <xsl:value-of select="."/>
    </br>
</xsl:template>

</xsl:stylesheet>
```

7. Select **Save XSLTFile1.xslt As...** from the **File** menu.
8. Browse to a temporary folder and save the file as `news_feed.xsl`.
9. Now that we have an XSL file, log in as Portal Administrator (`sysadmin` in our examples).
10. Add the XML/XSL module to a new or existing page (if you don't see the XML/XSL module in the module drop-down list, see the recipe *Installing standard DNN modules* in *Chapter 1, Installation and Setup*).
