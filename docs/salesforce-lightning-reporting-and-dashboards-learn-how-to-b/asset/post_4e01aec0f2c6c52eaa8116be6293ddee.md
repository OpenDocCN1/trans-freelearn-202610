
Salesforce provides a tool to help you check your organization's readiness for Lightning Experience. Click on the **Evaluate** link under **Check Your Lightning Experience Readiness** from the Lightning Experience setup menu. Salesforce will email a readiness report to you.

## Switch to Lightning Experience

Once Lightning Experience is enabled and permission assigned, click on **Switch to Lightning Experience** after clicking on your name, as seen in the following screenshot:

![](img/72d01bea5a9ccf02f9452aab7505d9de_30_0.png)

As of now, not all features from Classic are available in Lightning Experience; you may need to switch back to Classic when needed. For example, Recycle Bin is not available in Lightning Experience. To switch back to Classic, click on your photo, then click on Switch to Salesforce Classic. You can use the same path to switch back to Lightning Experience:

![](img/72d01bea5a9ccf02f9452aab7505d9de_32_0.png)

The Summer '17 release offers the ability for admin to remove the switcher for a user to go back to Classic. If you are not the system admin, reach out to your system admin, but if you are the system admin, there is a new permission that controls this-- it is called Hide Option to Switch to Salesforce Classic in Profile.

## The Salesforce object model

No matter what the user interface, Classic or Lightning Experience, the backend database is the same. You will see the same data in Classic or in Lightning Experience, or even from the Salesforce1 Mobile App. All reports and dashboards which can be built are based on the object model implemented in your Salesforce organization, from objects relationship, down to the field type level. It's crucial for you to know this as a basis, before learning to build advanced reports and dashboards.

This section will discuss the object model in Salesforce. Objects are a key component in Salesforce. They allow you to store your data. Similar to a table in the database, an object comprises several fields to store data. You can set some fields as mandatory, while some other fields will be automatically populated by the system, such as **ID**, **Created Date**, **Created By**, **Last Modified Date**, and **Last Modified By**.

You can illustrate an object as a table, field as a column in the table, and record as a row in the table. In the following table, field 1 will store values for all first names, field 2 for birth date, and so on:

| records | field 1 | field 2 | field 3 |
|---|---|---|---|
| record 1 | John | 29 Jun | ... |
| record 2 | May | 10 Dec | ... |
| record 3 | Steve | 24 Feb | ... |

There are two types of objects in Salesforce:

- Standard objects
- Custom objects

### Standard objects

Standard objects are provided by default when you subscribe for Salesforce; this is dependent on the cloud type you subscribe to. Each object has its own uniqueness for specific functions, and the objects have built-in relationships to each other. Here are a few main standard objects when you subscribe to Sales and Service Cloud:

- **Account**: Account is used to store information about the businesses and organizations your company interacts with
- **Contact**: Contacts store information about the people that you work with--prospects, customers, or suppliers
- **Opportunity**: Opportunity is used to store information about sales interactions with your customers; this is often known as the **Sales Cycle**
- **Lead**: Lead is to store information about people who might become customers or partners of your company
- **Case**: Case stores information about interactions with your customers related to the products or services you provide
- **Campaign**: Campaign stores information about your company's marketing activities and responses

Some other standard objects are Activity, Asset, Contract, Quote, Order, Products, and Price Book.

Each standard object comes with default fields based on the purpose of the object, for example, **Stage in Opportunity**, **Mobile Phone in Contact**, and so on. You can create your own fields in the Standard object; this is called a **custom field**. The maximum number of fields you can create depends on the Salesforce edition you subscribe to.

You can upgrade your Salesforce edition to a higher edition, such as Professional Edition to Enterprise Edition, by simply contacting your Account Executive and paying for the increased subscription fee. You will continue using the same organization with your existing database and customization. But if you plan to downgrade to a lower edition, it is actually not possible to downgrade. By the end of your contract, Salesforce will give you a brand new organization where you need to reconfigure and transfer all your data.

### Custom objects

Custom objects are objects created within an organization to store data specific to that organization's business, and which cannot be stored using standard objects. Only users with admin access can create a custom object. Most AppExchange packages create and use custom objects, since they provide specific business processes. Just like standard objects, custom objects are used to store specific data in Salesforce.

Do not create custom objects to replace the functionality offered by standard objects. For example, if you create a custom Lead object, you will waste all the functionalities offered by Lead object, such as Lead Conversion.

A limited number of custom objects can be created depending on the Salesforce edition you subscribe to. If you have admin permission, you can create objects and fields in Salesforce with just Point and Click rather than complex SQL scripts as in the traditional database.

In a standard object, the standard fields available depend on the object, but each custom object comes with the following few standard fields, which is the same for all custom objects:

- ID
- Name
- Created By and Created Date
- Last Modified By and Last Modified Date
- Owner (if the object is not a child of other objects in Master-Detail Relationship)

Just as with Standard objects, you can create custom fields in a custom object.

![](img/72d01bea5a9ccf02f9452aab7505d9de_35_0.png)

Use Standard object as it is designed for, for example, Salesforce CRM provides standard Account and Contact object, so do not create a custom field, such as a contact email address in Account.

### Object relationships

You can relate an object to other objects in Salesforce. For example, relating a custom object Expense to custom object Project. With this relation, you'll know which expense record is used for which project. Project, in this example, will be considered as the parent object, and Expense as a child object. From the record-level perspective, one parent can have many children, while the child can only have one parent. To build this relationship from the child object, create a Lookup Relationship or Master-Detail Relationship field to the parent object.

There are three types of object relationships in Salesforce, which are as follows:

- Master-Detail Relationship
- Lookup Relationship
- Hierarchical Relationship

The following table compares the Master-Detail Relationship and Lookup Relationship:

| Master-Detail Relationship | Lookup Relationship |
|---|---|
| You can define master-detail relationships between two custom objects, or between a custom object with a standard object (the standard object must be the parent). | You can define the relationship between any two objects, standard, or custom object. |
| When a record in the master object (parent) is deleted, all records in the detail object (child) related to the master record will be deleted. | You can configure the child object to control when the parent is being deleted, either to clear the parent record value in the child record, or to not allow deletion of the parent record. |
| All child records must have a related parent record. | The parent record may not be required, but you can configure to make the parent field required. |
| The ownership of a child record is determined by the related parent record; the child record does not have an owner. | Each child record has an owner. |
| A detail record inherits sharing and security from the master record. | There is no security sharing or inheritance between related parent and child records. |
| You can relate an existing custom object as a child object, but no records should exist in the child object. | To relate an object to another object, there is no validation on the existing number of records for the child object. |
| If you have a Roll-Up Summary field in the parent object, the create, edit, or delete actions in a child record will trigger edit action in the parent object. If you have validation rule in the parent object, it will trigger for the parent object as well. | You cannot create a Roll-Up Summary in Lookup Relationship using out-of-the-box Salesforce functionality. |
| Supports cross-object workflow; you can define to update a field in the parent record using the value from the child record. | Does not support cross-object workflow. |
| Ability to configure for a child record to allow re-parent to a new parent. | A child record can always re-parent to another parent record. |
| Ability to configure the sharing setting to allow to create, edit, or delete related Detail records based on permission on the parent record. | No sharing setting dependency between parent and child objects. |
