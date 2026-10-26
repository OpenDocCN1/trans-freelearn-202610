
11. The last change for the manifest is to update the version number on the assembly:

```
<assemblies>
    <assembly>
        <path>bin</path>
        <name>Employee.dll</name>
        <version>01.01.00</version>
    </assembly>
</assemblies>
```

12. Select Save All from the File menu to save your changes.

13. Rebuild the project and create an installation ZIP file according to the instructions in the recipe Deploying a module as a standalone package from Chapter 5, Building, Debugging, and Deploying Modules. Include both the 01.00.00 and 01.01.00 SqlDataProvider files.

14. Name the new ZIP file Employee_01.01.00_Install.zip.

15. The Employee_01.01.00_Install.zip file should now contain the following:

| Name | Type | Packe... | Has ... | Size |
|---|---|---|---|---|
| App_LocalResources | File Folder | 0 KB |  | 0 KB |
| Bin | File Folder | 0 KB |  | 0 KB |
| EditEmployee.ascx | ASP.NET User Control | 1 KB | No | 4 KB |
| Settings.ascx | ASP.NET User Control | 1 KB | No | 1 KB |
| ViewEmployee.ascx | ASP.NET User Control | 2 KB | No | 5 KB |
| Employee.dnn | DNN File | 2 KB | No | 6 KB |
| 01.00.00.SqlDataProvider | SQLDATAPROVIDER File | 2 KB | No | 8 KB |
| 01.01.00.SqlDataProvider | SQLDATAPROVIDER File | 1 KB | No | 3 KB |
| Employee.SqlDataProvider | SQLDATAPROVIDER File | 1 KB | No | 3 KB |
| Uninstall.SqlDataProvider | SQLDATAPROVIDER File | 1 KB | No | 2 KB |

16. To check the results, log in as SuperUser and use the Install Extension Wizard to deploy the new ZIP file. The module should install over the existing module without error.

17. You can use the Database Explorer in the Development tool to confirm the upgrade added the new columns:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_361_0.png)

18. The Database Explorer will also show the updated stored procedure.

## How it works...

It is important that you think carefully about the changes you need to make in your module upgrade. Keep in mind you must perform the upgrade without affecting the previous module versions. For example, to add columns to the Employee table we used the ALTER TABLE command rather than the DROP TABLE command. This preserves any records that might already be in the table.

Incremental updates like this are tricky and should be well tested before releasing your update. They also require a solid understanding of SQL.

## There's more...

In this example we did not create any new objects so the original uninstall file still applies. If we create a new table for example, then we have to update the uninstall file with a new version number. As we never do incremental uninstalls, we only need to have one uninstall file.

## Using multiple database connections

There are some situations when you need to access a database that is not the one running the DNN installation. Perhaps you need to run a query against a legacy database or extract information from a different company database.

In this recipe we will see how to create a second database connection and use it to populate the **Manager** drop-down list from the recipe *Populating a drop-down list from a stored procedure*.

For this recipe we will use the sample Northwind database that comes with MS Access.

## Getting ready

To follow along with this recipe you must have completed the following recipe from *Chapter 6, Data Entry Tricks*:

- *Populating a drop-down list from a stored procedure*

## How to do it...

- 1. Download the sample Northwind database from the Microsoft site by going to http://www.microsoft.com/downloads and searching for **Northwind Traders Sample Database.**
- 2. Select **Access 2000 Tutorial: Northwind Traders Sample Database** and click on the **Download** button to begin the download.
- 3. When prompted, select Run and agree to the install and license agreement.
- 4. Place the file on the C drive (c:\Nwind.mdb) of the DNN server.
- 5. Look on the DNN server and locate the root folder of the DNN installation.
- 6. Find the web.config file and open it in a text editor.
- 7. Scroll down through the file until you see <connectionStrings> and add the following key:
  ```
  <add name="LegacyAccess"
      connectionString="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=C:\Nwind.mdb; User Id=admin;Password=" />
  ```
- 8. Next, double-click to open the ViewEmployee.ascx.vb file.
- 9. As we are accessing an old MS Access database in this recipe, we need to use the OLE DB provider. So start by adding the necessary import directive at the top of the file:
  ```
  Imports System.Data.OleDb
  ```
- 10. Next we'll create a new routine to connect to the MS Access database file using the connection string we saved in the web.config file. Scroll down to the bottom of the Public Methods region and add the following code:
  ```
  Public Function GetLegacyManagers() As OleDbDataReader
  
      Dim sqlConn As OleDbConnection
      Dim sqlCmd As OleDbCommand
      Dim strConnection As String
  ```

14. Go to the page with the Employee module. The names in the Manager drop-down list are now coming from a second database connection to an MS Access database.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_364_0.png)

## How it works...

In this example we used an OLE connection to a MS Access 2000 database, but this technique works with other database types. Here are some other example connection strings for different types of databases:

| Database type | Sample connection string |
| --- | --- |
| Oracle Database using a TNSNames. ora file | <add name="LegacyOracle" connectionString="Data Source=LEGACY;User Id=legacyuser; Password=legacypassword;" /> |
| MS Access | <add name="LegacyAccess" connectionString="Provider=Microsoft.Jet.OLEDB.4.0; Data Source=C:\northwind.mdb;User Id=legacyuser;Password=legacypassword;" /> |
| SQL Server Express | <add name="LegacyExpress" connectionString="Data Source=.\SQLEXPRESS;Initial Catalog=LegacyDatabase;User ID=legacyuser;Password=legacypassword" /> |
| MySQL | <add name="LegacyMySQL" connectionString="Server=localhost;Port=3306;Database=LegacyMySQL;Uid=legacyuser;Pwd=legacypass word;" /> |

## 12

## Advanced Modules and Security

- Creating a secondary View control
- Controlling navigation with NavigateURL
- Module messages and event Logging
- Creating your own CBO Hydrator
- Using Active Directory with DNN
- Synchronizing security roles with DNN
- Correcting Active Directory issues

## Introduction

In this chapter, we will touch on some advanced module development topics and look at extending DNN security. We'll start by seeing how to create a secondary View control and navigation links to it. Then we'll see how to create custom display messages and use the event log for debugging messages. Lastly we'll see how to add Active Directory Authentication and synchronize it with DNN.

4. Select **New Item...** from the **Add** pop-up menu.

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_367_0.png)

- 5. Pick **Web User Control** as the template and call it `DisplayEmployee.ascx`.
- 6. Click on **Add** to create the file.
- 7. Double-click to open the new `DisplayEmployee.ascx` file.
- 8. To display the employee information, we'll create some label controls and organize them in an HTML table. Add the following code to the top of the file:

```
<%@ Control Language="vb" AutoEventWireup="false"
    CodeBehind="DisplayEmployee.ascx.vb"
    Inherits="ACME.Modules.Employee.DisplayEmployee" %>
<%@ Register TagPrefix="dnn" TagName="Label"
    Src="~/Controls/LabelControl.ascx" %>
```

- 9. Below that place an HTML table holding the controls:

```
<table width="650" cellspacing="0" cellpadding="0" border="0"
       summary="Display Table">
    <tr>
        <td width="40%" valign="top" align="center">
            <asp:Image ID="imgEmployee" runat="server"
                ImageUrl="~/images/thumbnail.jpg" />
        </td>
        <td width="60%">
            <table width="100%" >
                <tr valign="top">
                    <td class="SubHead" width="125">
```
