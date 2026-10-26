There are several reasons to use stored procedures instead of directly putting the SQL in the project code. For one, stored procedures generally perform better; also the code is easier to maintain than hard-coded SQL and we benefit from the increased encapsulation. If the code is only working with database tables and records, it makes sense to leave it in the database.

In this recipe we will show how to modify the wizard-generated code to perform the actual add, update, and deletes your module needs.

# Getting ready

To follow along with this recipe you must have completed these previous recipes:

+   - Creating a new module with the Starter Kit
+   - Creating a database table with keys

> In this recipe we will again locate and replace any occurrences of the phrase YourCompany. If you have followed the previous recipe this is already done and you can skip right to step 8.

# How to do it...

+   1. Start by launching your preferred Development Tool (Visual Studio or Visual Web Developer Express).
+   2. Look for Recent Projects shown on the left side of the screen. Click on Employee to load the project.
+   3. Look under the Solution Explorer on the right side of the screen and double-click on 01.00.00.SqlDataProvider. This will display the creation script in the editor.
+   4. Scroll down past the CREATE TABLE part of the script until you see Drop Existing Stored Procedures.

```
/** Drop Existing Stored Procedures **/

if exists (select * from dbo.sysobjects where id = object_id(N'{databaseOwner} [{objectQualifier} YourCompany_GetEmployees]') and OBJECTPROPERTY (id, N'IsProcedure') = 1)
    drop procedure {databaseOwner} {objectQualifier} YourCompany_GetEmployees
GO

if exists (select * from dbo.sysobjects where id = object_id(N'{databaseOwner} [{objectQualifier} YourCompany_GetEmployee]') and OBJECTPROPERTY (id, N'IsProcedure') = 1)
```