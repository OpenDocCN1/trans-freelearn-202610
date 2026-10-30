| Tag | Description | Attributes |
|---|---|---|
| url | Creates a URL with optional query parameters. | All attributes are of String type.
var- Name of the exported scoped variable for the processed URL.
scope- Scope of variable.
value- URL to be processed.
context- Context for a relative URL. |
| when | Subtag that would be evaluated, if a test condition evaluates to true. | test (boolean)- Required attribute that specifies the test condition. |

# Setting the Environment

Download JDeveloper 10.1.3 from: http://www.oracle.com/technology/software/products/jdev/index.html. Extract the JDeveloper zip file to a directory, which installs the JDeveloper IDE. Download the Oracle database. Install the Oracle database including the sample schemas. We need to copy the Oracle database 11g JDBC drivers JAR file, ojdbc6.jar, for JDBC 4.0 to C:\JDeveloper\j2ee\home\applib directory. Keep a copy of the JDBC JAR files and delete them from the C:\JDeveloper\jdbc\lib directory. Copy back the deleted JDBC JAR files to the directory after running the JSTL application. We also need to set the J2SE Version to JDK 6.0 as explained in Chapter 2. In JDeveloper, create a project for the JSTL SQL taglib. Select File | New, and in the New Gallery window, select General in the Categories list and Application in the Items list. Click on OK button. In the Create Application window, specify an Application Name, JSTLApp, and click on OK. In the Create Project window specify a Project Name, JSTLSQL, and click on OK. An Application and a Project node get added to the Applications Navigator.

![](bbox=[0.33, 0.62, 0.67, 0.74])