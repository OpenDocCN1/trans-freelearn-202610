# Step 3 – identifying the schema

The next step is to identify the schema of the dataset. For this purpose, we will use the
AWS Glue crawlers. AWS Glue crawlers crawl through the data to detect the schema. If a
schema can be determined (remember there is no guarantee that the data has a specific
schema) then Glue crawlers will populate the Glue Catalog with the schemas identified
after crawling the data. Glue tables always belong to a Glue database. A database in Glue is
just a logical repository of tables in the Glue Catalog:

+   1. Let’s start by creating a database using the `create-database` command:

```
`% aws glue create-database --database-input
"{\"Name\":\"my-data-lake-db\"}" --region us-east-1`
```

+   2. We can verify the successful database creation using the `get-databases`
+   command:

```
`% aws glue get-databases --region us-east-1
{
 "DatabaseList": [
 {
 "Name": "default",
 "CreateTime": 1553517157.0
 },
 {
 "Name": "my-data-lake-db",
 "CreateTime": 1616865129.0
 }
 ]
}`
```

![](bbox=[0.187, 0.089, 0.817, 0.165])