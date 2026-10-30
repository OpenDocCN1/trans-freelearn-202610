A catalog.pdf PDF file is generated. The PDF file can be opened in an Adobe Acrobat Reader.

![](bbox=[0.17, 0.19, 0.83, 0.285])

# Creating an Excel Spreadsheet

Create a catalog-excel.jsp in the JasperReports project in JDeveloper for creating a Excel spreadsheet. If an Excel report is required, use a JRXlsExporter object to export the JasperReports document to an Excel spreadsheet. Specify an OutputStream to output the Excel spreadsheet. Specify a ByteArrayOutputStream for the output from the JRXlsExporter object:

```
OutputStream ouputStream=new FileOutputStream
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
ouputStream.write(byteArrayOutputStream.toByteArray());
ouputStream.flush();
ouputStream.close();
```

The catalog-excel.jsp is listed as follows:

```
<%@ page contentType="text/html;charset=windows-1252"%>
<%@ page import="java.io.*, java.util.*, java.sql.Connection, javax.
sql.DataSource, javax.naming.InitialContext,
net.sf.jasperreports.engine.*, net.sf.jasperreports.engine.design.
JasperDesign,net.sf.jasperreports.engine.xml.JRXmlLoader,
```