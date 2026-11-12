Conversely, if your data model is scattered with a lot of objects with no relationships, you will need to write a lot of code to even run queries on your data, let alone build your business functionality using clicks.

I am not saying that you should have few objects and that they must be related, but rather encouraging you to think hard and from multiple angles to ensure that you have a very good justification to have a very complex data model with few relationships.

Now that we have looked at the design principles and we've mentioned relationships a few times, let's dive into the different types of relationships we have in Salesforce and how we can leverage them during data modeling.

# # Reviewing object relationships

In a relational database, you would define primary and foreign keys to link up two tables; for example, in an Order Management application, you will have order and order line item tables in which the Order Line object will have a foreign key referencing the Order table. In Salesforce, because we don't get access to the underlying tables, we must establish relationships explicitly using the relationship types provided by Salesforce. In the following screenshot, three different types of relationship are shown:

![](bbox=[0.14, 0.475, 0.86, 0.6])

Figure 2.9 – Three common types of relationship in Salesforce

As can be seen in the preceding screenshot, a **Lookup Relationship** creates a loosely coupled relationship, a **Master-Detail Relationship** is useful for tightly linking the two objects together, whereas an **External Lookup Relationship** allows data from an external system to be linked up with data in Salesforce.

# # Lookup relationship

Like a **Parent-child** field, a **Lookup** field establishes a parent-child relationship, but it differs in many ways. The relationship is loosely coupled, so the record-level security is not inherited from the parent, but the child can have its own independent record-level access, which means a user accessing the parent cannot necessarily access the child record unless the configuration allows that. Unless enforced by configuration, you can have a **Lookup** field that's blank. This is a key difference between **Lookup** and **Parent-child** because in the latter, you don't have the option to leave the field blank.