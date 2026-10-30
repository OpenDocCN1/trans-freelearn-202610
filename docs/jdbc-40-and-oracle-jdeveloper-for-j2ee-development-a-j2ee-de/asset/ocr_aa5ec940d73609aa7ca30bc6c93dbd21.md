An XML document is added to the JasperReports project. Copy catalog.xml listing from the previous section to the XML document, catalog.xml. Save catalog.xml to the C:/JasperReports directory with File | Save As. Create a JSP to generate a JasperReports report. Right-click on the JasperReports project node and select New. In the New Gallery window, select Web Tier | JSP in the Categories listed and select JSP in the Items listed. Click on the OK button. In the Create JSP Wizard, click on Next. In the Web Application window, select Servlet 2.4/JSP 2.0 and click on Next. In the JSP File window, specify catalog.jsp as the File Name and select the default Directory Name. Click on Next. Select the default selection in the Error Page Options window and click on Next. In the Tag Libraries window, click on Next. In the HTML Options page, click on Next. In the Finish window, click on Finish. JSP catalog.jsp is added to the JasperReports project.

![](bbox=[0.368, 0.348, 0.63, 0.5])

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