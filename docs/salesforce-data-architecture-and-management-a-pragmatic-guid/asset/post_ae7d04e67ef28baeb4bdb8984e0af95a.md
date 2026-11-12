
- Under the Hood: How the Salesforce Platform Handles 1.3+ Billion Transactions per Day: https://developer.salesforce.com/blogs/engineering/2013/10/under-the-hood-how-the-salesforce-platform-handles-1-3-billion-transactions-per-day.html

- Experience Cloud Performance and Scale Best Practices: https://help.salesforce.com/articleView?id=sf.networks_perf_scale_best_practices.htm&type=5

## Section 3: Large Data Volumes (LDVs) and Data Migrations

In this section, you will learn about LDVs – what they are and how to use them in searches and SOQL queries. Topics related to loading LDVs will also be discussed, with examples demonstrating how to load data efficiently without running into issues. In the final chapter, best practices for migrating data and the use of APIs will be discussed.

In this section, the following chapters are included:

- Chapter 7, Working with Large Volumes of Data
- Chapter 8, Best Practices for General Data Migration

## Working with Large Volumes of Data

In this chapter, we will discuss working with big data in Salesforce. Salesforce has been in use by organizations for a long time, and more and more organizations have moved from the implementation and stabilization phase to a more mature growth stage where the focus is on leveraging features introduced during the implementation phase to increase revenues and reduce costs. Business growth means data is growing as well and it needs to be managed properly to prevent any disruption to the business.

We will take a brief dip into different types of databases and then dive right into **Large Data Volumes (LDVs)**, a topic that piques my interest because of the unique challenges associated with it. We will look at what defines an LDV scenario and also discuss how we can avoid these scenarios when possible. And when we do find ourselves working with LDVs, we'll see some of the considerations to take into account to be successful along with the techniques to optimize loading or integrating LDVs.

There is no need to always do a point-to-point integration involving a middleware with Salesforce: that's where Salesforce Connect and external objects come into play. We will review external objects, their limitations, and the benefits of using external objects. And when you are working with very large volumes of data going into hundreds of millions of rows or billions of rows of data, big objects come to the rescue. We will discuss the benefits of big objects and the considerations for them before wrapping up this chapter.

The topics covered in this chapter include the following:

- Revisiting databases
- **Large Data Volumes (LDVs)**
- Salesforce Connect and external objects
- Introducing big objects

By the end of our discussion, we will understand the options available to us when it comes to big data and also which option to use given the use cases we are working with.

### Revisiting databases

Before we dive deep into LDVs and some associated topics, let’s take a brief history trip. Understanding relational and non-relational databases will help us understand the different types of solutions that are available with Salesforce and assist us in determining which one to use depending on requirements. In the early days of modern computing, tapes were used for storage, and the first digital-tape storage system, IBM’s Model 726, could store 1.1 **MB** (megabytes) on a single reel of data. For comparison purposes, these days, a tape cartridge can hold up to 15 **terabytes** (**TB**). Model 726 was a non-relational database in that it was free-form records linked together.

Then came relational databases, which offered a more organized approach to store, retrieve, and search data. Data was stored in multiple tables in keeping with normalization principles and logically linked together using unique identifiers called **primary** and **foreign** keys. Refer to *Chapter 2, Understanding Salesforce Objects and Data Modeling*, for a more in-depth discussion on normalization with the help of examples.

### Relational databases

**Relational databases** provide an optimal medium for transactional and analytic applications and are widely used today as storage backbones for **Enterprise Resource Planning** (ERP) systems, **Customer Relationship Management** (CRM) systems, and analytical applications. The following four characteristics, abbreviated as **ACID** (**Atomic**, **Consistency**, **Isolation**, **Durability**), of relational databases make them ideal for these purposes:

- **Atomic**: Relational databases have a very robust transaction control mechanism built into them. This means that if a transaction fails at any time, it will be rolled back, switching values to earlier values to ensure that data remains accurate. This prevents some values in the database from being partially updated while others are left untouched because of transaction failure. A common example used to demonstrate atomicity is the transfer of funds from one account to another in a bank. The atomicity attribute ensures that the full transaction, that is, debiting one account and crediting the other, is completed, otherwise the full transaction has to fail to keep the database in a consistent state.

- **Consistency**: This means that checks and constraints that are defined in the database are satisfied for every transaction. For example, if a column in a table has been defined to hold integer values, the database shouldn't allow a transaction to insert characters or **Binary Large Objects** (or **BLOBs**, which are used in Oracle databases usually to store images and audio files). Another example is when the customer's name is required in a table that holds customer master data. Any time a null value is entered and a save attempted, the constraint defined on the column will kick in and prevent the save from completing.

- **Isolation**: This attribute is related to transaction visibility to other users and processes of the system. It ensures that a transaction accessing the same data is not running concurrent to another transaction. This is important: let's say a bank account has a balance of $1,000 and two transfers are initiated to transfer $200 each to two different accounts. These should be two individual transactions, but if they run concurrently, both will read that the balance is $1,000, whereas the second transaction should have read the balance as $800 (after the first $200 has been transferred to the other account).

- **Durability**: This attribute is related to ensuring that once a transaction has been completed, the results of the transaction must survive any system malfunctions. There are measures that database administrators can take to ensure that their databases are durable, including taking regular backups of data, and modern databases, such as Oracle and SQL Server, have built-in mechanisms to cope with catastrophic failures and still maintain data durability.

Now that we have reviewed relational databases, let’s take a look at non-relational databases in the next section.

### Non-relational databases

You can probably guess from the name that non-relational databases do not use relationships (take primary, foreign keys as an example) as in relational databases, and therefore do not use tables with columns and rows.

Non-relational databases use storage that is optimized for the type of data that will be stored in the database. For example, if it’s JavaScript Object Notation (JSON) data that will be stored, a document data store that is optimized for JSON storage may be appropriate. Non-relational databases are usually referred to as NoSQL databases. This is because they don’t use SQL for querying data and other methods are used to query data. Let’s briefly discuss some NoSQL databases:

- **Columnar data stores**: This type of store looks deceptively similar to a relational database, but it stores data in a denormalized format. These databases have column families (similar to tables in relational databases) that have rows that have columns. Values in these columns are essentially tuples of key-value pairs. The column family holds columns of data that are logically related and usually queried and manipulated as a unit. Figure 7.1 shows an example of a column family:
