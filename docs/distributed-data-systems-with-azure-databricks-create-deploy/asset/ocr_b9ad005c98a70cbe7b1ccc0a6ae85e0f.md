```
df.write.format("tfrecords").option("recordType", "Example").save(path)
After this, we can read the TFRecord file back to a Spark Data frame in the next way:
df = spark.read.format("tfrecords").option("recordType", "Example").load(path)
df.show()
```

3. Here, we see the entire process running in an Azure Databricks notebook:

```
from pyspark.sql.types import *
path = "test-output.tfrecord"
fields = [StructField("id", IntegerType()),
StructField("IntegerCol", IntegerType()),
StructField("LongCol", LongType()),
StructField("FloatCol", FloatType()),
StructField("DoubleCol", DoubleType()),
StructField("VectorCol", ArrayType(DoubleType(), True)),
StructField("StringCol", StringType())]
schema = StructType(fields)

test_rows = [[11, 1, 23, 10.0, 14.0, [1.0, 2.0], "r1"], [21, 2, 24, 12.0, 15.0, [2.0, 2.0], "r2"]]
rdd = spark.sparkContext.parallelize(test_rows)
df = spark.createDataFrame(rdd, schema)
path= 'dbfs:/tmp/dataset'
df.write.format("tfrecords").option("recordType", "Example").save(path)
display(df)
```

(3) Spark Jobs
df: pyspark.sql.dataframe.DataFrame = [id: integer, IntegerCol: integer ... 5 more fields]

| id | IntegerCol | LongCol | FloatCol | DoubleCol | VectorCol | StringCol |
|---|---|---|---|---|---|---|
| 1 | 11 | 1 | 23 | 10 | 14 | [1, 2] | r1 |
| 2 | 21 | 2 | 24 | 12 | 15 | [2, 2] | r2 |
Showing all 2 rows.

Fig.9.2 – TFRecord column example data types