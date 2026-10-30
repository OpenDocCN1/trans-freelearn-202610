```
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
        OutputStream ouputStream=new FileOutputStream
            (new File("c:/JasperReports/catalog.xls"));
        ByteArrayOutputStream byteArrayOutputStream = new
            ByteArrayOutputStream();
        JRXlsExporter exporterXLS = new JRXlsExporter();
        exporterXLS.setParameter(
            JRXlsExporterParameter.JASPER_PRINT, print);
        exporterXLS.setParameter(JRXlsExporterParameter.
            OUTPUT_STREAM, byteArrayOutputStream);
        exporterXLS.exportReport();
        ouputStream.write(byteArrayOutputStream.toByteArray());
        ouputStream.flush();
        ouputStream.close();
%>
```

To generate an Excel report right-click on the catalog-excel.jsp and select Run.
The Excel spreadsheet may be opened in MS Excel or Excel Viewer.

![](bbox=[0.18, 0.56, 0.82, 0.75])