IDs. Thinking purely in terms of performance, the Bulk API in parallel mode is the fastest way to load millions of records into Salesforce in the shortest amount of time. An external ID field can be used to avoid duplicates while importing records, as the external system ID is used to identify records. This can be particularly useful when performing, for example, scripted data loader tasks as the external ID can be used as the key for associating parent and child records.

Next, think about the operation that will be used to create and update data. Salesforce provides **insert**, **update**, and **upsert** operations. Insert is used for data creation, update is used for updating existing records, and upsert is used for updating existing records, or creating a new record if no record can be found to update. As you can imagine, an upsert operation is costlier to perform in terms of Salesforce resources as the calculation is done on the fly for every record when determining the operation to perform. Therefore, the best practice is to use the insert and update operations when migrating LDVs to the Salesforce Platform as they are faster than upsert.

Lastly, it is important to keep in mind that data storage on Salesforce is finite and generally linked to the number of user licenses purchased. If you keep loading millions upon millions of records into Salesforce, eventually, your data storage will run out. However, the limits are quite generous when a Salesforce environment is provisioned. Typically, data storage is set at 10 GB (for Salesforce orgs provisioned from late March 2019) plus either 20 MB or 120 MB per licensed user, depending on the Salesforce edition purchased (so a 20-user org on Salesforce Professional Edition will have 10.4 GB of data storage).

**Important note**
More details on data allocation and storage limits can be found at https://help.salesforce.com/s/articleView?id=sf.overview_storage.htm&type=5.

This leads us nicely to the next section, where we’ll look at exporting data from Salesforce.

# Exporting data from Salesforce

As much as we may want to maximize the investment our clients make in Salesforce by moving data into it, we should consider the requirements surrounding exporting data from Salesforce. This may involve exporting certain data (for example, to a data warehouse platform), which typically involves an automated process being performed at a scheduled interval (such as every night). As expected, several techniques can be used to accomplish this task.

# Data Export

Salesforce offers two methods for exporting data out of the box. The first is the Data Export service, which is accessible from within the Salesforce setup. Data Export can be used to manually export data weekly or monthly (depending on the edition of Salesforce you have), as well as to schedule regular data exports weekly (every 7 days) or monthly (every 29 days).