```
EmployeeURL,
SecurityLevel,
{objectQualifier}ACME_Employee.CreatedByUser,
{objectQualifier}ACME_Employee.CreatedDate,
'CreatedByUserName' = {objectQualifier}Users.FirstName + ' ' + {objectQualifier}Users.LastName
FROM {objectQualifier}ACME_Employee
INNER JOIN {objectQualifier}Users on
    {objectQualifier}ACME_Employee.CreatedByUser =
    {objectQualifier}Users.UserId
INNER JOIN #PageIndex PageIndex
ON {objectQualifier}ACME_Employee.ItemId = PageIndex.ItemId
WHERE ( (PageIndex.IndexID > @PageLowerBound) OR
        @PageLowerBound is null )
    AND ( (PageIndex.IndexID < @PageUpperBound) OR
        @PageUpperBound is null )
ORDER BY
    PageIndex.IndexID
GO
```

+   8. Next we need to update the manifest file to include the new 01.01.00. SqlDataProvider file. Double-click to open the Employee.dnn file.

+   9. At the top of the file, update the version number to 01.01.00:

```
<packages>
  <package name="ACME.Employee" type="Module"
          version="01.01.00">
    <friendlyName>ACME Employees</friendlyName>
```

+   10. Scroll down until you see the <scripts> section and add the following code:

```
<scripts>
  <basePath>DesktopModules\ACME.Employee</basePath>
  <script type="Install">
    <name>01.00.00.SqlDataProvider</name>
    <version>01.00.00</version>
  </script>
  <script type="Install">
    <name>01.01.00.SqlDataProvider</name>
    <version>01.01.00</version>
  </script>
  <script type="UnInstall">
    <name>Uninstall.SqlDataProvider</name>
    <version>01.00.00</version>
  </script>
</scripts>
```