
The foundation of dynamic user security and visibility in Power BI is the ability to extract the **user principal name (UPN)** or login credential of the business user connected to content in the Power BI service.

Power BI uses **Microsoft Entra ID (Entra ID)** to authenticate users who log in to the Power BI service, and the Power BI login credentials (such as BrettP@FrontlineAnalytics.onmicrosoft.com) are used as the effective username whenever a user attempts to access resources that require authentication.

The USERPRINCIPALNAME DAX function returns the email address used to log in to Power BI. For organizations that use work email addresses for Power BI logins, this effective username maps to a UPN in the local active directory. This then enables filter expressions to be applied to the tables of a model in security roles. In addition to RLS roles, which override and impact all DAX measures of a semantic model, the UPN or *current user* text value can be used by DAX measures to retrieve the UPN prefix and suffix, or filter other measures.

The DAX functions used in this recipe are specific to a parent-child hierarchy that exists in the `DimEmployee` source table. The `DimEmployee` table contains an email address column that will correspond to the value returned by the DAX USERPRINCIPALNAME function when logged in to the Power BI service. Additionally, this recipe is exclusive to Import mode semantic models, as parent-child DAX functions are not currently supported in DirectQuery mode models for either calculated columns or security filter expressions.

The `Employees` table has 296 rows, but a logged-in user should only see their data and the data of those who report to them directly or indirectly. For example, a vice president should still have visibility to see a manager even if the manager reports to a senior manager who reports to the vice president. The senior manager, however, should not be able to view the vice president’s data or the data of a different senior manager. Visibility is limited to the current user’s data and the data of those employees who report to them directly or indirectly via other managers.

The `EmployeeKey` value is the last item in the `ManagementPath` column via the PATH function. The `Current User EmployeeKey` measure, which uses the `Current User Principal Name` to retrieve the `EmployeeKey` value, is passed as the item parameter to the PATHCONTAINS function in the RLS filter expression. The calculated column created in `step 1`, `ManagementPath`, provides the string of values for each `Employees` row to be evaluated against.

## There’s more…

RLS expressions can significantly degrade query performance, as these filters will be applied in addition to other filters and expressions from Power BI reports when members of security roles access this content. As a general rule, try to use relationships between tables with low cardinality to implement dynamic security, as per the following recipe in this chapter. Utility or information functions, such as LOOKUPVALUE, CONTAINS, and PATHCONTAINS, can meet complex security rules in Import mode models but can be very expensive from a performance standpoint when applied against larger-dimension tables—such as customer or product tables containing more than 1 million rows.

## Getting ready

To prepare for this recipe, follow these steps:

- 1. Download Building Dynamic Security for DirectQuery Start.pbix from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition.
- 2. Open Building Dynamic Security for DirectQuery Start.pbix in Power BI Desktop. You may receive an error regarding being able to connect to the data source.
- 3. Click Transform data from the ribbon of the Home tab, and edit the Source step of each of the four queries to point to your SQL server with the AdventureWorksDW2022 database installed.
- 4. Select Close & Apply from the ribbon of the Home tab.
- 5. Save your work.

## How to build dynamic security models for DirectQuery

To implement this recipe, perform the following:

- 1. In the Model view, open the properties for the Countries to Employees relationship, and set the Cross filter direction to Both.
- 2. Check the checkboxes for both Assume referential integrity and Apply security filter in both directions, and then click OK.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_367_0.png)

Figure 8.12: Bidirectional relationship between the Countries and Employees tables

- 3. The model should now look the same as that shown in Figure 8.13:
- 10. Click View as in the ribbon of the Modeling tab, choose Current User, and click the OK button to observe that only the United States column is shown in the clustered column chart.
- 11. Change the Current User Principal Name measure to lynn@adventure-works.com and observe that only Australia is shown in the clustered column chart.

## How it works

In a production environment, the Current User Principal Name measure would not be hardcoded to an email address but would, rather, be the following formula:

```
Current User Principal Name = USERPRINCIPALNAME()
```

When a user mapped to the dynamic security role connects to the DirectQuery semantic model, their UPN is computed via the USERPRINCIPALNAME function. This value filters the Employees table to a single row, which then filters the Countries table via the bidirectional, one-to-many relationship. The filtered countries, such as Australia and the United States, then filter the Sales Territories dimension table. As a final step, the Internet Sales fact table is filtered by the Sales Territories table, and thus all Internet Sales measures and aggregations reflect the given SalesTerritoryCountry of the employee.

Note that the Countries table, which contains only the distinct country values, is necessary, since the Sales Territories table contains many regions for the same country, and all relationships must have a side that identifies each row of a table.

Ensure that Apply security filter in both directions is selected for the bidirectional (both) cross-filter relationship between the Countries and Employees tables. In addition, the Assume referential integrity setting causes the DirectQuery semantic model to send inner join SQL queries to the source database, and this—of course—significantly improves performance with larger models.

While this recipe implements an organizational hierarchy, other hierarchies such as geographic (region and country), departmental, and chart of accounts are also quite common and can be implemented in a similar manner.

## There’s more…

The approach from this recipe can be implemented in the same way for an Import mode model, and it can also be used with a consolidated security role table. For example, instead of an Employees table containing UPNs (email addresses), a Permissions table could be loaded to the model, containing the names of each RLS role and the columns to secure.

For each role, a simple security filter could be applied that references the name of the role. As in this recipe, bridge tables containing the unique values of the secured columns could be created, and security filters would flow across relationships from the Permissions table to the Dimension and Fact tables, via the Bridge table(s).

Given the performance advantage of relationship filtering (including bidirectional relationship filtering), as well as the avoidance of relatively complex DAX, there could be value for organizations to follow this approach to dynamic security for both the Import and DirectQuery models.

Remember that it is important to optimize RLS expressions as much as possible, given that they add an extra layer of filtering to the report that must be evaluated in real time. Thus, it is best practice to configure RLS expressions across dimension tables versus fact tables as, typically, fewer rows are present in dimension tables that must be evaluated.

## See also

- Row-level security (RLS) with Power BI: https://bit.ly/3nhx3zW
- Row-level security (RLS) guidance in Power BI Desktop: https://bit.ly/3tUH3kY
- DirectQuery model guidance in Power BI Desktop: https://bit.ly/3VJuAR0

## Displaying the Current Filter Context

DAX measures can be created to dynamically display the current filter context to report users. These measures can detect filters, retrieve values, and apply conditional logic to the filters applied to both slicer visuals and report- and page-level filters. With the filter context as a visual aid, users consuming or interacting with Power BI reports can focus on the data visualizations to obtain insights more quickly and with greater confidence. In addition, such measures can be useful when designing RLS scenarios as a check that the correct information is filtered by the RLS rules.

In this recipe, DAX measures are created to detect and display the filter selections applied to a specific column, either on the report canvas itself or as a report- or page-level filter. An additional example displays the values of a column that are “remaining,” given the filters applied to the column directly and indirectly via other filters.

## Getting ready

To prepare for this recipe, follow these steps:

- 1. Download Displaying the Current Filter Context Start.pbix from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition.
- 2. Open Displaying the Current Filter Context Start.pbix in Power BI Desktop.
- 3. Edit the AdventureWorks query and change the Source line to point to your SQL server with the AdventureWorksDW2022 database installed.
- 4. Select Close & Apply from the ribbon of the Home tab.
- 5. Save your work.
