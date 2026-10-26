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