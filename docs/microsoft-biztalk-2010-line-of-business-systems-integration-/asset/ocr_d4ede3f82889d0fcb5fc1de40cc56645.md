The next improvement we will make to the solution is related to how we enrich the sales order information before sending out the final sales order message. A great feature of the ASDK SQL Adapter is that it can handle multiple resultsets. We have created a stored procedure called usp_GetSalesOrderDetails that takes in the sales order ID and performs three distinct select statements, one for each of the three lookups of Example2—the function call, the select statement, and the view call. Here is the stored procedure:

```
```sql
CREATE PROCEDURE [dbo].[usp_GetSalesOrderDetails]
@SalesOrderId [int]
AS
SET no count on
setxact_aborton
SET TRANSACTION ISOLATION LEVEL READ COMMITTED

DECLARE @ContactId int

SELECT * FROM Sales.vSalesOrderDetailProduct
WHERE SalesOrderID = @SalesOrderId

SELECT
at.Name, a.AddressLine1
, a.AddressLine2, a.City
, sp.StateProvinceCode, cr.Name as Country
FROM
Sales.SalesOrderHeader soh
inner join Sales.Customer c on soh.CustomerID = c.CustomerID
inner join Sales.CustomerAddress ca ON C.CustomerID = ca.CustomerID
inner join Person.Address a ON ca.AddressID = a.AddressID
inner join Person.AddressType at ON ca.AddressTypeID =
at.AddressTypeID
inner join Person.StateProvince sp ON a.StateProvinceID =
sp.StateProvinceID
innerjoin Person.CountryRegion cr ON sp.CountryRegionCode =
cr.CountryRegionCode
WHERE soh.SalesOrderID = @SalesOrderId

SELECT @ContactId = ContactId FROM Sales.SalesOrderHeader
WHERE SalesOrderID = @SalesOrderId
SELECT * FROM dbo.ufnGetContactInformation(@ContactId)
```
```