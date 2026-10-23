# Data ingestion from SaaS data stores

So far, we have learned how you use AWS Glue to bring data from object stores such as Amazon S3 and relational databases such as Microsoft SQL Server. Now, we are going to learn how you use AWS Glue to ingest data from SaaS data stores.

To connect to SaaS data stores, you need to use an AWS Glue Marketplace connector; these were introduced in December 2020. At the time of writing, the Marketplace has a total of 66 connectors, which include AWS and third-party connectors such as SnowFlake, ServiceNow, Google BigQuery, ElasticSearch, Apace Iceberg, Delta Lake, and more:

![](bbox=[0.13, 0.285, 0.855, 0.545])

Figure 6.25 – AWS Glue Marketplace connectors

To connect ServiceNow and bring data to an Amazon S3 data lake, follow these steps:

+   1. Visit the AWS Marketplace (https://us-east-1.console.aws.amazon.com/gluestudio/home?region=us-east-1#/marketplace) and subscribe to the ServiceNow connector. Review the documentation, pricing, and user guide before proceeding.
+   2. Create a connection using the subscribed ServiceNow connector.
+   3. Build an ETL pipeline using AWS Glue Studio:

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job

args = getResolvedOptions(sys.argv, ["JOB_NAME"])
```