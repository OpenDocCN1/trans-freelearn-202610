# 12

Creating a Report with JasperReports

JasperReports is a Java reporting tool to prepare reports for presentation. A JasperReports report is an HTML, PDF, Excel XLS, CSV, or a XML report. The data for a JasperReports report is static data or is data retrieved from a database table with an SQL query. JasperReports is designed to be included in Java/J2EE applications to generate dynamic presentation reports.

In this chapter, we will create JasperReports reports in JDeveloper. JasperReports uses JDBC to connect to database and retrieve data for a JasperReports report. A JDBC connection with the Oracle database 10g is established in the JasperReports configuration file (.xml file). We will create a PDF report and an Excel spreadsheet report using JasperReports. In this chapter we will learn the following:

+   - Create a PDF Report with JasperReports
+   - Create a Excel Spreadsheet with JasperReports

# Setting the Environment

Download the open-source JasperReports tool JAR file `jasperreports-2.0.5.jar`. Download the iText Java-PDF library `itext-2.1.0.jar`. Download the Jakarta-POI ZIP file `poi-bin-3.0.2-FINAL-20080204.zip`. Download `Commons Digester 1.8` and extract the ZIP file to a directory. Install the JDeveloper 10g IDE Studio Edition Complete Install. Download and install the Oracle database from: http://www.oracle.com/database/index.html 10g.

Create an example database table, `Catalog` from which a JasperReports PDF report and Excel report will be generated. Create the database table in Oracle SQL *Plus. The SQL script to create the example table is listed below.