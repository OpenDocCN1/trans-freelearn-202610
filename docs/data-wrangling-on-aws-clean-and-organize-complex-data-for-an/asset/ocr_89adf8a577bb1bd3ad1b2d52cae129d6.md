| ▼ | id | ▼ | latitude | ▼ | longitude | ▼ | elevation | ▼ | state | ▼ | name |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  | US1CTR00002 |  | 41.4230 |  | -73.5187 |  | 256.0 |  | CT |  | DANBURY 2.8 NNW |
|  | US1CTR00003 |  | 41.1486 |  | -73.4591 |  | 71.6 |  | CT |  | NEW CANAAN 1.9 ENE |
|  | US1CTR00005 |  | 41.1028 |  | -73.4893 |  | 60.0 |  | CT |  | DARIEN 3.6 N |

Figure 7.22: Amazon Athena querying the table loaded with a regex statement

3. We will join tables using the id column and then filter and aggregate on records only from state='CT' and country code='US' (partition column to speed up analysis) to identify the maximum temperature in the current century (21ⁱ⁰ century), as follows:

```
```
select max(data_value) from noaa_data_parquet t1 inner join
 ghnd_stations t2 on t1.id=t2.id
where countrycode='US' and state='CT' and element='TMAX' and
 date_field>cast('2000-01-01' as date)
```
```

We can see from the following screenshot that we have 54.4°C as the maximum temperature recorded within this century:

![](bbox=[0.18, 0.41, 0.85, 0.63])

4. Perform further analysis by identifying the places with the top three recorded temperatures in this century within Connecticut. We will use a partition and rank function to achieve this result by executing the following query:

```
```
select * from ( select t1.id,t1.data_value,t1.obs_time,t2.
 state,t2.name,
 ROW_NUMBER() OVER (PARTITION BY t1.countrycode,t2.state ORDER BY
 data_value DESC) AS temp_rank
 from noaa_data_parquet t1 inner join ghnd_stations t2 on
 t1.id=t2.id
where countrycode='US' and state='CT' and element='TMAX' and
 date_field>cast('2000-01-01' as date) ) TMP_TABLE
 where temp_rank <=3
```
```