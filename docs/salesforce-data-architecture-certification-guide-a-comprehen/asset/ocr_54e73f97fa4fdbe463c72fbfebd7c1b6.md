When issuing a form of query to the Salesforce database for data, such as Salesforce Object Query Language (SOQL) or a REST API call, that request is turned into system-generated SQL for the underlying database. It is not possible to tune the SQL that is generated but being mindful of how our data queries will affect the generation of the SQL provides us with something that we can affect to a certain extent. It is within these parameters that we can optimize our data queries.

For example, consider this SOQL query for the first 10 account record names as they would appear in an alphabetically sorted list:

```
SELECT Name FROM Account ORDER BY Name ASC LIMIT 10
```

Given that we’re querying a standard field from a standard object, Salesforce will only need to retrieve data from the standard objects/standard fields table for Account.

When adding a custom field into the mix (which we’ll assume is unimaginatively called CustomField for the purposes of this explanation), we’re then asking the underlying database query to have a JOIN in order to allow data from the Standard Object/Custom Fields table to be retrieved. Therefore, a query as simple as this will cause data to be retrieved from more than one underlying database table:

```
SELECT Name, CustomField__c FROM Account ORDER BY Name ASC LIMIT 10;
```

Now consider a third scenario. We are going to query a custom object (called CustomObj__c) that has a lookup to an account (called Account__c), and also place the name of the account and the custom account field from the last example in the results:

```
SELECT Name, Account__c.Name, Account__c.CustomField__c FROM CustomObj__c ORDER BY Name ASC LIMIT 10;
```

We’ve now queried the Custom Object/Custom Fields, Standard Object/Custom Fields, and Standard Object/Standard Fields tables, all for the retrieval of 10 rows worth of data from 3 fields!

It is therefore best practice to design our data access queries to avoid joins as much as possible, therefore ensuring we get the best performance possible. Given the way in which the underlying Salesforce data is structured, having a well-structured data model (avoiding joins as much as possible) means reporting, dashboards, and list views will perform well.

Chapter 6, Understanding Large Data Volumes, describes the issues that can arise with large amounts of data (particularly with performance) and how to mitigate them.

With the Salesforce data model now introduced, let’s turn our attention to sharing and security, which affect how data is accessed.