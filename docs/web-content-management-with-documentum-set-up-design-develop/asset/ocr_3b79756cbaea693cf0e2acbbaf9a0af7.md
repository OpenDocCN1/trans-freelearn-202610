This code stores the DQL query string used to fetch the object ID (r_object_id attribute) and the file name (object_name attribute) of the current content file (i.e. whose object ID matches the current object ID: DMS_INPUT_OBJECT_ID).

Also, define a variable to store the results of the above DQL query by calling the pre-defined XDQL template and passing the DQL query string to it. For example:

```
<xsl:variable name = "query_results">
  <xsl:call-template name="XDQL">
    <xsl:with-param name="dql" select="string($dqlQuery)"/>
  </xsl:call-template>
</xsl:variable>
```

Once you have the query results saved in a variable, iterate through it, retrieve the required attribute (object_name in our case) and save it in a variable.

```
For example:
<xsl:variable name="ObjectName">
  <xsl:for-each select="xalan:nodeset($query_results)/xdql/object">
    <xsl:value-of select="object_name"/>
  </xsl:for-each>
</xsl:variable>
```

This is all that needs to be taken care of while using XDQL within XSL stylesheets in Web Publisher. In our example, displaying the content file's name (object_name attribute) is as simple as accessing the value of the objectName variable. For example:

```
<!-- Displaying the name of the file (object_name attribute) -->
<td><b><u><xsl:value-of select="$objectName"/></u></b></td>
```

Note that by using $(variableName), the system substitutes the value contained in the variable at run time.

# 15.6 Create_Dynamic_Content Job

Presentation files using XDQL queries can retrieve the Docbase content and its attributes whenever:

+   - A new content file is created from a template and saved from within Web Publisher editor
+   - An existing content file is edited and saved
+   - The reapply presentation feature is used for existing content

However, what happens to those published active content files that show up on the website but whose underlying data (attribute or content values) has been modified in the Docbase? Do the content authors have to edit/save and republish these content files all over again to ensure that XDQL query is fired again and modified data shows up on the running website? The answer is NO.