# # Using Database Relationships

One of the goals of the normalization process is to remove data redundancy. To do that, you must divide tables into multiple related subject-based tables. There are three main types of relationships—one-to-one, one-to-many, and many-to-many. Take a look at these in more detail:

+   • **One-to-one relationship**: In this type of relationship, each record in the first table has one—and only one—linked record from the second table. And similarly, each record from the second table can have one—and only one—matching record from the first table. In this relationship, one of the tables is effectively considered an extension of the other. In many cases, the additional attributes will simply end up being added to one of the tables. That is why this relationship is not very common. However, it still has its valid cases, including splitting the data for security reasons.

In Salesforce, this is particularly useful considering the rich sharing model available. You can implement one-to-one relationships in many ways, with the most common being to create a lookup relationship from each object to the other, with an automation script (trigger, workflow, process builder, and others) to ensure both lookup fields are always populated with the right values and each object is linked to its counterpart:

![](bbox=[0.281, 0.438, 0.733, 0.565])

Figure 2.12 – One-to-one relationship

+   • **One-to-many relationship**: This common type of relationship occurs when a record in one table (sometimes referred to as a **parent**) is associated with multiple records of another table (sometimes referred to as a **child**). There are plenty of examples of this, including a customer placing multiple orders, an account with multiple addresses, and an order with multiple order line items.

To have this relationship, you would need to use what are known as primary keys and foreign keys. In the child table, the primary ID would be the unique identifier for each record in that table, while the foreign key would refer to the unique identifier of the parent's primary key. This type of relationship can be created in Salesforce using lookup fields or master-detail fields. This book assumes you already know the differences between the two. You will also cover some particular differences that you need to remember in later chapters: