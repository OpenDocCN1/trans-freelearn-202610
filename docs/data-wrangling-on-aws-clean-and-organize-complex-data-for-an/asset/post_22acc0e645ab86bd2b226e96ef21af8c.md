
## Schema (60)

View and manage the table schema.

Filter schemas

| # | Column name | Data type | Partition key | Comment |
|---|---|---|---|---|
| 1 | date | bigint | - | - |
| 2 | state | string | - | - |
| 3 | positive | bigint | - | - |
| 4 | probablecases | bigint | - | - |
| 5 | negative | bigint | - | - |
| 6 | pending | bigint | - | - |
| 7 | totaltestresultssource | string | - | - |
| 8 | totaltestresults | bigint | - | - |
| 9 | hospitalizedcurrently | bigint | - | - |
| 10 | hospitalizedcumulative | bigint | - | - |
| 11 | inicucurrently | bigint | - | - |
| 12 | inicucumulative | bigint | - | - |
| 13 | onventilatorcurrently | bigint | - | - |
| 14 | onventilatorcumulative | bigint | - | - |
| 15 | recovered | bigint | - | - |

Figure 6.19 – AWS Glue Catalog table schema definition

Having explored the table definition, we can now review the data using Amazon Athena.

Clicking on Table data will redirect us to the Athena console:

## Tables (4)

View and manage all available tables.

Filter tables

| Name | Database | Location | Classification | Deprecated | View data |
|---|---|---|---|---|---|
| covid19_csv | chapter6_covid19 | s3://covid19-lake/rearc-co... | csv | - | Table data |
| covid19_states_daily | chapter6_covid19 | s3://covid19-lake/rearc-co... | json | - | Table data |
| covid19_us_daily | chapter6_covid19 | s3://covid19-lake/rearc-co... | json | - | Table data |
| covid19_us_total_latest | chapter6_covid19 | s3://covid19-lake/rearc-co... | json | - | Table data |

Figure 6.20 – Tables list in AWS Glue Data Catalog

Here, we accessed the data of the `covid19_csv` table using Amazon Athena. By clicking on **Table data**, we were redirected to the Athena console, where we can see the data of the table. Other tables are also listed in Athena. In the upcoming chapter, we will explore Athena in more detail:

![](img/e28602b44ec813a47df57c96b204412c_212_0.png)

In this example, we demonstrated how to use AWS Glue Catalog to catalog the Covid-19 dataset and run SQL queries against it using Amazon Athena. In the next section, we will focus on how to perform data ingestion using AWS Glue ETL.

## Data ingestion using AWS Glue ETL

In the previous section, we learned how to use various features of AWS Glue Crawler and AWS Glue Data Catalog to create a centralized data catalog for data discovery. In this section, we will explore the option of using AWS Glue ETL for data ingestion from various data sources, such as data lakes (Amazon S3), databases, streaming, and SaaS data stores. Additionally, we will learn about how to use job bookmarks to perform incremental data loads from Data Lake (S3) and JDBC.

Glue enables users to create ETL jobs using three different types of ETL frameworks – Spark ETL, Spark Streaming, and Python Shell. In the introduction section of Glue DataBrew, we learned how AWS Glue has evolved and that now, AWS Glue Studio is available to build ETL pipelines.

The AWS Glue user interface allows you to build your ETL pipeline with an interesting feature that converts your UI job into a script, which helps you scale when building similar pipelines or when you need some help to get started and then add customization.

In addition to supporting multiple Spark frameworks to process data, Glue provides advanced features during job creation, such as auto-scaling, job bookmarking, enabling continuous logging, the Spark UI, Flex instances, and passing Spark and job parameters.

In addition to the Spark framework, Glue ETL provides Glue-native advanced frameworks/APIs and formats for complex ETL, such as GlueContext, DynamicFrame, job bookmarking, Glue Parquet, Glue Interactive session, workflows, triggers, and more.

Let’s explore these features one by one.

### AWS GlueContext

The `GlueContext` class wraps Apache’s `SparkContext` and can be initialized using the following code snippet:

```
```python
sc = SparkContext()
glueContext = GlueContext(sc)
```
```

GlueContext allows you to communicate with the Spark framework and is the entry point for many Glue ETL features, such as DynamicFrame, job bookmarking, job parameters, and more.

### DynamicFrame

Glue ETL’s DynamicFrame is a vital feature that enables Spark developers to create more efficient Spark pipelines, and it is native to Glue. A Glue DynamicFrame is a distributed collection of self-describing DynamicRecord objects, but it does not require them to adhere to a fixed schema. Due to the self-describing nature of records in DynamicFrame, it does not require a schema to be created and can be used to read/transform data with inconsistent schemas.

You can create a Glue DynamicFrame using different APIs, depending on your use case. For example, the following code snippet creates a DynamicFrame object called `datasource0` for the `my_table` table in the `my_database` database:

```
```python
datasource0 = glueContext.create_dynamic_frame.from_catalog(database_
name='my_database', table_name='my_table', transformation_
ctx='datasource0')
```
```

In this statement, we used the GlueContext object, which uses the Glue SDK, to connect to Glue Data Catalog and fetch the data source classifications and properties to create the object. Users can also pass additional options using the `additional_options` parameter, such as the `push_down_predicate` parameter.

Besides using the `from_catalog` method to create a DynamicFrame, you can create DynamicFrames using the following methods:

- **`from_options`**: This method allows users to create DynamicFrames by manually specifying the connection type, options, and format. This method provides users with the flexibility to customize options for a data store.
- **`from_rdd`**: This method allows users to create DynamicFrames using Spark RDDS.

To leverage the features available in DataFrame, you can convert a DynamicFrame into a DataFrame using the following syntax:

```
df = datasource0.toDF()
```

Similarly, you can convert a Spark DataFrame into a Glue DynamicFrame using the following syntax:

```
from awsglue.dynamicframe import DynamicFrame

dyf = DynamicFrame.fromDF(dataframe=df, glue_ctx=glueContext, name="dyf")
```

Both DynamicFrame and DataFrame are high-level APIs that interact with Spark RDDs. However, the structure of a DynamicFrame is different from a DataFrame:

| Spark DataFrames | Glue DynamicFrames |
| ----------------- | ------------------ |
| Spark Core : RDD |  |

Figure 6.22 – AWS Glue ETL high-level architecture

It is important to note that converting a Glue DynamicFrame into a Spark DataFrame requires a full Map stage in Spark, which can be resource-intensive and add extra time to your job execution time. This is because converting a DynamicFrame into a DataFrame blocks Spark, using all the optimization done against DynamicFrame.

### AWS Glue Job bookmarks

AWS Glue offers the unique capability of performing bookmarking, which distinguishes it from other Spark-based ETL solutions such as running Spark on Amazon Elastic MapReduce (EMR). Bookmarking enables incremental data loading by tracking the data that has already been processed and written. This is particularly useful when dealing with large datasets or extracting data from databases.

Job bookmarks are available for JDBC data sources, Amazon S3, and relational transforms. For Amazon S3, job bookmarks are available for the JSON, CSV, Apache Avro, XML, Parquet, and ORC file formats. When working with JDBC sources, users need to specify a column as the bookmark key; otherwise, AWS Glue will use the primary key as the bookmark key, provided that it is sequentially increasing or decreasing without gaps. If user-defined bookmark keys are used, they should be monotonically increasing or decreasing; gaps are permitted in this case.

For Amazon S3, AWS Glue uses the last modified time of the objects to determine which objects need to be reprocessed. There are various processing options available, such as resetting job bookmarks and rewinding. Resetting job bookmarks allows users to reprocess all the data using the same
