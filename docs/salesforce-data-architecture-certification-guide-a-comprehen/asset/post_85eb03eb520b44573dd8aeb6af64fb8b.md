
6. For how long is Field Audit Trail data available when Salesforce Shield is enabled in a Salesforce instance? Pick one answer:

+   A. 5 years
+   B. 7.5 years
+   C. 10 years
+   D. 12.5 years

7. Which three features are provided by Salesforce for preserving the traceability of changes made to a Salesforce instance without additional product subscription? Pick three answers:

+   A. Setup Audit Trail
+   B. Weekly Report Exports
+   C. Field History Tracking
+   D. Event Monitoring
+   E. Field Audit Trail

8. By default, Salesforce Connect supports which protocols? Pick two answers:

+   A. OData 2.0
+   B. SOAP
+   C. OData 4.0
+   D. OAuth

9. *Universal Containers* have two separate Salesforce instances, and they wish to connect them to show data from one instance in the other. Which technology can be used to quickly achieve this requirement? Pick one answer:

+   A. Middleware, such as MuleSoft
+   B. Salesforce-to-Salesforce connector
+   C. The Salesforce REST API
+   D. Custom Apex logic

10. Which of the following Salesforce products require connectors to read/write data to/from the core Salesforce platform? Pick three answers:
A. Marketing Cloud
B. Pardot
C. B2B Commerce
D. Service Cloud
E. Experience Cloud

11. Which of the following statements is true about Task record-locking behavior? Pick one answer:
A. When a task is deleted, the Account record, along with the records referenced by the WhoId and WhatId fields, is not locked
B. When a task is deleted, the Account record, along with the records referenced by the WhoId and WhatId fields, is locked, irrespective of Task record values
C. When a task is deleted, only the records referenced by the WhoId and WhatId fields are locked, irrespective of Task record values
D. When a task is deleted, the Account record, along with the records referenced by the WhoId (but not the WhatId) field is locked, irrespective of Task record values

12. Universal Containers have an Experience Cloud site that is available to external users. They wish for all users to have access to Events, with a subset of users having edit access to Events. Which license types are necessary to fulfill this requirement? Pick two answers:
A. Customer Community
B. Partner Community
C. Customer Community Plus

13. Universal Containers have implemented Enterprise Territory Management. Some of the territories need to be reassigned due to a change in the territory structure. Which is the best-practice procedure to follow to keep recalculations, and therefore performance impacts, to a minimum? Pick one answer:
A. Start with the lowest-level territory in a single branch first, and then work your way up the hierarchy
B. Start with the highest-level territory in a branch first, and then work your way down the hierarchy
C. Start with the lowest-level territories in the hierarchy first, and then work your way up the hierarchy
D. Start with the highest-level territories in the hierarchy first, and then work your way down the hierarchy

14. True or false? Applying inherited rules to child territories in Enterprise Territory Management improves performance. Pick one answer:

+   A. True
+   B. False

15. Universal Containers have filter criteria on some of their Enterprise Territory Management rules. Which are considerations for optimal performance of rule criteria? Pick two answers:

+   A. Define criteria on numeric fields only
+   B. Do not define criteria on numeric fields as text literals
+   C. Define criteria on string fields only
+   D. Only define criteria on numeric fields as text literals

16. Universal Containers wish to backup data from their Salesforce instance locally. Which options are available immediately (without incurring additional fees) to facilitate this requirement? Pick three answers:

+   A. Data Export Service
+   B. Salesforce Backup and Restore
+   C. Report Export
+   D. Data Loader
+   E. List Views

17. Universal Containers wish to backup metadata from their production Salesforce instance. Which options are available immediately (without incurring additional fees) to facilitate this requirement? Pick three answers:

+   A. Change Sets
+   B. Sandbox Refresh to create a sandbox from production
+   C. Force.com Migration Tool
+   D. Salesforce Metadata Export
+   E. Salesforce Org Backup

18. Universal Containers are both loading in and updating millions of records in their Salesforce instance. What are considered best practices for mitigating any performance impacts during this activity? Pick two answers:

+   A. Deferring sharing calculations
+   B. Disabling any automation, such as triggers and workflow rules
+   C. Enabling automation, such as triggers and workflow rules
+   D. Enabling sharing calculations

19. Which of the following features is Salesforce support required to enable? Pick three answers:

+   A. Big Objects
+   B. Skinny Tables
+   C. Divisions
+   D. Custom Indexes
+   E. Selective Filter Conditions

20. Why do skinny tables speed up performance for objects containing millions of rows of data? Pick three answers:

+   A. They avoid a join between standard and custom fields in the Salesforce database
+   B. They do not contain soft-deleted records
+   C. They do not contain all the fields of the object
+   D. They only contain indexed fields
+   E. Other fields of the object are discarded when queries are run

## Summary

In this chapter, we’ve covered a plethora of different, exam-like questions in order to test your knowledge of the content covered in the exam. Check your answers in Chapter 17, Answers to Practice Questions.

In Chapter 14, Cheat Sheets, we’ll introduce some revision aids.

Next, let’s look at Many-to-Many relationships:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_212_0.png)

Figure 14.2 – Modeling Many-to-Many relationships

Next up is the sharing model:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_212_1.png)

Figure 14.3 – The Sharing model

Finally, we can retouch upon object and field storage:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_213_0.png)

Figure 14.4 – Object and field storage

Now let’s move on to master data management.

## Master data management

Let’s now look at the diagrams we introduced on the topic of Master Data Management (MDM).

The following can be used as an aid for the topic of master data management, where we explored an MDM scenario with Salesforce as the keeper for golden record associations:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_213_1.png)

Figure 14.5 – Salesforce holding the golden record associations

Figure 14.7 – External registry holding the golden record associations

1 Invocation of data update from Salesforce
2 Middleware queries for system identifiers associated with Salesforce record from external registry
3 Data updates made to systems associated with Salesforce record

Let’s continue with Salesforce data management.

## Salesforce data management

Let’s now look at Salesforce data management revision aids.

Introduced earlier in this book in Chapter 4, Salesforce Data Management, let’s recap the licensing for Experience Cloud so that you can be best equipped when answering questions on Experience Cloud and licensing options:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_216_0.png)

Figure 14.8 – Salesforce Experience Cloud licensing

Let’s now move on to data governance.

## Data governance

In *Chapter 5, Data Governance*, we introduced the concept of an enterprise data governance strategy.

Let’s recap the enterprise data governance strategy:

![](img/7cca8452e237cf62a2d0e00a080d3fe9_217_0.png)

Figure 14.9 – Enterprise data governance strategy

Next up, let’s move on to large data volumes.

## Large data volumes

Chapter 6, Understanding Large Data Volumes, saw us go in depth into Large Data Volumes (LDVs) in Salesforce, including techniques for mitigation.
