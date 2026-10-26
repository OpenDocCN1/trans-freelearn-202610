
## How to do it...

- 1. Start by launching the Development Tool and loading the Employee project.
- 2. Double-click to open the Employee.dnn file.
- 3. Scroll down until you see the section labeled <license>.
   `<license src="license.txt" />.`
- 4. Replace the reference to license.txt with a character data tag (CDATA) and the HTML representing your formatted license text:
```
<license>
<![CDATA[
<h1>SAMPLE END USER LICENSE AGREEMENT (EULA)</h1>
This End-User License Agreement ("EULA") is a legal agreement made and entered into by you ("LICENSEE"), the person, business or other entity which will be bound by and subject to the terms and conditions set forth in this EULA, and ACME Incorporated ("ACME"), the author of SOFTWARE (as defined below). By installing and using the SOFTWARE, LICENSEE agrees to be bound by all of the terms and conditions of this EULA. If LICENSEE does not agree to the terms of this EULA, LICENSEE may not install or use the SOFTWARE.
<p>
<h3>INTRODUCTION</h3>
<h3>DEFINITIONS</h3>
<h3>MORE STUFF LIKE THIS...</h3>
]]>
</license>
```
- 5. The same trick also works in the release notes section. Try the following code for the release notes:
```
<releaseNotes>
<![CDATA[
<h1>RELEASE NOTES</h1>
These are some sample release notes demonstrating HTML formatting.
<h3>Version 01.00.00</h3>
<table border="1">
<tr>
<th>Change ID</th>
<th>Date</th>
<th>Description</th>
</tr>
<tr>
<th>Change ID</th>
<th>Date</th>
<th>Description</th>
</tr>
<tr>
<td>12345</td>
```
```
<td>10/01/2010</td>
<td>Fixed bug in manifest file.</td>
</tr>
</table>
]]>
</releaseNotes>
```

6. Finally, you can use this same technique to provide a better company URL and e-mail link in the Owner section. Normally when the owner is displayed in the package information the URL and e-mail are displayed as fixed text:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_354_0.png)

7. You can turn the company URL into a real hyperlink and the e-mail address into a mailto: link with the following code:

```
<owner>
  <name>John Smith</name>
  <organization>ACME Corporation</organization>
  <url><![CDATA[<a href="http://www.mycompany.xyz/">http://www.mycompany.xyz/</a>]]></url>
  <email><![CDATA[<a href="mailto:support@mycompany.xyz">support@mycompany.xyz</a>]]></email>
</owner>
```

8. To check the results, log in as SuperUser and use the Install Extension Wizard to deploy the module.

9. When the wizard displays the Package Information page the new links will appear:

![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_354_1.png)

## Deploying a new module version

In previous recipes we have seen how to create and deploy a DNN module. But what happens when you have a new version that you want to deploy? How do you redeploy a module so it upgrades the previous version rather than replacing it?

In this recipe we will see how to assign version numbers to a module and update the manifest file to create a seamless module upgrade.

There are three things to consider when planning a module upgrade:

- - What changes do you need to make to the database?
- - Has the assembly (the DLL file) changed?
- - Have you added any new files (images, forms, and so on)?

### Getting ready

To follow along with this recipe you must have completed the following recipe from *Chapter 5, Building, Debugging, and Deploying Modules:*

- - *Deploying a module as a standalone package*

In this recipe we will enhance the sample Employee project by adding three new fields to the employee table: `DepartmentID`, `EmployeeURL`, and `SecurityLevel`. This means adding new table columns, updating a stored procedure, and rewriting the manifest file.

### How to do it...

- 1. We'll start by creating a new `SqlDataProvider` script that will create the new fields in the database. Launch the Development Tool and load the Employee project.
- 2. In the Solution Explorer, right-click on the Employee project.
- 3. Select **New Item...** from the **Add** pop-up menu.
- 4. Pick **Text File** as the template and called it `01.01.00.SqlDataProvider`.
- 5. Click on **Add** to create the file.
- 6. To create the three new fields in the Employee table, add the following code to the top of the file:

```
```sql
/** Alter Table **/

if not exists (select * FROM syscolumns WHERE (name =
    'DepartmentID') AND (id = OBJECT_ID(N'{databaseOwner}
    [{objectQualifier}ACME_Employee]')))
```
```
```
BEGIN

ALTER TABLE {databaseOwner}][{objectQualifier}ACME_Employee]
ADD DepartmentID VARCHAR (30) ;

ALTER TABLE {databaseOwner}][{objectQualifier}ACME_Employee]
ADD EmployeeURL VARCHAR (120);

ALTER TABLE {databaseOwner}][{objectQualifier}ACME_Employee]
ADD SecurityLevel VARCHAR (30);

END

GO
```

7. Next, we'll add the code to drop the existing GetEmployeesByFilter procedure from version one and replaced it with an improved version:

```
/** Drop Existing Stored Procedures **/

if exists (select * from dbo.sysobjects where id =
    object_id(N'{databaseOwner}][{objectQualifier}
    ACME_GetEmployeesByFilter]') and OBJECTPROPERTY(id,
        N'IsProcedure') = 1)
    drop procedure {databaseOwner}{objectQualifier}ACME_
GetEmployeesByFilter
GO
```

```
/** Recreate stored procedure with new columns **/

CREATE procedure {databaseOwner}{objectQualifier}ACME_
GetEmployeesByFilter
@ModuleID int,
@MgrFilter int,
@SalaryFilter  int,
@PageIndex int,
@PageSize int
AS
```

```
/** If we pass -1 for a numeric filter it means ignore the filter **/

DECLARE @PageLowerBound int
```
```
DECLARE @PageUpperBound int

/** Calculate the upper and lower bounds based on the PageIndex and PageSize **/

SET @PageLowerBound = @PageSize * @PageIndex
SET @PageUpperBound = @PageLowerBound + @PageSize + 1

/** create a temp table to hold the Ids of our results **/

CREATE TABLE #PageIndex
(
    IndexID    int IDENTITY (1, 1) NOT NULL,
    ItemId     int
)

/** populate the temp temp with the filtered results **/

INSERT INTO #PageIndex (ItemId)
SELECT ItemId
FROM {objectQualifier}ACME_Employee
WHERE (ModuleId = @ModuleId) AND  (ManagerNo = @MgrFilter OR @MgrFilter = -1) AND (Salary >= @SalaryFilter)
ORDER BY ItemId DESC

/** remember the count of the results **/

SELECT COUNT(*) as TotalRecords
FROM #PageIndex

/** join the filtered Ids to the employee table to get our fields **/

select {objectQualifier}ACME_Employee.ModuleId,
       {objectQualifier}ACME_Employee.ItemId,
       EmpFirstName,
       EmpLastName,
       ManagerNo,
       HireDate,
       Salary,
       DepartmentID,
```
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

- 8. Next we need to update the manifest file to include the new 01.01.00. SqlDataProvider file. Double-click to open the Employee.dnn file.

- 9. At the top of the file, update the version number to 01.01.00:

```
<packages>
  <package name="ACME.Employee" type="Module"
          version="01.01.00">
    <friendlyName>ACME Employees</friendlyName>
```

- 10. Scroll down until you see the <scripts> section and add the following code:

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
