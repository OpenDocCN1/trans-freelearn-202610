Ultimately, items are analogous to the rows and attributes are analogous to the columns found in traditional relational databases. The following screenshot illustrates this in more detail.

![](bbox=[0.145, 0.145, 0.87, 0.4])

Figure 6.16 – Data types in DynamoDB

Attributes can have the following data types in DynamoDB:

+   - **Scalar types** – Numbers, String, Boolean, Binary, and Null
+   - **Multi-valued types** – String Set, Number Set, and Binary Set
+   - **Document types** – List and Map

You can have document structures with multiple nested values, where you use maps to store JSON-style formatted documents in DynamoDB. You can also have lists as depicted in the preceding screenshot.

> **Note**
> The maximum size of an item can be up to 400 KB in size.

Next, let’s look at primary keys and their related components in more detail.