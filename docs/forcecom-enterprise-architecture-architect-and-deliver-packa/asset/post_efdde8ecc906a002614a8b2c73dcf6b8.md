
Formula fields can be indexed so long as the formula result is deterministic, which means that it does not use any dynamic data or time functions such as TODAY or NOW, or leverage-related fields via lookup fields or any other formula fields that are non-deterministic.

As creating indexes and maintaining them consumes platform resources, custom indexes cannot be packaged, as the decision to enable them is determined by conditions only found in the subscriber org.

Though objects can contain millions of records, standard indexes will only index up to 1 million records and custom indexes up to 333,333 records. These limits have a bearing on whether an index is used or not, depending on the query and filter criteria being used. This is discussed in more detail further in this chapter.

With respect to the Race Data object, the following fields will be indexed by default by the platform. Only through contacting Salesforce support and enabling in the subscriber org can additional custom indexes be created, for example, on the Type__c field.

| Field | Standard Index |
|---|---|
| Created By | Yes (Lookup Field) |
| LastModifiedBy | Yes (Lookup Field) |
| Name | Yes |
| Owner | Yes |
| Contestant__c | Yes (Lookup Field) |
| Race__c | Yes (Lookup Field) |
| DriverId__c | No |
| Lap__c | No |
| RaceName__c | No |
| Sector__c | No |
| Type__c | No |
| Value__c | No |
| Year__c | No |

Under Setup, navigate to Objects and locate the Race Data object. When viewing fields listed in the Standard Fields or Custom Fields & Relationships related lists, observe the Index column. This is the easiest way to determine whether a field has a standard or custom index applied to it.

Thus, if a query filters by fields covered by a system or custom index, the index will be considered by the Force.com query optimizer. This means that just because an index exists does not mean it will be used. The final decision is determined by how selective the query is.

> By default, Salesforce does not include rows (that contain null values) within its indexes for the respective field. In this case, although the **Contestant** field is indexed by default, it initially contains a null value and thus initially the index will be ineffective (this fact relates to why the use of nulls in filter criteria will deter the Force.com query optimizer from using an index, as discussed in the next section). You can, however, get Salesforce Support to discuss enablement of the inclusion of nulls in indexes.

## Ensuring queries leverage indexes

The process of selecting an appropriate index applies to queries made via SOQL through APIs and Apex, but also via **Reporting and List Views**. The use of an index is determined by the SOQL statement being selective or not. The following provides definitions of being selective and being non selective:

- Salesforce uses the term **selective** to describe queries filtering record data that are able to leverage indexes maintained behind the scenes by Salesforce, making such queries more efficient and thus returning results more quickly.

- Conversely, the term **non selective** implies a given query is not leveraging an index; thus, it will, in most cases, not perform as well. It will either time-out, or for objects with records greater than 100,000, result in a runtime exception.

In a nonselective query situation, the platform will scan all the records in the object, applying the filtering to each; this is known as a **full table scan**. However, this is not necessarily the slowest option, depending on the number of records and the filter criteria.

The main goal of this part of the chapter is to explain what it takes for Salesforce to select an index for a given query. This depends on a number of factors, both when you build your application queries and the number of records in your objects within the subscriber org.

## Factors affecting the use of indexes

There are two things that determine whether an index is used. Firstly, the **filter criteria** (or the `WHERE` clause in SOQL terms) of the query must comply with certain rules in terms of operators and conditions used. Secondly, the **total number of records** in the object versus the estimated number of records that will be returned by applying the filter criteria.

The filter criteria must, of course, reference a field that has either a standard or custom index defined for it. After this, unary, AND, OR, and LIKE operators can be used to filter records. However, other operators, such as negative operators, wild cards, and text comparison operators, as well as the use of null in the filter criteria, can easily disqualify the query from using an index. For example, a filter criteria that is more explicit in the rows it wants, `Type__c = 'Sector Time'`, is more likely to leverage an index (assuming that this field has a custom index enabled), than say, for example, `Type__c != 'Pit Stop Time'`, which will cause the platform to perform a full table scan, as the platform cannot leverage the internal statistics it maintains on the spread of data throughout the rows across this column.

The filtered record count must be below a certain percentage of the overall records in the object for an index to be used. You might well ask how Salesforce knows this information without actually performing the query in the first place? Though they do not give full details, they do describe what is known as a pre-query, using statistical data that the platform captures as the standard or custom indexes are maintained. This statistical data helps to understand the spread of data based on the field values. For example, the percentage of rows loaded by the execution of the scripts to load sample data where the `Type__c` field contains the **PitStop Time** value is 0.16 percent, and thus, if a custom index was in place and the queries filter criteria was selective enough (for example, `Type__c = 'PitStop Time'`), an index would be used.

A query for **Sector Time**, such as `Type__c = 'Sector Time'`, will result in 1664 records, which is 16.64 percent of the total rows being selected. So, while the filter criteria are selective, the filtered record count is above the threshold for custom index: 10 percent or 333,333 records max. Thus, in this case, an index is not used. This might result in a timeout or runtime exception. So, in order to avoid this, the query must be made more selective, for example, by enabling a custom index and adding either `Lap__c` or `Sector__c` into the filter criteria.

Salesforce provides more detailed information on how to handle large data volumes, such as fully documenting the filter record count tolerances used to select indexes (note that they differ based on standard or custom indexes) and some insights as to how they store data within the Oracle database that help make more sense of things. These are a must-read in my view and can be found in various white papers and Wiki pages. Some of them are as follows:

- Query & Search Optimization Cheat Sheet: http://resources.docs.salesforce.com/rel1/doc/en-us/static/pdf/salesforce_query_search_optimization_developer_cheatsheet.pdf
- Best Practices for Deployments with Large Data Volumes: http://www.salesforce.com/docs/en/cce/ldv_deployments/salesforce_large_data_volumes_bp.pdf
- Force.com SOQL Best Practices: Nulls and Formula Fields: https://developer.salesforce.com/blogs/engineering/2013/02/force-com-soql-best-practices-nulls-and-formula-fields.html
- Developing Selective Force.com Queries through the Query Resource Feedback Parameter Pilot: https://developer.salesforce.com/blogs/engineering/2014/04/developing-selective-force-com-queries-query-resource-feedback-parameter-pilot.html

## Profiling queries

As you can see, consideration for which indexes are needed and when they are used are very much determined by the rate of data growth in the subscriber org and also the spread of distinct groups of data values in that data. So, profiling is typically something that is only done in conjunction with your customers and Salesforce, within the subscriber org.

In some cases, disclosing the queries your application makes from Apex and Visualforce pages might help you and your subscribers plan ahead as to which indexes are going to be needed. You can also capture the SOQL queries made from your code through the debug logs, once you're logged in through the **Subscriber Support** feature.
