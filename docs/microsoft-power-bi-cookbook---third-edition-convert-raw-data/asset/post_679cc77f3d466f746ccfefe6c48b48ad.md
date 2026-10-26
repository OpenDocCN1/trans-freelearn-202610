
## 8. Implementing Dynamic User-Based Visibility in Power BI

Data security, wherein users or groups of users are prevented from viewing a portion of a semantic model, is often a top requirement in Power BI deployments. Security implementations can range in complexity from mapping a few security groups to their associated row-level security roles based on a single dimension value, such as department or region, to user-based security involving DAX measures and user permission tables which retrieve the report user’s identity and filter the data model based on this identity, respectively.

Given the variety of use cases and the importance of this feature to securely share a semantic model across stakeholders, it is important to understand the process and techniques available to develop, test, and operationalize data security roles.

In addition to row-level security (RLS) roles, dynamic, user-based filter context techniques can also be used to simplify and personalize the user experience. For example, the filter conditions built into reports, as well as the interactive filter selections from end users, can be dynamically updated and displayed in intuitive visuals to aid comprehension.

In more advanced scenarios, DAX measures themselves can be filtered, based on information about the user interacting with the content to deliver a personalized experience. This chapter contains detailed examples of building and deploying dynamic, user-based security for both the Import and DirectQuery semantic models, and examples of dynamic filter context functionality to enhance the user experience.

In this chapter, we will cover the following recipes:

- Defining RLS roles and filter expressions
- Designing dynamic security models
- Building dynamic security for DirectQuery
- Displaying the current filter context
- Avoiding manual user clicks

### Technical Requirements

The following are required to complete the recipes in this chapter:

- Power BI Desktop.
- SQL Server 2022 or newer with the AdventureWorksDW2022 database installed. See *Installing SQL Server and the AdventureWorks Database* recipe from *Chapter 1, Installing and Licensing Power BI Tools*.
- Files for this chapter can be downloaded from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition.

### Defining RLS Roles and Filter Expressions

Data security should always be top of mind, and BI teams and developers should strive to ensure that users are never granted greater access or permission than is necessary to perform a legitimate business function.

In the context of Power BI projects, the question *Who should be able to see what?* should be well defined and documented before any reports are published or read access to a semantic model is granted. Additionally, there should be a well-defined process to create and manage the security groups used in security implementations.

The data security of Power BI models comprises security roles defined within a model, with each role containing a unique set of one or more DAX filter expressions. Roles and their associated filter expressions are created in Power BI Desktop, and users or groups are mapped to security roles in the Power BI service. A single DAX filter expression can be applied to each table of a model within a given security role, with users having the option of being mapped to multiple security roles. The filter expressions applied to tables within a security role also filter other tables in the model via relationships defined in it, like the filters applied to Power BI reports, and are applied to all queries submitted by the security role member.

This recipe contains an end-to-end example of configuring, deploying, and validating RLS roles, applicable to both the Import and DirectQuery semantic models. Additional guidance on a consolidated security role table that improves the manageability of changing security requirements is also included.

#### Getting ready

- 1. Download Defining RLS Roles and Filter Expressions Start.pbix from the following GitHub repository: https://github.com/PacktPublishing/Power-BI-Cookbook-Third-Edition.
- 2. Open Defining RLS Roles and Filter Expressions Start.pbix in Power BI Desktop.
- 3. Edit the AdventureWorks query, and change the Source line to point to your SQL server with the AdventureWorksDW2022 database installed.
- 4. Select Close & Apply from the ribbon of the Home tab.
- 5. Save your work.

The semantic model in this file looks like the following:

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_357_0.png)

Figure 8.1: Semantic model for RLS roles and filter expressions

Define and document the security role requirements to be implemented, and the members or groups of these roles. Use the bus matrix diagrams (Figure 3.1 and Figure 3.2) described in Chapter 3, Building a Power BI Semantic Model, to help communicate what data is currently stored in the model. Validate that role security is indeed required (not report or model filters), given the risk or sensitivity of the data.

Do not confuse security role filters with the various other forms of filters in Power BI, such as report, page, and visual-level filters, as well as filter logic in DAX measures. RLS role filters are applied to all queries of security role members, effectively producing a virtual subset of the semantic model for the given role at query time. Given the performance implications of compounding security role filters with report query filters, all user experience and analytical filters should be implemented outside of the security role filters. Security filters should be used exclusively to secure sensitive data.

#### How to define RLS roles and filter expressions

To implement this recipe, use the following steps:

- 1. In the Table view, create the following measures in the Calculations table:

```
Customers Row Count = COUNTROWS('Customers')
Internet Sales Row Count = COUNTROWS('Internet Sales')
Products Row Count = COUNTROWS('Products')
Reseller Sales Row Count = COUNTROWS('Reseller Sales')
Resellers Row Count = COUNTROWS('Resellers')
Sales Territories Row Count = COUNTROWS('Sales Territories')
Dates Row Count = COUNTROWS('Dates')
```

- 9. Click View as in the ribbon of the Modeling tab, choose the United States Online Bike Sales role, click the OK button, and note that the row counts in the table visualization change, with the Reseller Sales Row Count and Resellers Row Count measures being blank.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_359_0.png)

Figure 8.4: Viewing row counts as a member of the United States Online Bike Sales role

- 10. Click View as in the ribbon of the Modeling tab, choose the Europe Reseller Mountain and Touring role, uncheck the United States Online Bike Sales role, click the OK button, and note that the row counts in the table visualization change, with the Internet Sales Row Count and Customers Row Count measures blank.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_359_1.png)

Figure 8.5: Viewing row counts as a member of the Europe Reseller Mountain and Touring role

- 11. Save the file, click Publish in the ribbon of the Home tab, and deploy to the Power BI service.

- 12. Log in to the Power BI service and navigate to the workspace of the published report.

- 13. Click the ellipsis next to the semantic model and select Security.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_359_2.png)

Figure 8.6: Security settings for a published Power BI semantic model

### Designing Dynamic Security Models

```
PATHCONTAINS(Employees[ManagementPath],[Current User EmployeeKey])
```

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_364_0.png)

Figure 8.9: Dynamic filter expression applied to the Employees table for the security role Current User

- 8. Click the Save button.

- 9. Create a Multi-card visualization using the Current User Name, Current User Manager, Current User Org Level, and Employee Row Count measures as Fields.

- 10. Select View as in the ribbon of the Modeling tab and choose the Current User role:

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_364_1.png)

Figure 8.10: Viewing the report as A. Scott Wright

- 11. Edit the Current User Principal Name measure to peter0@adventure-works.com.

![](img/5e366dc9e90fe4b1ca9dd97c82976bbd_364_2.png)

Figure 8.11: Viewing the report as Peter J. Krebs

#### How it works

In a production environment, the Current User Principal Name measure would not be hardcoded to an email address but would, rather, take the form of the following formula:

```
Current User Principal Name = USERPRINCIPALNAME()
```
