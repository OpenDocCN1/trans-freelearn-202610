
Reading/exporting data from the data warehouse:

- **Unloading data into a pandas DataFrame:** There are a couple of options to export data from Redshift. The unload method will export data from Redshift to an S3 staging path and then load it into a pandas DataFrame; this is supposed to be a high throughput option with additional IAM permissions to unload data, but it can help with performance on large datasets. For smaller datasets, users can directly load data into a pandas DataFrame using read_from_sql.

- **Unloading files directly to S3:** This method is more efficient if you want to export data from Redshift directly into S3/a data lake without loading data into a pandas DataFrame. This method uses the UNLOAD command to move data from Redshift to S3 quickly.

We will look at a couple of use cases similar to the Redshift data loading scenario to understand this better.

### Use case 1
The data from Redshift needs to be exported to an S3 data lake from where it will be shared with external users, with a pre-signed URL that expires in a week:

```
    : %time
    #select query helps you remove certain columns or make changes as necessary
    wr.redshift.unload_to_files(sql="select * from public.speed_test",
                          path="s3://***/aws-ranger/data-staging/rsexport/",
                          con=con,
                          iam_role="arn:aws:iam::473373155991:role/RedshiftImmersionRole")
    CPU times: user 11.4 ms, sys: 255 μs, total: 11.7 ms
    Wall time: 4.73 s
```

Figure 3.72: Redshift integration – exporting data from Redshift using the unload function

The data will be loaded in the configured S3 path, and you can use the S3 Select functionality in the AWS console to verify that the data has populated as expected. Also, you can see there are multiple Parquet files available in the S3 path because the UNLOAD command exports data from Redshift in a parallel manner, thus completing the export in a faster time.

```
{
  "queryKey": "0022133223300013",
  "title": "POLYGON((-160.032348632812 70.6399478155463, -160.02685546875 70.6399478155463, -160.02685546875 70.63812673065321, -1
  "avg_d_kbps": 85,
  "avg_u_kbps": 103,
  "avg_lat_ms": 72,
  "tests": 1,
  "devices": 1
}
```

Figure 3.73: Redshift integration – Redshift data exported in S3 and queried using S3 Select in the AWS console

### Use case 2
The data from Redshift is exported to S3 and then transformed before being shared with a specific customer team.

We can use the `unload` command to export files to an S3 staging location and load them into a pandas DataFrame. You can also see the results from the DataFrame printed as follows:

```
```text
%time
#select query helps you remove certain columns or make changes as necessary
wr.redshift.unload(sql="select * from public.speed_test", 
       path="s3://***.redshift.amazonaws.com/us-west-1/yourbucket/data/staging/rsxport_stg/",
con=con,
       iam_role="arn:aws:iam::1234567890:role/MyRedshiftRole")

CPU times: user 13.8 s, sys: 2.01 s, total: 15.8 s
Wall time: 16.3 s
```
```

