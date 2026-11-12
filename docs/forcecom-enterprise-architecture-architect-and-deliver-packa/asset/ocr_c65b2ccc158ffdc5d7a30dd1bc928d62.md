The following diagram illustrates where Apex classes implementing the Selector layer fit in terms of its interaction and reuse within the Service and Domain layers as well as other execution contexts such as **Batch Apex**. For the most part, it is a layer that exists conceptually below the Service layer (under the Service boundary), though it can be reused from execution contexts that perform database queries, such as Batch Apex and Controllers.

![](bbox=[0.145, 0.185, 0.795, 0.545])

# # Implementing design guidelines

The methods in the Selector classes encapsulate common SOQL queries made by the application, such as `selectById`, as well as more business-related methods, such as `selectByTeam`. This helps the developers who consume the Selector classes identify the correct methods to use for the business requirement and avoids replication of SOQL queries throughout the application.

Each method also has some standard characteristics, such as the `SObject` fields selected by the queries executed, regardless of the method called. The overall aim is to allow the caller to focus on the record data returned and not how it was read from the database.