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