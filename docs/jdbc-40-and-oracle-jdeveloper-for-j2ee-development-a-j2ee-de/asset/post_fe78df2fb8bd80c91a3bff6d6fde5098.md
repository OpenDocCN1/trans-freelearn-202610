
<font isUnderline="false" reportFont="Arial_Bold"/>
</textElement>
<text><![CDATA[EDITION]]></text>
</staticText>
<staticText>
    <reportElement x="650" y="0" width="250" height="20"/>
    <textElement>
        <font isUnderline="false" reportFont="Arial_Bold"/>
    </textElement>
    <text><![CDATA[TITLE]]></text>
</staticText>
    <staticText>
        <reportElement x="925" y="0" width="200" height="20"/>
        <textElement>
            <font isUnderline="false" reportFont="Arial_Bold"/>
        </textElement>
        <text><![CDATA[AUTHOR]]></text>
    </staticText>
</band>
</columnHeader>
<!-- Specify column data binding.-->
<detail>
    <band height="20">
        <textField>
            <reportElement x="0" y="0" width="100" height="20"/>
            <textFieldExpression
                class="java.lang.String"><![CDATA[${CatalogId}]]>
            </textFieldExpression>
        </textField>
        <textField pattern="0.00">
            <reportElement x="125" y="0" width="150" height="20"/>
            <textFieldExpression
                class="java.lang.String"><![CDATA[${Journal}]]>
            </textFieldExpression>
        </textField>
        <textField pattern="0.00">
            <reportElement x="300" y="0" width="150" height="20"/>
            <textFieldExpression
                class="java.lang.String"><![CDATA[${Publisher}]]>
            </textFieldExpression>
        </textField>
<textField>
    <reportElement x="475" y="0" width="150" height="20"/>
    <textFieldExpression
class="java.lang.String"><![CDATA[$F{Edition}]]>
</textFieldExpression>
</textField>
<textField pattern="0.00">
    <reportElement x="650" y="0" width="250" height="20"/>
    <textFieldExpression
        class="java.lang.String"><![CDATA[$F{Title}]]>
    </textFieldExpression>
</textField>
<textField>
    <reportElement x="925" y="0" width="200" height="20"/>
    <textFieldExpression
        class="java.lang.String"><![CDATA[$F{Author}]]>
    </textFieldExpression>
</textField>
</band>
</detail>
<columnFooter>
    <band>
    </band>
