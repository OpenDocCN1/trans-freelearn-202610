We will also convert the CSV data into Parquet format, along with the partitioning schema that we require based on access patterns from data. We will partition the table based on country code to access the weather data for each country in a performant manner.

We have already loaded the CSV table manually, earlier in this chapter, into a `noaa_data_csv` table. We will use the data from the table and extract the `countrycode` value from the first two characters of the `id` column. We will also convert the `date` field into a `date` type for running efficient time-based filters in our queries:

```
```
create table noaa_data_parquet
with (
    format = 'Parquet',
    write_compression = 'GZIP',
    partitioned_by = ARRAY['countrycode'],
    external_location = 's3://<bucketname>/noaa_data_parquet/')
as
select
    id, element, data_value, m_flag, q_flag, s_flag, obs_time, cast(date_
    parse(cast("date" as varchar(8)), '%Y%m%d') as date_
    field, substr(id, 1, 2) as countrycode
from noaa_data_csv
```
```

Please note that we have used a `partition` column in the preceding query, which we didn't use in the inventory table (`ghndinv`). We will get an error when this query is executed, as shown in the following screenshot:

![](bbox=[0.13, 0.56, 0.87, 0.77])

Athena supports inserting only 100 partitions at a time, and we have 218 unique country code values in our data, as shown in *Figure 7.17*:

```
```
Select substr(id, 1, 2) as countrycode
from noaa_data_csv
group by substr(id, 1, 2) order by substr(id, 1, 2)
```
```