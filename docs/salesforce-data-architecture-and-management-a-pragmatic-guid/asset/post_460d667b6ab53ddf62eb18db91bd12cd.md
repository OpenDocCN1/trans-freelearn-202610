
Lookup skews can still happen even if multiple objects have a lookup to the same object; for example, objects x, y, and z each have a lookup to object a. If record ID ax1 on object a is the parent record for say 50,000 records on object x, 10,000 records on object y, and 20,000 records on object z, you will still have a lookup skew situation. Lookup skews can be avoided by employing some of the following methods:

**Improving record save performance** – This can be done by doing the following:

- a) Increasing the performance of your synchronous Apex code – tune your triggers for peak performance by consolidating code into a single trigger object.

- b) Removing unnecessary workflows, process builders, or flows or consolidating into existing trigger code. Keep in mind that these automations lengthen the lock duration, which can cause increased lock failures. Removing or consolidating workflows into Apex can improve performance.

- c) Processing only what's required synchronously and moving the rest of the code to asynchronous processing since locks are held for the full duration of the custom code execution.

**Distributing the skew** – Consider adding additional values to the lookup to avoid the skew or leaving it blank, if possible. This is because lookup issues are caused when many records have a lookup relationship to a single record. For example, a business provides quick budgetary quotes to walk-in customers without taking all of their details and creating an account in Salesforce. A generic account record has been created for this purpose. Over time, as quotes get created under this generic account, a lookup skew situation will arise that can then start to impact performance.

**Using a picklist field** – If there are a few lookup values, consider using a picklist value. By using a picklist value, you eliminate the locks associated with lookups and, hence, any locking issues.

As can be seen, something as innocuous as a lookup can lead to serious performance issues. This is a good reminder that we need to consider the consequences of our design and design for scalability and performance.

## List views and reports

List views and reports are usually the first places where users start noticing performance issues. This is because almost every user of the platform will either have reports, list views, or both, so they can focus on the work they need to do:

- **Limit the number of fields:** The first thing I recommend is to educate your users to use fields on both list views and reports that they truly need. On many occasions, when creating reports, users pull fields on them to get a feel for what data is available but never get rid of them when saving the report. There is much less value in having reports with data that won't be used by users, and not only that, but it also impacts performance as well. The best strategy that I find works is to not just tell users what to do when creating reports but explain the rationale behind why it's important that this is done and how it can impact them down the road.

Also, using relative dates, rather than exact date values, will result in better performance. For example, using **LAST WEEK** in your list view to filter all the leads from last week will be more performant, all things being equal, than entering **From** and **To** dates.

The use of **CONTAINS**, != is also discouraged as it prevents the Salesforce platform from using indexes and therefore negatively impacts performance. If you are unsure about how a report that you are creating for your users will perform, I sometimes find it helpful to write the query behind the report or list view and run it in the query plan. This can give you a sense of how your query will perform in the report or list.

- **Unused reports:** Salesforce provides the ability to schedule reports and dashboards so users can automatically get the report or dashboard emailed to them. Ensure that you do a frequent cleanup of reports and dashboards that are scheduled. In one instance, a shared mailbox used by admins had multiple reports emailed that had been requested as one-time requests from executives, but they were never deleted from the platform.

- **Governance**: Implement at a minimum some level of governance around reports. Report proliferation is a real thing and can get out of control really quickly if not dealt with properly. One way that I find useful is to create report folders for each business department and give users in the department full access to the folder. Alternatively, you can designate 1-2 power users and give them **Manager** access, with the remaining users having **Editor** access to the folder.

## SOQL best practices

Every time we press a Visualforce page that shows us some data, runs a report, or runs an interface to update data using Data Loader, chances are that we are querying some data from the database and running a query behind the scenes, whether we realize it or not. Acknowledging that some of the data is also cached for performance reasons, but assuming we run the aforementioned operations first time, a query will be running behind the scenes.

Because the Salesforce platform is multi-tenant and customers don't get access to the database directly, when you run a report, SOQL, and list view, your request is sent to what's called a **Query Optimizer**. This is a layer above the database that then converts your SOQL into a SQL query that the database can understand. The Query Optimizer understands *sharing rules*, including *with sharing*, in Apex classes as these have a direct impact on the results of a SOQL statement or when a report is run. The Query Optimizer is designed to optimize the SOQL into SQL for the database to run. *Optimize here refers to whether a full table scan or index will be performed as well as the order in which the joins will be performed. Some of the practices mentioned in the List views and reports section are applicable to SOQL queries as well and so I won't be repeating them here.*

Let's review some of the best practices related to running SOQL queries:

- **Using indexes**: This is very useful for ensuring that your queries will run efficiently. There are two types of indexes in Salesforce – standard and custom:

Standard indexes include fields such as RecordId, fields marked as external IDs, Name, LastModifiedDate, CreatedDate, RecordTypeId, Division, Email (for Contact and Lead objects only), lookup, and master-detail relationships. You can ask Salesforce Support to create custom indexes on formula fields and fields containing nulls. **Formula** fields calculate values on the fly and, by default, they don't have indexes, which typically means that a table scan would be required. If you decided to create custom **Indexes** on **Formula** fields, the function that defines the **Formula** field must be deterministic. Examples of non-deterministic **Formula** fields include the following:

- They contain a reference to another field, for example, lookup.
- They reference another **Formula** field.
- They use dynamic dates and time functions, for example, NOW, TODAY, and others.
- They have owner, auto number, division, and audit fields (excluding **CreatedDate** and **CreatedByID**).
- They reference other fields that cannot be indexed, for example, multi-select picklists, currency, long text area fields, and binary fields, including encrypted fields.

Standard fields with special functions, for example, on the **Opportunity** object, the **Amount** and **TotalOpportunityQuantity** fields.

Null fields are not indexed by default and, therefore, a null field would require a full table scan. A two-column index, including a null field, can be created on request to Salesforce. In this case, the second column can be null, but not the first column.

## Divisions

This feature facilitates the partitioning of data, thereby improving query performance. You could have Salesforce enable this functionality and create divisions based on regions, but other values are also allowed. Each record then has a value for the **Division** field. For example, *division* values can be based on regions such as North America, Europe, Australia, and the like.

If you have the **Affected by Division** permission, then your search results, list views, and other functionality in Salesforce provide the option to select the division that you want to return results from. For example, you can only show records specific to a division if, at the time of creating or editing the list view, you specify the division you are interested in. Similarly, you can specify the division in report options to return results from a specific division, otherwise records from all divisions will be shown.
