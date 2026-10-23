
## Custom indexes

To speed up query performance, Salesforce supports the creation of **custom indexes**. Think of an index as a way of sorting records in an order that's based on the parameters that form the index. By using a sorted index, record retrieval speed can be increased. Custom indexes are enabled by contacting **Salesforce Support**. Before considering doing so, it's worth touching on which fields are automatically indexed by the Salesforce platform for most objects:

- `RecordTypeId`
- `Division`
- `CreatedDate`
- `SystemModstamp (LastModifiedDate)`
- `Name`
- `Email (for Contact and Lead records)`
- `Foreign key relationships (Lookup and Master-Detail)`
- `The (unique) Salesforce record ID, which is the primary key for each object`

> **Important note**
> Custom indexes can be created on almost all types of fields, except for multi-select picklists, text areas (long or rich), non-deterministic formula fields, and encrypted text fields. External ID field types cause an index to be created on that field by default, but only Auto Number, Email, Number, and Text types are supported for the creation of external ID field types.

Salesforce's underlying database architecture makes the actual data table for custom fields unsuitable for traditional database indexing (because we're essentially talking about a large single physical table that contains all the fields for the objects in your Salesforce instance). To overcome this indexing limitation, Salesforce creates a separate index table (for each Salesforce infrastructure node running the Salesforce application) that essentially contains a copy of the object field data in that node, along with information about the data types of the object fields. Salesforce then can (through its application layer) build and maintain a traditional database index on this separate index table. The index table places upper limits on the number of records that an indexed search can effectively return, as only those indexed records are considered to be returned to the process or the user issuing the query before defaulting to a much wider (and therefore less performant) non-indexed search.

By default, the index tables do not include empty records (sometimes known as null records, although Salesforce support can enable custom indexes that contain null records if desired). If custom indexes are created on custom fields, then you must explicitly enable and rebuild the indexes to get the empty-value rows included in the index.

It is also worth knowing that the Salesforce query optimizer (a feature of the Salesforce application layer) maintains a separate database table that contains statistics about the distribution of object and field data in each index. Salesforce’s query optimizer uses this table to perform pre-queries when an automated process, user, or report issues a query to determine whether using an index can speed up the query (and therefore increase performance).

## Standard indexed fields

Salesforce’s standard indexed fields are used when a SOQL query filter matches less than 30% of the first million returned records in the database and less than 15% of additional records in the database, up to a total of 1 million records.

For example, a standard index is used in the following cases:

- When a SOQL query is issued against an object table with 2 million records, and the filter matches roughly 450,000 or fewer records (as 30% of 1 million is roughly 300,000 and 15% of the remaining 1 million is roughly 150,000)
- When a SOQL query is issued against an object table with 5 million records, and the filter matches roughly 900,000 or fewer records (as 30% of 1 million is roughly 300,000 and 15% of the remaining 4 million is roughly 600,000)

With standard indexed fields covered, let’s look at custom indexed fields and when they are used.

## Custom indexed fields

Unlike standard indexes, custom indexes are used when a SOQL query filter matches less than 10% of the total records, up to a maximum of 333,333 records.

For example, a custom index is used in the following cases:

- When a SOQL query is executed against an object table with 500,000 records, and the filter matches 50,000 or fewer records (as 10% of 500,000 is 50,000)
- When a SOQL query is executed against an object table with 5 million records, and the filter matches 333,333 or fewer records (as 10% of 5 million is 500,000, yet the upper limit is 333,333 records so the value of 333,333 is used)

For more information about some of the intricacies concerning indexes, see https://developer.salesforce.com/docs/atlas.en-us.salesforce_large_data_volumes_bp.meta/salesforce_large_data_volumes_bp/ldv_deployments_infrastructure_indexes.htm.

Now that you have understood indexing, let’s focus on a solution that helps us avoid joins in tables for optimizing read-based operations.

## Skinny tables

To speed up the performance of certain read-only operations on a single object, **skinny tables** can be created to avoid joins across the standard and custom fields. When skinny tables are enabled, they are used automatically where appropriate. You can't create, access, or modify skinny tables directly since this is done by Salesforce Support. In essence, you work with them to define what the skinny table should contain when optimizing read-only queries to, say, accounts for specific fields. If the report, list view, or query you're optimizing (and therefore getting the skinny table created for) changes (such as adding a new field to respond to business users needing an extra value in a report), you'll need to contact Salesforce to update your skinny table definition.

It's also worth noting that skinny tables don't contain soft-deleted records, so they won't be available when queries are using the skinny table over the broader object data in the standard underlying tables.

Read-only operations that reference only those fields defined in a skinny table won't require an extra join across the standard and custom object field tables, and therefore they can perform better. According to Salesforce (at https://developer.salesforce.com/docs/atlas.en-us.salesforce_large_data_volumes_bp.meta/salesforce_large_data_volumes_bp/ldv_deployments_infrastructure_skinny_tables.htm), the use of skinny tables is best employed with Salesforce objects containing millions of data records to improve the performance of read-only operations on those records, such as executing reports and dashboards or list views.

Skinny tables can be created on all custom objects and the following standard objects:

- Account
- Contact
- Opportunity
- Lead
- Case

Some other considerations apply to skinny tables. For example, skinny tables can contain a maximum of 100 columns, and they cannot contain fields from other objects. Think of them as optimized tables for read-only operations for a single object.

Let’s look at a visual example of a skinny table:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_112_0.png)

Figure 6.1 – Example of a skinny table

They are created to remove joins across standard and custom fields when issuing read-only queries to read data from a single object.

## Selective filter conditions

**Selective filter conditions** can be used in your **Salesforce Object Query Language (SOQL)** queries to improve performance. Within the Salesforce application logic, the Salesforce platform’s query optimizer feature makes this decision and determines the filter condition selectivity for an issued SOQL query (including those required for reports and list views) when queries are run.

When dealing with relatively simple SOQL queries, you can easily determine whether a specific filter condition is selective by gathering statistics on it using several tools, but the humble **Developer Console Query Editor** will do just fine. After you apply an index to a selective field, queries with related filter conditions will perform more efficiently.

By confirming that filter conditions in reports, SOQL, and list views are selective, then by applying indexes to those fields, the Lightning Platform query optimizer doesn’t have to perform full dataset scans when looking for target data. This means that the performance will be optimal in such use cases.

## Divisions

**Divisions** can be used to partition data and reduce the number of records returned by SOQL queries, therefore increasing performance for the user or process calling the query. Salesforce provides special support for partitioning data by divisions (which can be thought of as logical sections of data), but this needs to be specifically enabled by contacting Salesforce Customer Support.
