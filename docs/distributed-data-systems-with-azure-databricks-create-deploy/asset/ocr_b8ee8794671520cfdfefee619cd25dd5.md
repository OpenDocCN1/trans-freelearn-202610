```
wget https://pages.databricks.com/rs/094-YMS-629/images/ASOF_Trades.csv ;
```

Here, we have downloaded two kinds of datasets, one for trades and one for offers. We will merge this data afterward over the time column to compare the trades and quotes made at the same points in time.

2. Before reading the data, we will define a schema for it because this information is already known to us. We will assign the PySpark TimestampType class to the columns that hold temporal data about the trade execution and quote time. We will also rename the trade and execution time column names to events_ts to finally convert this data into Delta format. The code to do this is shown in the following snippet:

```
from pyspark.sql.types import *
trade_schema = StructType([
    StructField("symbol", StringType()),
    StructField("event_ts", TimestampType()),
    StructField("trade_dt", StringType()),
    StructField("trade_pr", DoubleType())
])
quote_schema = StructType([
    StructField("symbol", StringType()),
    StructField("event_ts", TimestampType()),
    StructField("trade_dt", StringType()),
    StructField("bid_pr", DoubleType()),
    StructField("ask_pr", DoubleType())
])
```

Once we have downloaded the data and specified the desired schema for our Spark data frame, we can parse the CSV files according to the data schema and store them as Delta tables that we will be able to query afterward.

3. We have decided to use Delta tables to benefit from the optimized data format that allows us to work with large compressed flat files and harness the power of the underlying engine, enabling us to easily scale and parallelize the process according to the amount of data available. The code is illustrated in the following snippet:

```
spark.read.format("csv").schema(trade_schema).
option("header", "true").option("delimiter", ",").load("/tmp/finserv/ASOF_Trades.csv").write.mode('overwrite').
```