
## Time-series data sources

In data science and engineering, one of the most common challenges is temporal data manipulation. Datasets that hold geospatial or transactional data, which mostly lie in the financial and economics area of an application, are some of the most common examples of data that is indexed by a timestamp. Working in areas such as finance, fraud, or even socio-economic temporal data ultimately leads to the need to join, aggregate, and visualize data points.

This temporal data regularly comes in datetime formats that might vary not only in the format itself but in the information that it holds. One of the examples of this is the difference between the DD/MM/YYYY and MM/DD/YYYY format. Misunderstanding these different datetime formats could lead to failures or wrongly formed results if the formats used don't match up. Moreover, this data doesn't come in numerical format, which—as we have seen in previous sections of the chapter—can lead to several impediments that need to be overcome, one of them being the fact that this data cannot be interpreted easily by most of the learning algorithms used in machine and deep learning.

This is where feature engineering comes into play, providing us with the tools to transform and create new features from this data. An example of this could be to reorganize the data in numerical features such as day, month, and year, and even manipulate features using techniques as dynamic time warping to compare time series of different lengths, as could be the case when comparing the sales of the months of February and March, which have 28 and 31 days respectively.

In Azure Databricks, we have several functionalities in place that allow us to perform operations such as joins, aggregations, and windowing of time series, with the added benefit of doing this processing in parallel. Moreover, the Koalas API allows us to work with this data using a Pandas-like syntax that makes the transition from experiment to production much easier to handle.

In this example, we will work with financial data to illustrate how we can manipulate temporal datasets in Azure Databricks. The data that we will use is an example that is based on the stock market and holds trade information for different companies that are being traded. You can find more information about this kind of data at this link: https://www.tickdata.com/product/nbbo/:

- 1. We can get example data by running the following code in an Azure notebook cell. This will download the data that we will read afterward:

```
%sh
wget https://pages.databricks.com/rs/094-YMS-629/images/ASOF_Quotes.csv ;
```

```
wget https://pages.databricks.com/rs/094-YMS-629/images/ASOF_Trades.csv ;
```

Here, we have downloaded two kinds of datasets, one for trades and one for offers. We will merge this data afterward over the time column to compare the trades and quotes made at the same points in time.

- 2. Before reading the data, we will define a schema for it because this information is already known to us. We will assign the PySpark TimestampType class to the columns that hold temporal data about the trade execution and quote time. We will also rename the trade and execution time column names to events_ts to finally convert this data into Delta format. The code to do this is shown in the following snippet:

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

- 3. We have decided to use Delta tables to benefit from the optimized data format that allows us to work with large compressed flat files and harness the power of the underlying engine, enabling us to easily scale and parallelize the process according to the amount of data available. The code is illustrated in the following snippet:

```
spark.read.format("csv").schema(trade_schema).
option("header", "true").option("delimiter", ",").load("/tmp/finserv/ASOF_Trades.csv").write.mode('overwrite').
```

```
format("delta").save('/tmp/finserv/delta/trades')
```

```
spark.read.format("csv").schema(quote_schema).option("header", "true").option("delimiter", ",").load("/tmp/finserv/ASOF_Quotes.csv").write.mode('overwrite').format("delta").save('/tmp/finserv/delta/quotes')
```

- 4. After we have read and stored the data into the Delta tables, we can check that the data has been parsed correctly by displaying the resulting Delta table as a Spark Data Frame, as follows:

```
display(spark.read.format("delta").load("/tmp/finserv/delta/trades"))
```

Now that we have our transactional data available, we can use it to merge the trades and quotes, aggregate the trades, and perform windowing operations on it.

## Joining time-series data

When working with time-series data, an as-of join is a merge technique that commonly refers to obtaining the value of a given event in the exact moment of the timestamp. For most of this temporal data, different types of datetime series will be joined together. In our particular case, we want to know for each company in the dataset the particular state of trade at any given point in time that we have available. Here, these states can be—for example—the NBBO, which is an acronym for National Best Bid Offer in trading and investing.

In the following example, we will obtain the state of the NBBO for each company available in the dataset. We will work the data in order to have for each company the state of the latest bid and offer for each of the data points that we have as timestamps. Once we have this computed, we can visualize the difference between bids and offers to comprehend at which points in time the liquidity of the company may hit a low point:

- 1. First, we will merge the data on the symbol. We assume that we have already loaded the Delta tables into two Spark data frames named trades and offers, which we will merge on the symbol column. The code is shown in the following snippet:

```
un= trades.filter(col("symbol") == "K").select('event_ts', 'price', 'symbol', 'bid', 'offer', 'ind_cd').union(quotes.filter(col("symbol") == "K").select('event_ts', 'price', 'symbol', 'bid', 'offer', 'ind_cd'))
```

- 2. After we have performed the merge, we can visualize the results, as follows:

```
display(un)
```

- 3. Now that our data has been merged into a single Spark DataFrame, we can perform the windowing operation. First, we need to define the windowing function that we will use. To do this, we will use the Window class, which is a built-in method in PySpark to perform windowing operations. In the following code snippet, we will define the partition by the symbol column, which represents the company:

```
from pyspark.sql.window import Window
```

```
partition_spec = Window.partitionBy('symbol')
```

- 4. Then, we need to specify the mechanism that will be used to sort the data. Here, we use the ind_cd column as the sort key, which is the column that specifies the values of the quotes before trades:

```
join_spec = partition_spec.orderBy('event_ts').
```

```
rowsBetween(Window.unboundedPreceding,
```

```
Window.currentRow)
```

- 5. Then, we will use SQL commands to query the data and get the lasted_bid value by running a SELECT operation over the last bid and over join_spec, as follows:

```
%sql
```

```
select(last("bid", True).over(join_spec).alias("latest_bid"))
```

This way, we have demonstrated how easy is to manipulate time-series data in Azure Databricks using the PySpark API to run joins, merges, and aggregations on temporal data. In the next section, we will learn how we can leverage the benefit of the Pandas-like syntax to ease the transition from notebooks to production easier.
