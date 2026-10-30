
A catalog.pdf PDF file is generated. The PDF file can be opened in an Adobe Acrobat Reader.

![](img/553e44bc30e70d0d8e58278a7ece8da8_359_0.png)

## Creating an Excel Spreadsheet

Create a catalog-excel.jsp in the JasperReports project in JDeveloper for creating a Excel spreadsheet. If an Excel report is required, use a JRXlsExporter object to export the JasperReports document to an Excel spreadsheet. Specify an OutputStream to output the Excel spreadsheet. Specify a ByteArrayOutputStream for the output from the JRXlsExporter object:

```
OutputStream outputStream=new FileOutputStream
(new File("c:/JasperReports/catalog.xls"));
ByteArrayOutputStream byteArrayOutputStream = new
ByteArrayOutputStream();
```

Create a JRXlsExporter object and set the JasperPrint object from which the report is to be generated and the OutputStream to output the report.

```
JRXlsExporter exporterXLS = new JRXlsExporter();
exporterXLS.setParameter(JRXlsExporterParameter.JASPER_PRINT, print);
exporterXLS.setParameter
(JRXlsExporterParameter.OUTPUT_STREAM,byteArrayOutputStream);
```

Export the JasperReport document using the exportReport() method:

```
exporterXLS.exportReport();
```

Output the JasperReports excel report to catalog.xls file:

```
outputStream.write(byteArrayOutputStream.toByteArray());
outputStream.flush();
outputStream.close();
```

The catalog-excel.jsp is listed as follows:

```
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page import="java.io.*, java.util.*, java.sql.Connection, javax.
sql.DataSource, javax.naming.InitialContext,
net.sf.jasperreports.engine.*, net.sf.jasperreports.engine.design.
JasperDesign,net.sf.jasperreports.engine.xml.JRXmlLoader,
net.sf.jasperreports.engine.export.*"
%>
<%
    InputStream input=new FileInputStream
        (new File("c:/JasperReports/catalog.xml"));
    JasperDesign design = JRXmlLoader.load(input);
    JasperReport report = JasperCompileManager.compileReport(design);
    Map parameters = new HashMap();
    parameters.put("ReportTitle", "Excel JasperReport");
    InitialContext initialContext = new InitialContext();
    DataSource ds = (DataSource)initialContext.lookup
        ("java:comp/env/jdbc/JasperReportsConnectionDS");
    Connection conn = ds.getConnection();
    JasperPrint print = JasperFillManager.fillReport
        (report, parameters, conn);
        OutputStream outputStream=new FileOutputStream
            (new File("c:/JasperReports/catalog.xls"));
        ByteArrayOutputStream byteArrayOutputStream = new
            ByteArrayOutputStream();
        JRXlsExporter exporterXLS = new JRXlsExporter();
        exporterXLS.setParameter(
            JRXlsExporterParameter.JASPER_PRINT, print);
        exporterXLS.setParameter(JRXlsExporterParameter.
            OUTPUT_STREAM, byteArrayOutputStream);
        exporterXLS.exportReport();
        outputStream.write(byteArrayOutputStream.toByteArray());
        outputStream.flush();
        outputStream.close();
%>
```

To generate an Excel report right-click on the catalog-excel.jsp and select Run.
The Excel spreadsheet may be opened in MS Excel or Excel Viewer.

![](img/553e44bc30e70d0d8e58278a7ece8da8_360_0.png)

## JDBC 4.0 Version

In the JDBC 4.0 version of the JasperReports application, to generate a PDF report, use the Oracle database 11g JDBC 4.0 drivers JAR file, ojdbc6.jar, instead of the Oracle JDBC library. JDBC 4.0 drivers require JDK 6.0. Therefore, set the J2SE Version to JDK 6.0 in the Project Properties window. To modify the J2SE Version, select the Libraries node in Project Properties window. Click on the Change button for the J2SE Version field. In the Edit J2SE Definition window, click on New to create a J2SE definition for JDK 6.0. In the Create J2SE window, select a JDK 6.0 Java Executable and click on OK. Select the JDK 6.0 definition in Edit J2SE Definition window and click on OK. The J2SE Version gets set to JDK 6.0 in Project Properties window. Click on OK in Project Properties.

