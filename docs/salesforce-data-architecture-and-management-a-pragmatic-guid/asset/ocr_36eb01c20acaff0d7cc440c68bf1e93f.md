It's important to note that skinny tables can be used for read-only operations and can be created on both standard and custom objects. There are certain limitations on what types of fields can be created; for example, neither **Roll-Up Summary** fields nor **Formula** fields are supported. The customer cannot create skinny tables themselves, but rather they have to open a case with Salesforce Customer Support to get them created.

There are some limits for skinny tables that must be kept in mind when considering them as a solution. One of the major limitations is that you cannot use them across different objects, that is, each skinny table can only have data from a single object. For example, a skinny table cannot be created for the `Opportunity` object that also has data from the `Opportunity Team Member` object.

You can have up to 100 fields in a skinny table and any time a new field is added to the object, you will need to contact Salesforce to get it added to the skinny table. They also support encrypted fields and encrypted indexes. Skinny tables are copied over to full-copy sandboxes at the time of sandbox creation or refresh, but for other types of sandboxes, a request must be submitted to Salesforce Customer Support.

# # Query filters, indexes, and negative operators

Ensure that you are using filters in your SOQL query as much as possible as that helps the Optimizer narrow down the records when certain conditions are met. For example, this developer org has 39 `Account` records, so let's try to query all the records and view the query cost. As seen in Figure 6.22, the query cost with no filters is greater than 1, which we know is inefficient:

![](bbox=[0.37, 0.57, 0.86, 0.8])

Figure 6.22 – The cost is a high 2.78, which is very inefficient for a query