</columnFooter>
<pageFooter>
    <band height="15">
        <staticText>
            <reportElement x="0" y="0" width="40" height="15"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Italic"/>
            </textElement>
            <text><![CDATA[Page #]]></text>
        </staticText>
        <textField>
            <reportElement x="40" y="0" width="100" height="15"/>
            <textElement>
                <font isUnderline="false" reportFont="Arial_Italic"/>
            </textElement>
            <textFieldExpression
                class="java.lang.Integer"><![CDATA[$V{PAGE_NUMBER}]]>
            </textFieldExpression>
        </textField>
    </band>
</pageFooter>
<summary>
    <band>
    </band>
</summary>
</jasperReport>

## Generating a PDF Report

We will use the JasperReports classes and interfaces to generate a PDF file from the catalog.xml file. Add an XML file to the JasperReports project with **File | New**. In the **New Gallery** window, select **General | XML** in the **Categories** listed and **XML Document** in the **Items** listed. Click on the **OK** button. Specify a **File Name**, catalog.xml, in the **Create XML File** window and click on the **OK** button.

![](img/553e44bc30e70d0d8e58278a7ece8da8_355_0.png)

An XML document is added to the JasperReports project. Copy catalog.xml listing from the previous section to the XML document, catalog.xml. Save catalog.xml to the C:/JasperReports directory with File | Save As. Create a JSP to generate a JasperReports report. Right-click on the JasperReports project node and select New. In the New Gallery window, select Web Tier | JSP in the Categories listed and select JSP in the Items listed. Click on the OK button. In the Create JSP Wizard, click on Next. In the Web Application window, select Servlet 2.4/JSP 2.0 and click on Next. In the JSP File window, specify catalog.jsp as the File Name and select the default Directory Name. Click on Next. Select the default selection in the Error Page Options window and click on Next. In the Tag Libraries window, click on Next. In the HTML Options page, click on Next. In the Finish window, click on Finish. JSP catalog.jsp is added to the JasperReports project.

![](img/553e44bc30e70d0d8e58278a7ece8da8_356_0.png)

The JDBC data source, jdbc/JasperReportsConnectionDS, is required to be configured in the JSP web.xml configuration file. Add the following resource-ref element to the web.xml file:

```
<resource-ref>
<res-ref-name>jdbc/JasperReportsConnectionDS</res-ref-name>
<res-type>javax.sql.DataSource</res-type>
<res-auth>Container</res-auth>
</resource-ref>
```

In the catalog.jsp, first, import the JasperReports classes and interfaces:

```
<%@ page import="java.io.*, java.util.*, java.sql.Connection, javax.sql.DataSource, javax.naming.InitialContext, net.sf.jasperreports.engine.*, net.sf.jasperreports.engine.design.JasperDesign, net.sf.jasperreports.engine.xml.JRXmlLoader" %>
```

Next, create a InputStream for the JasperReports configuration file, catalog.xml. Next, load the XML file with JRXmlLoader class static method load(). The load() method is overloaded and a XML configuration file may be loaded from a File object, an InputStream object, or a String object. The load() method returns a JasperDesign object.

```
InputStream input=new FileInputStream(new File("c:/JasperReports/
 catalog.xml"));
JasperDesign design = JRXmlLoader.load(input);
```

A JasperDesign object represents the report design. Compile the report design file with the JasperCompileManager method compileReport(). The compilation of the report design file validates the JasperReports XML file (catalog.xml) with the jasperreports.dtd DTD and converts the report expressions in a ready-to-evaluate form. The compileReport() method returns a JasperReport object, which represents the compiled report.

```
JasperReport report = JasperCompileManager.compileReport(design);
```

Create a Connection object to retrieve data from the database to create a PDF file. Create a InitialContext object to lookup the data source JNDI with the lookup() method. Obtain a JDBC connection from the data source using the getConnection() method.

```
InitialContext initialContext = new InitialContext();
DataSource ds = (DataSource)initialContext.lookup
("java:comp/env/jdbc/JasperReportsConnectionDS");
Connection conn = ds.getConnection();
```

Generate a JasperReports report from the compiled report. Fill the compiled report with data from the database using the fillReport() method of the JasperFillManager class. The fillReport() method returns a JasperPrint object, which represents the report document that may be viewed, printed, or exported to other formats:

```
JasperPrint print =JasperFillManager.fillReport(report,parameters,
    conn);
```

The parameters in the fillReport() method consist of the parameter values specified in the <parameter/> elements of the XML configuration file. Catalog.xml has the parameter ReportTitle, therefore specify a value for the ReportTitle parameter.

```
Map parameters = new HashMap();
parameters.put("ReportTitle", "PDF JasperReport");
```

A JasperReports report may be exported to a XML file, a PDF file, an HTML file, a CSV file, or a Excel XLS file. Export the generated JasperReports to a PDF file using the exportReportToPdfStream() method of the JasperExportManager class:

```
OutputStream output=new FileOutputStream(new File("c:/JasperReports/
 catalog.pdf"));
JasperExportManager.exportReportToPdfStream(print, output);
```

The JSP file, catalog.jsp, is listed below:

```
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page import="java.io.*, java.util.*, java.sql.Connection, javax.sql.DataSource, javax.naming.InitialContext, net.sf.jasperreports.engine.*, net.sf.jasperreports.engine.design.JasperDesign,net.sf.jasperreports.engine.xml.JRXmlLoader" %>
<%InputStream input=new FileInputStream(new
File("c:/JasperReports/catalog.xml"));
JasperDesign design = JRXmlLoader.load(input);
JasperReport report = JasperCompileManager.compileReport(design);
Map parameters = new HashMap();
parameters.put("ReportTitle", "PDF JasperReport");
InitialContext initialContext = new InitialContext();
DataSource ds = (DataSource)initialContext.lookup
("java:comp/env/jdbc/JasperReportsConnectionDS");
Connection conn = ds.getConnection();
JasperPrint print = JasperFillManager.fillReport(report,
parameters, conn);
OutputStream output=new FileOutputStream
(new File("c:/JasperReports/catalog.pdf"));
JasperExportManager.exportReportToPdfStream(print, output);%>
```

Copy code from the catalog.jsp listing to catalog.jsp in the JasperReports application. To run the catalog.jsp JSP, right-click on the catalog.jsp node and select Run.

![](img/553e44bc30e70d0d8e58278a7ece8da8_358_0.png)
