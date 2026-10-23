# Schema (60)

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

# Tables (4)

View and manage all available tables.

Filter tables

| Name | Database | Location | Classification | Deprecated | View data |
|---|---|---|---|---|---|
| covid19_csv | chapter6_covid19 | s3://covid19-lake/rearc-co... | csv | - | Table data |
| covid19_states_daily | chapter6_covid19 | s3://covid19-lake/rearc-co... | json | - | Table data |
| covid19_us_daily | chapter6_covid19 | s3://covid19-lake/rearc-co... | json | - | Table data |
| covid19_us_total_latest | chapter6_covid19 | s3://covid19-lake/rearc-co... | json | - | Table data |

Figure 6.20 – Tables list in AWS Glue Data Catalog