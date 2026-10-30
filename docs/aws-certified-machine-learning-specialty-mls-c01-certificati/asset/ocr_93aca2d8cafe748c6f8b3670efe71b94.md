11. Select the public schema and now you can query the table to see the records, as shown in Figure 6.7:

![](bbox=[0.13, 0.14, 0.87, 0.42])

Figure 6.7 – A screenshot of Amazon Redshift's Query editor

We now understand how to create an ETL job using AWS Glue to copy the data from the S3 bucket to Amazon Redshift. We also queried the data in Amazon Redshift using the query editor from the UI console. It is recommended to delete the Redshift cluster and AWS Glue job if you have completed the steps successfully. AWS creates two buckets in your account to store the AWS Glue scripts and AWS Glue temporary results. Therefore, delete these as well to save costs. We will use the data catalog created on S3 data in the next section.

In the following section, we will learn about querying the S3 data using Athena.

# Querying S3 data using Athena

Athena is a serverless service designed for querying data stored in S3. It is serverless because the client doesn't manage the servers that are used for computation:

+   - Athena uses a schema to present the results against the query on the data stored in S3. You define how you want your data to appear in the form of a schema and Athena reads the raw data from S3 to show the results as per the defined schema.