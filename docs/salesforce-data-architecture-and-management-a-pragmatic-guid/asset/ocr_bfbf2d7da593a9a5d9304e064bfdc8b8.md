+   • **Durability**: This attribute is related to ensuring that once a transaction has been completed, the results of the transaction must survive any system malfunctions. There are measures that database administrators can take to ensure that their databases are durable, including taking regular backups of data, and modern databases, such as Oracle and SQL Server, have built-in mechanisms to cope with catastrophic failures and still maintain data durability.

Now that we have reviewed relational databases, let’s take a look at non-relational databases in the next section.

# Non-relational databases

You can probably guess from the name that non-relational databases do not use relationships (take primary, foreign keys as an example) as in relational databases, and therefore do not use tables with columns and rows.

Non-relational databases use storage that is optimized for the type of data that will be stored in the database. For example, if it’s JavaScript Object Notation (JSON) data that will be stored, a document data store that is optimized for JSON storage may be appropriate. Non-relational databases are usually referred to as NoSQL databases. This is because they don’t use SQL for querying data and other methods are used to query data. Let’s briefly discuss some NoSQL databases:

+   • **Columnar data stores**: This type of store looks deceptively similar to a relational database, but it stores data in a denormalized format. These databases have column families (similar to tables in relational databases) that have rows that have columns. Values in these columns are essentially tuples of key-value pairs. The column family holds columns of data that are logically related and usually queried and manipulated as a unit. Figure 7.1 shows an example of a column family: