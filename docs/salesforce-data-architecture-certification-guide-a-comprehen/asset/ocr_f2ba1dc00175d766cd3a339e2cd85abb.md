In the next section, we'll take a look at the representation of a golden record when middleware holds the record associations.

# Middleware holding the golden record associations

Taking our example from the previous section, if the middleware (such as MuleSoft) holds golden record associations, we can essentially have Salesforce only care about itself in terms of record integrity, with the onus being passed to the middleware to ensure that data operations to or from associated systems are correct. This does reduce the initial querying of Salesforce to determine those associated system IDs, but an additional process needs to be put in place to ensure that the middleware is updated appropriately if an associated system is added to, or removed from, the IT enterprise.

The following diagram shows an example of how this may be sequenced when middleware holds the record associations:

![](bbox=[0.12, 0.38, 0.88, 0.68])

+   1. Invocation of data update from Salesforce
+   2. Middleware queries for system identifiers associated with Salesforce record from internal database
+   3. Data updates made to systems associated with Salesforce record

Figure 3.3 – Middleware holding the golden record associations

Next, let's look at the representation of a golden record when a separate system or external registry holds the record associations.