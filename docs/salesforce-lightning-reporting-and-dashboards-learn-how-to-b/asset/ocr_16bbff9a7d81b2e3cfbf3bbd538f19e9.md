A limited number of custom objects can be created depending on the Salesforce edition you subscribe to. If you have admin permission, you can create objects and fields in Salesforce with just Point and Click rather than complex SQL scripts as in the traditional database.

In a standard object, the standard fields available depend on the object, but each custom object comes with the following few standard fields, which is the same for all custom objects:

+   - ID
+   - Name
+   - Created By and Created Date
+   - Last Modified By and Last Modified Date
+   - Owner (if the object is not a child of other objects in Master-Detail Relationship)

Just as with Standard objects, you can create custom fields in a custom object.

![](bbox=[0.12, 0.42, 0.2, 0.52])

Use Standard object as it is designed for, for example, Salesforce CRM provides standard Account and Contact object, so do not create a custom field, such as a contact email address in Account.

# Object relationships

You can relate an object to other objects in Salesforce. For example, relating a custom object Expense to custom object Project. With this relation, you'll know which expense record is used for which project. Project, in this example, will be considered as the parent object, and Expense as a child object. From the record-level perspective, one parent can have many children, while the child can only have one parent. To build this relationship from the child object, create a Lookup Relationship or Master-Detail Relationship field to the parent object.

There are three types of object relationships in Salesforce, which are as follows:

+   - Master-Detail Relationship
+   - Lookup Relationship
+   - Hierarchical Relationship