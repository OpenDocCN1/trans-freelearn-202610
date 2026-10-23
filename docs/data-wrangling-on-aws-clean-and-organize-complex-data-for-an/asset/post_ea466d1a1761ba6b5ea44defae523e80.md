
```python
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args["JOB_NAME"], args)

# Script generated for node CData AWS Glue Connector for ServiceNow
CDataAWSGlueConnectorforServiceNow_node1 = (
    glueContext.create_dynamic_frame.from_options(
        connection_type="marketplace.jdbc",
        connection_options={
            "tableName": "incident",
            "dbTable": "incident",
            "connectionName": "service-now-glue-connector",
        },
        transformation_ctx="CDataAWSGlueConnectorforServiceNow_node1",
    )
)
```

You can find a helpful blog post at [https://aws.amazon.com/blogs/big-data/extract-servicenow-data-using-aws-glue-studio-in-an-amazon-s3-data-lake-and-analyze-using-amazon-athena/](https://aws.amazon.com/blogs/big-data/extract-servicenow-data-using-aws-glue-studio-in-an-amazon-s3-data-lake-and-analyze-using-amazon-athena/), which provides a detailed, step-by-step guide on how to build a pipeline that connects ServiceNow and brings data to an Amazon S3 data lake using AWS Glue Studio. This blog post includes instructions on subscribing to the ServiceNow connector in AWS Marketplace, creating a connection using the connector, and building an ETL pipeline using AWS Glue Studio. Additionally, it explains how to analyze the ServiceNow data using Amazon Athena once it has been ingested into the data lake.

# Summary

In this chapter, we covered Apache Spark, its connection with AWS Glue, and the various features available in AWS Glue, including AWS Glue Data Catalog for data discovery, AWS Glue Crawler for metadata extraction, and AWS Glue Studio for building UI-based ETL pipelines. We also explored how to use the AWS Glue Marketplace to subscribe to different connectors so that we can extract data from SaaS applications.

In the next chapter, we will discuss another essential service that plays a significant role in the data wrangling and discovery process: Amazon Athena.

# Working with Athena

In the previous chapters, you learned about the AWS Glue and Amazon S3 services and how they fit into the data wrangling pipeline. In this chapter, we will explore how Amazon Athena service can help in the data discovery, data structuring, data enriching, and data quality phases of the data wrangling pipeline. We will work with some practical examples and perform an analysis on the data to keep it more interesting.

This chapter covers the following topics:

- Understanding Amazon Athena
- Advanced data discovery and data structuring with Athena
- Enriching data from multiple sources using Athena
- Setting up a serverless data quality pipeline with Athena

## Understanding Amazon Athena

Amazon Athena is an interactive, serverless analytical service that can help to explore data available in Amazon S3 without loading the data. Athena helps to analyze the data through SQL syntax or interactive Spark applications. It can help customers to analyze datasets stored in multiple data formats on Amazon S3 through familiar SQL. Since Athena is a serverless offering, users are charged based on the data scanned by the queries and there is no need to maintain separate servers to enable user queries. This has enabled business and analytics teams to quickly analyze, transform, and visualize data in the data lake for specific use cases without relying on the data engineering team to come up with complex ETL pipelines.

Amazon Athena was launched on re:Invent 2016 and has gone through significant improvements over the course of time. It was launched as a serverless mechanism to query Amazon S3 data using managed Presto servers from the Athena catalog (different from the Glue catalog). Now, it supports querying data from the Glue catalog, Query federation, CTAS statements, Lake Formation integration, support for transactional data lake formats, Athena for Spark, and more.

Athena supports two modes for data analysis after the announcement of Athena for Apache Spark in *re:Invent 2022*, as follows:

- Users can query data using SQL syntax, which is similar to Presto/Trino but with limitations. This enables analysts without programming language expertise to analyze the data in the S3 data lake without support from anyone. The following blog contains best practices for Amazon Athena usage for optimizing cost and improving performance for SQL-based analysis: [https://aws.amazon.com/blogs/big-data/top-10-performance-tuning-tips-for-amazon-athena/](https://aws.amazon.com/blogs/big-data/top-10-performance-tuning-tips-for-amazon-athena/).
- Athena for Spark supported Apache Spark version 3.2 when it was launched. Refer to the Athena documentation for the support of recent Spark versions. For additional details about Athena for Spark, refer to the following blog.

### When to use SQL/Spark analysis options?

If you want to perform analytics on your data lake without loading it into another storage layer, use Athena SQL to catalog and query the data. You can also use CTAS statements to perform lightweight data transformations. If you want to execute Spark programs in an interactive and serverless manner for some of your analysis use cases, the Athena for Spark option can be used.

## Advanced data discovery and data structuring with Athena

In this section, we will explore how Amazon Athena helps in performing advanced data discovery and data structuring phases of the data wrangling pipeline.

### SQL-based data discovery with Athena

Amazon Athena provides SQL-based data exploration and supports advanced data types as well. The advantage here is that even people with no coding expertise can explore data with familiar SQL syntax on multiple formats of data and different storage types.

Before we query data using Amazon Athena, we need to create metadata for the table in the Glue catalog. Amazon Athena helps you to query data from different data sources, as follows:

- **AWS Glue catalog**—Access tables that are available in the AWS Glue catalog. We can create tables in the AWS Glue catalog in multiple ways, which we will explore later in this chapter.
- **Federated data sources**—Supports querying data from Apache Hive, AWS, and third-party databases and services. For an extensive list of supported data sources, refer to [https://docs.aws.amazon.com/athena/latest/ug/connectors-prebuilt.html](https://docs.aws.amazon.com/athena/latest/ug/connectors-prebuilt.html).

In addition to prebuilt connectors, you can also create custom connectors to query the data source of your choice. We will explore in detail federated data sources further in this chapter.

Let us understand more about the Glue catalog, which will be the central metadata store for most Athena queries. The Glue catalog is a persistent metadata store that lets users store, annotate, and share metadata. There will be a unique Glue catalog per region within each AWS account. You can use **Identity and Access Management (IAM)** policies to define and control access to users for the Glue catalog and tables. For fine-grained data access (record and column level), users will have to explore the AWS Lake Formation service.

If you look at the AWS Glue console, you can see different links under the **Data Catalog** section, as shown here:

![](img/e28602b44ec813a47df57c96b204412c_223_0.png)

Let's look at these sections in a bit more detail:

- **Databases:** A database is a container for storing table metadata information from different data sources. Users can understand and relate this to the concept of schema/databases from the database world.
- **Tables:** Each table will have a schema definition of the source data. We want to emphasize that this is only a metadata definition of the actual data stored somewhere else, such as a S3 data lake. It also creates partitions in the table, when the files have the same schema, data format, and compression format.

Tables can be created from three sources, as follows:

- The crawler creates new tables when it's run. We can keep the schema updated with new partitions, additional columns, and so on by running the Glue crawler on a schedule.
- Manually add the schema definition. You can go through this approach when you want to customize the name of the table/column, show only specific columns from the dataset, and so on.
- Create a table from an existing schema from the schema registry for streaming data sources.
