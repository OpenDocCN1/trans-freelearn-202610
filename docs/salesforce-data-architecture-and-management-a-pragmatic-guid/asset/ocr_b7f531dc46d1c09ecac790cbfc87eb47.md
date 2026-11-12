As can be seen in the preceding diagram, the logical data model has more details, including the names of the attributes (fields in Salesforce). Sometimes, the data type and length of each attribute are also listed.

# Physical data model

This is the most detailed level in data modeling and is primarily used for the actual build of the database. At the physical level, entities are really tables and attributes are columns in those tables. When visually representing this type of data model, the convention is to use the actual database names of the tables, and columns along with the data type of each attribute. This level also includes indexes, triggers (different to the triggers we build in Salesforce, although the idea is similar), and constraints. I won't present examples of this type to avoid causing confusion because when you create an object using the frontend in Salesforce, you are really sending a command to create a table in the database with its column, primary keys, and constraints. We will look at how all that works later in this chapter. Here is an example of a physical data model for our recruitment app:

![](bbox=[0.14, 0.41, 0.86, 0.54])

Figure 2.4 – Example of a physical data model

The physical data model, as shown in the preceding diagram, shows the actual attribute (field) names as well as the data type and length of each attribute. Now that we understand data models, let's look at the concepts of normalization and denormalization.

# Normalization

**Normalization** is a database technique for organizing data where large tables are divided into smaller manageable tables while linking them using relationships. The reason this is done is to maintain data integrity and eliminate data redundancy. The theory of normalization covers the different normalization forms starting from 1 Normal Form (1NF), 2NF, 3NF, up to 5NF, but a detailed discussion on normalization is beyond the scope of this book.