The JDBC 4.0 features can be availed in a JDeveloper version that supports JDBC 4.0 in the embedded OC4J. JDBC 4.0 provides enhanced connection management with connection state tracking and connection client identification. Connection state tracking is implemented by the connection pool manager. If some of the connections in a connection pool become unusable, the connection pool manager closes those connections. Prior to the connection state tracking feature, the connection pool manager typically closed all connections and reinitialized the connection pool if some of the connections became unusable. Closing connections and opening new connections incurs an overhead. The Connection interface in JDK 6.0, which supports JDBC 4.0, has a isValid() method to test the validity of a connection. The connection pool manager invokes the isValid method on connections and if a connection returns false the connection pool manager closes the connection using close() method as follows:

```
if(!connection.isValid())
    connection.close();
```

The connection client identification is implemented by the JDBC driver. Connection client identification is used to identify connections that might be bogging down an application due to excessive usage of CPU. The JDK 6.0 Connection interface provides two new methods for connection client identification; setClientInfo() and getClientInfo(). A Connection object is identifiable by the client info properties set on the Connection using setClientInfo(). The different client info properties supported by a database may be obtained from the database metadata using getClientInfoProperties method of the DatabaseMetaData interface.

```
DatabaseMetaData metaData=connection.getMetaData();
ResultSet clientInfo=metaData.getClientInfoProperties();
```

The standard client info properties are ApplicationName, ClientUser, and ClientHostname. In the JDBC 4.0 version of the JasperReports application, set the standard client info properties as follows:

```
connection.setClientInfo("ApplicationName","OracleApp");
connection.setClientInfo("ClientUser","OracleUser");
connection.setClientInfo("ClientHostname","OracleHost");
```

In JDBC 4.0 version of the JasperReports application we may also avail the support for Java SE chained exceptions facility. Specify an error page in the JasperReports JSP and in the error page, retrieve the chained exceptions and chained causes using the enhanced for-each loop introduced in J2SE 5.0 as follows:

```
for(Throwable e : exception ) {
    out.println("Error encountered: " + e);
}
```

The JDBC 4.0 version of the JasperReports application to generate a PDF report is listed below.

```
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page import="java.io.*, java.util.*, java.sql.Connection, javax.sql.DataSource, javax.naming.InitialContext, net.sf.jasperreports.engine.*, net.sf.jasperreports.engine.design.JasperDesign,net.sf.jasperreports.engine.xml.JRXmlLoader" %>
<%
    InputStream input=new FileInputStream
    (new File("c:/JasperReports/catalog.xml"));
    JasperDesign design = JRXmlLoader.load(input);
    JasperReport report = JasperCompileManager.compileReport(design);
    Map parameters = new HashMap();
    parameters.put("ReportTitle", "PDF JasperReport");
    InitialContext initialContext = new InitialContext();
    DataSource ds = (DataSource)initialContext.lookup
    ("java:comp/env/jdbc/JasperReportsConnectionDS");
    Connection conn = ds.getConnection();
    connection.setClientInfo("ApplicationName","OracleApp");
    connection.setClientInfo("ClientUser","OracleUser");
    connection.setClientInfo("ClientHostname","OracleHost");

    JasperPrint print = JasperFillManager.fillReport(report, parameters, conn);
    OutputStream output=new FileOutputStream(new File("c:/JasperReports/catalog.pdf"));
    JasperExportManager.exportReportToPdfStream(print, output);
%>
```

The same modifications apply to the JasperReports application to generate an Excel spreadsheet.

## Summary

JasperReports is a Java reporting tool to generate HTML, XML, PDF, CSV, or XLS reports. In this chapter we learned how to create a PDF and an Excel spreadsheet report from a database table using JDBC to connect to the database and run an SQL query. We added JDBC 4.0 features connection state tracking and connection client identification in the JDBC 4.0 version of the JasperReports application.
