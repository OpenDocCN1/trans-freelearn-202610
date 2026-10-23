+   Reading/exporting data from the data warehouse:

+   **Unloading data into a pandas DataFrame:** There are a couple of options to export data from Redshift. The unload method will export data from Redshift to an S3 staging path and then load it into a pandas DataFrame; this is supposed to be a high throughput option with additional IAM permissions to unload data, but it can help with performance on large datasets. For smaller datasets, users can directly load data into a pandas DataFrame using read_from_sql.

+   **Unloading files directly to S3:** This method is more efficient if you want to export data from Redshift directly into S3/a data lake without loading data into a pandas DataFrame. This method uses the UNLOAD command to move data from Redshift to S3 quickly.

We will look at a couple of use cases similar to the Redshift data loading scenario to understand this better.

**Use case 1:** The data from Redshift needs to be exported to an S3 data lake from where it will be shared with external users, with a pre-signed URL that expires in a week:

```
    : %time
    #select query helps you remove certain columns or make changes as necessary
    wr.redshift.unload_to_files(sql="select * from public.speed_test",
                          path="s3://***/aws-ranger/data-staging/rsexport/",
                          con=con,
                          iam_role="arn:aws:iam::473373155991:role/RedshiftImmersiionRole")
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

**Use case 2:** The data from Redshift is exported to S3 and then transformed before being shared with a specific customer team.