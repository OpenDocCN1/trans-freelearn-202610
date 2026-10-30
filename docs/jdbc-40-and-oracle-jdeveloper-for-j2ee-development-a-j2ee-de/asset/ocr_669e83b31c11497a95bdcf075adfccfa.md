```
<tr>
    <th>Journal</th>
    <th>Publisher</th>
    <th>Edition</th>
    <th>Title</th>
    <th>Author</th>
  </tr>
  <xsl:for-each select="ROWSET/ROW">
    <tr>
      <td><xsl:value-of select="JOURNAL"/></td>
      <td><xsl:value-of select="PUBLISHER"/></td>
      <td><xsl:value-of select="EDITION"/></td>
      <td><xsl:value-of select="TITLE"/></td>
      <td><xsl:value-of select="AUTHOR"/></td>
    </tr>
  </xsl:for-each>
  </table>
  </body>
</html>
</xsl:template>
</xsl:stylesheet>
```

Save the catalog.xls stylesheet to the same directory as the queryDB.xsql, the public_html directory, using File | Save. The XSL Stylesheet to transform XSQL output is specified in an XSQL page with xml-stylesheet element:

```
<?xml-stylesheet type="text/xsl" href="catalog.xsl"?>
```

The XSQL page, queryDb.xsql, with an xml-stylesheet element is listed below:

```
<?xml version = '1.0' encoding = 'windows-1252'?>
<?xml-stylesheet type="text/xsl" href="catalog.xsl"?>
<page xmlns:xsql="urn:oracle-xsql" connection="jdbc/xsqlConnectionDS">
    <xsql:set-page-param name="JOURNAL"  value="Oracle Magazine"/>
    <xsql:set-page-param name="PUBLISHER"  value="Oracle Publishing"/>
    <xsql:query bind-params="JOURNAL PUBLISHER">
SELECT  JOURNAL, PUBLISHER, EDITION, TITLE, AUTHOR from
    OE.Catalog WHERE JOURNAL=? AND PUBLISHER=?
</xsql:query>
</page>
```