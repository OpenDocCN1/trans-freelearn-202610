```
<field name="Author" class="java.lang.String"/>
<!-- Specify report title.-->
<title>
    <band height="50">
        <textField>
            <reportElement x="350" y="0" width="200" height="50" />
            <textFieldExpression
                class="java.lang.String">$P{ReportTitle}
            </textFieldExpression>
        </textField>
    </band>
</title>
<!-- Specify page header.-->
<pageHeader>
    <band>
    </band>
</pageHeader>
<!-- Specify column headers.-->
<columnHeader>
    <band height="20">
        <staticText>
            <reportElement x="0" y="0" width="100" height="20"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Bold"/>
            </textElement>
            <text><![CDATA[CATALOG ID]]></text>
        </staticText>
        <staticText>
            <reportElement x="125" y="0" width="150" height="20"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Bold"/>
            </textElement>
            <text><![CDATA[JOURNAL]]></text>
        </staticText>
        <staticText>
            <reportElement x="300" y="0" width="150" height="20"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Bold"/>
            </textElement>
            <text><![CDATA[PUBLISHER]]></text>
        </staticText>
        <staticText>
            <reportElement x="475" y="0" width="150" height="20"/>
            <textElement>
```