| | quadkey | tile | avg_d_kbps | avg_u_kbps | avg_lat_ms | tests | devices |
|---|---|---|---|---|---|---|---|
|0| 0022133222330013 | POLYGON((-160.323438632812 70.63994784155403,... | 85 | 103 | 72 | 1 | 1 |
|1| 0022133222330201 | POLYGON((-160.043343690938 70.63484406683086,... | 14947 | 20624 | 70 | 1 | 1 |

Figure 3.74: Redshift integration – exporting data from a pandas DataFrame and an S3 staging location

We can use the `read_from_sql` function to directly load data into a pandas DataFrame. We can see in the following figure that the data was loaded much faster with a direct SQL load function. However, this test was done on a `t3.2xlarge` machine, and your response time might vary based on machine capacity and other factors. You can run this test for your specific workloads and use either `unload` or a direct SQL export as necessary:

```
```text
%time
#select query helps you remove certain columns or make changes as necessary
wr.redshift.read_sql_query(sql="select * from public.speed_test", con=con)

CPU times: user 1.2 s, sys: 37.9 ms, total: 1.24 s
Wall time: 5.27 s
```
```

| | quadkey | tile | avg_d_kbps | avg_u_kbps | avg_lat_ms | tests | devices |
|---|---|---|---|---|---|---|---|
|0| 021232020112030 | POLYGON((-123.123779296875 44.048157308235,... | 17609 | 5433 | 51 | 8 | 4 |
|1| 021232020112032 | POLYGON((-123.123779296875 44.0441673535722,... | 20644 | 10528 | 47 | 8 | 3 |
|2| 021232020112033 | POLYGON((-123.118286132812 44.0441673535722,... | 89217 | 16508 | 38 | 7 | 4 |
|3| 021232020112100 | POLYGON((-123.117279296875 44.0560116967853,... | 309396 | 28648 | 26 | 14 | 8 |

Figure 3.75: Redshift integration – exporting data from the pandas DataFrame directly

The takeaway here is that writing to S3 is a parallel activity, but it creates an additional step before loading data into a pandas DataFrame. You should evaluate the different export options based on the use case, compute/storage capacity, and the size of the exported data.

The following table will give you a high-level idea of using different functions to load and unload data from Redshift using `redshift-connector`:

|  | Load from pandas DataFrame | Load from S3 storage | Export to pandas DataFrame | Export to S3 storage |
|---|---|---|---|---|
| Redshift data < 1K records | to_sql function | COPY_from_files | read_from_sql | unload_to_files |
| Redshift data > 1K records | copy function | COPY_from_files | unload or read_from_sql | unload_to_files |

Table 3.1: Functions for RedShift

In addition to using the preceding `redshift-connector` method, we can also use the Redshift DataAPI to connect with Redshift and execute our queries through `awswrangler`.

> **Challenge**
> 
> You can use Redshift Upsert mode to update existing records and insert new records, similar to how we did it in RDS. This will help you to use `awswrangler` in your data pipeline to load data into Redshift on a scheduled frequency.

## Summary

In this chapter, we learned how to use AWS SDK for pandas (aka `awswrangler`). We explored various components available in the `awswrangler` library and how it brings pandas DataFrames closer to the AWS ecosystem. We learned how to customize and install the library for different use cases and development environments. We also looked at `awswrangler` integration with AWS services, such as Amazon S3, Amazon RDS, Amazon Redshift, and Amazon Athena, and the different features available within `awswrangler`.

In the next chapter, we will learn about SageMaker Data Wrangler, which helps us perform data-wrangling activities as a part of ML pipelines.

## Introduction to SageMaker Data Wrangler

Data processing is an integral part of machine learning (ML). In fact, it is not a stretch to say that ML models are only as good as the data that is used to train them. According to a Forbes survey from 2016, 80% of the time spent on an ML engineering project is data preparation. That is an astonishingly high percentage of time. Why is that the case? Due to the inherent characteristics of data in the real world, data preparation is both tedious and resource intensive. This real-world data is often referred to as dirty, unclean, noisy, or raw data in ML. In almost all cases, this is the type of data you begin your ML process with. Even in rare scenarios where you think you have good data, you still need to ensure that it is in the right format and scale it to be useful. Applying ML algorithms on this raw data would not give quality results as they would fail to identify patterns, detect anomalies correctly, or generalize well enough outside their development and training environment.

In today’s connected and digital world, data is generated more rapidly than it can be consumed and processed. This enormous quantity of data that is being generated can be a real advantage for building ML models for applications that were previously unexplored or thought impossible. However, this also presents a few challenges. Let’s look at some of them next.

One of the main challenges is the data quantity. Due to the sheer volume of data that is generated, we need frameworks and tools that can ingest, process, and store the relevant attributes. We need tools that are robust and can scale automatically to meet data ingestion needs. The next challenge is the quality of data. In the real world, data is hardly ever perfect; you have to deal with challenges such as inaccuracies, inconsistencies, missing data, and multiple data formats, to name a few. Here, we need tools that can prepare and clean this data so that it can be used as input to our ML pipelines. Since a lot of this data cleaning is repeatable and time-consuming, these tools should natively support reusability so that data processing practitioners can operate with this effectively with the least amount of friction possible. Data practitioners seldom work in silos. They are usually part of a larger team within the organization and will often need to collaborate with other cross-functional teams. It is also important that the tools they use for data processing support this collaboration and integrate with the organization’s existing tools and repositories.
