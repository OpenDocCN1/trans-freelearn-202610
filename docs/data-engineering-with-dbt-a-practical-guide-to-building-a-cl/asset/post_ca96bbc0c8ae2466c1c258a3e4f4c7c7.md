
## Patterns for Frequent Use Cases

In this last chapter, we want to present you with a small library of patterns that are frequently used for ingesting data from external files and storing this ingested data in what we call history tables.

We will start this chapter from the key task of the automation of data ingestion. We will discuss alternative approaches and their pros and cons, and provide you with our opinionated insights and code to ingest data in Snowflake.

Next we will extend the basic pattern for storing the history of changes in an entity that we presented in previous chapters. We will provide you with a set of frequent use cases for storing incoming data and the code and conditions to deal with all these different needs.

In this chapter, you will learn about the following topics:

- Ingestion patterns
- History patterns

### Technical requirements

This chapter is more technical than the previous ones, so a good understanding of SQL and knowledge of how dbt and Snowflake work is needed to fully understand all the topics. However, for many of the topics that will be covered, having an understanding of SQL might be enough to get the basics.

All the code samples for this chapter are available on GitHub at https://github.com/PacktPublishing/Data-engineering-with-dbt/tree/main/Chapter_15.

### Ingestion patterns

Ingesting data into the platform is still one of the key aspects of building any data platform – without data, there is no data platform.

It is a topic with a big range of possibilities as ingesting data depends on the data platform, the storage service, the file format, and many more big and small details.

In this chapter, we will concentrate on Snowflake, where it is possible to load data in multiple ways. However, the most efficient way to ingest decent amounts of data is through files. The concepts that we described for Snowflake apply, with some adaptations, to other platforms.

As an example, it is possible to use SQL insert statements, but the throughput is orders of magnitude lower than file-based ingestion or reading data from external tables (that, under the hood, read from files), but that also has its drawbacks, as we will see in a second.

In this section, we will present some of the most common use cases for ingesting data into Snowflake using dbt and one possible way to solve them, using code that implements tried and tested best practices, and should be simple to use.

Let’s look at the different ways we can load data into Snowflake:

- **SQL INSERT**: This is the standard SQL INSERT command that you know from the transactional databases. It works well for inserting a few rows, but it does not scale.

    dbt uses the SQL INSERT command to load the rows from the CSV files that you place in the seed/data folder into Snowflake. It is a way to limit the size of what you can load. Good luck loading 10K or 100K rows...

    This is not a viable way to ingest any decent amount of data.

- **EXTERNAL TABLE**: In Snowflake, it is possible to define external tables that point to a folder in a container on an external storage service and the table will parse the files according to the provided file format and expression for the fields.

    This is a very flexible way to access external data, structured or semi-structured. Snowflake can exploit the schema information stored in semi-structured files, making the creation very simple.

    The major downside of using external tables is that every time that you query the table, and you do not hit the cache, all the files that back it must be loaded, parsed, and eventually filtered to answer your query.

    Some improvement can be achieved by adding partitioning based on the file/path name, but they help only if you query with a filter on a partitioning column. In the special case of Delta Lake-based tables, Snowflake can use the metadata in the Delta Lake log files to just parse the files that make up the current state of the table. However, they still must be parsed at every query (that does not hit the cache).

- **COPY INTO**: The COPY INTO command is the most important for loading data in Snowflake because all high-performance ingestion methods rely on ingesting files through this command in one way or the other.

    You can do it actively by issuing the `COPY INTO` command directly (manually or with a script, as we will do) or indirectly by having a Snowpipe issue it. Even when ingesting data through the Snowflake Kafka Connector, you end up writing temp files that are then ingested through a `COPY INTO` command.

    The main advantage of using `COPY INTO` is that files are parsed only once. This is because Snowflake tracks the files loaded in the destination table, which is often called a “landing table,” so that at every `COPY INTO` query, Snowflake will only load new or modified files in the landing table.

    A further advantage is that at the end of the `COPY INTO` command, the data is in a normal Snowflake table and every query that you issue on it takes advantage of micro-partitions and the associated metadata to reduce the data that’s queried.

    Another advantage is that you could transform the data while loading it, using almost all normal SQL expressions and functions, even though we will not do that.

    As a final note, consider that, with `COPY INTO`, you can write from Snowflake tables into files of compatible formats, allowing you to feed data back into a data lake.

    The effort to create a SQL expression to generate the columns of an external table is comparable to that of writing a SELECT statement to do the same as part of a `COPY INTO` command to load the data into a landing table.

    Considering the similar effort, we generally prefer to use the landing table approach so that data is parsed only once and then is immediately ready to be queried with the best possible performance. This makes little difference for small tables but is a noticeable difference when you have millions of rows in a table or GBs of data in files.

Now that we have discussed the possible way to ingest data, let’s see what techniques we will present to load different types of data:

- 1. Ingestion of **structured data**, which is typically done by loading CSV files.

    We will ingest the data by using the `COPY INTO` command inside a script that will be using the position of each column in the files. Column names and data types will be put in place by the `CREATE TABLE` statement for the landing table.

    In the special case that we are reading from a Delta Lake, then we would consider the external table approach for small to mid-sized tables, as the performance penalty is reduced sensibly because Snowflake will just parse the Parquet files that make up the current table.

- 2. Ingestion of **semi-structured data** in the file formats that Snowflake supports, such as JSON, Parquet, AVRO, and ORC.

    In this case, we will use the landing table approach, but we will show how Snowflake can use the schema information embedded in some of these file formats to make our life easier and type little or nothing.

The file format, as you can guess, encapsulates all that is needed to read the files in the stage and is optional. If we provide it with the stage, we do not need to provide the file format when we are reading from the stage and the format set in the stage is fine.

The **Directory Table** feature was recently added to Snowflake stages to allow us to query the file metadata of staged files like we do when listing a directory.

To add the directory service to a stage, add the following to the stage definition:

```
DIRECTORY = ( ENABLE = TRUE
             REFRESH_ON_CREATE = TRUE )
```

Once you have added the directory table to a stage, you can run queries on it with the following syntax while using the DIRECTORY function on the stage name:

```
SELECT * FROM DIRECTORY(@mystage)
WHERE SIZE > 100000;
```

The preceding code makes it possible to query the metadata fields of the files, such as FILE_URL, LAST_MODIFIED, or RELATIVE_PATH.

When you use the Directory Table feature, you have to refresh the file list either by providing Snowflake with integration from the underlying cloud provider or by issuing a manual request before querying it.

While planning your file containers, consider that when you have many files in a stage, numbered in the many hundred thousand or millions, the file access operations, including filtering the files to ingest and refreshing the directory, become slower.

In our ingestion automation, we do not need to use a Directory Table as we rely on the ability of Snowflake to track the files that have been already processed and loaded into a landing table by the COPY INTO command.

### Authorizing access to your files

Now that you understand that a stage describes where the files are, we have the problem of providing the authorization information to access such files.

In Snowflake, it is possible to do this in multiple ways, depending on the cloud provider where the files are, including providing the credentials when creating a stage or loading or unloading data.

We prefer to use **Storage Integration**, a Snowflake object that stores the identity and the authorization received by the external cloud provider, along with some metadata, that can be used to create multiple stages or loads/unloads without the need to know the credentials. This provides both extra security and ease of use.
