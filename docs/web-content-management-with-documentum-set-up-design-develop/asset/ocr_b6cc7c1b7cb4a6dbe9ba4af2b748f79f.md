```
<!-- Displaying the name of the file ('object_name' attribute) -->
- <td>
  - <b>
    - <u>
        <xsl:value-of select="$ObjectName" />
      </u>
    </b>
  </td>
</tr>
- <tr>
  - <td>
      <br />
    </td>
  </tr>
- <tr>
  <!-- Displaying short description -->
  - <td>
    <xsl:copy-of select="SHORTDESC" />
  </td>
</tr>
- <tr>
  - <td>
      <br />
    </td>
  </tr>
</table>
</xsl:template>
<!-- Template code for integrating DQL queries in XSL Starts -->
- <xsl:template name="XDQL">
  <xsl:param name="dql" />
  <xsl:param name="rootTag" select="string('xdql')" />
  <xsl:param name="UpperTags" select="string('false')" />
  <xsl:param name="MaxRows" select="10" />
  <xsl:param name="RowIDAttrName" select="ID" />
  <xsl:param name="RowsetTag" select="Row" />
  <xsl:param name="UseGivenCaseTagNames" select="true" />
  <xsl:param name="UseNullAttrIndicator" select="false" />
  <xsl:variable name="xdql" select="java:com.documentum.xml.xdql.DfXmlQuery.new()" />
  <xsl:variable name="init" select="java:init($xdql)" />
  <xsl:variable name="param" select="java:setDql($xdql,$dql)" />
  <xsl:variable name="param1" select="java:setRootNode($xdql,$rootTag)" />
  <xsl:variable name="param3" select="java:includeContent($xdql,false())" />
  <xsl:variable name="setContentEncoding" select="java:setContentEncoding($xdql,string('dom'))" />
  <xsl:variable name="setContentFormat" select="java:setContentFormat($xdql,string('xml'))" />
  <xsl:variable name="execute" select="java:execute($xdql, 'DF_READ_QUERY', $DMS_SESSION_ID)" />
  <xsl:variable name="queryresult" select="java:getXMLDOM($xdql)" />
  <xsl:copy-of select="$queryresult" />
</xsl:template>
<!-- Template code for integrating DQL queries in XSL Ends -->
</xsl:stylesheet>
```

Figure 15.17: Sample XSL file using XDQL—continued