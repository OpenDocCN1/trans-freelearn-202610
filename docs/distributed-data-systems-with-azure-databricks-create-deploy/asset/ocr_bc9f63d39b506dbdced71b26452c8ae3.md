```
format("delta").save('/tmp/finserv/delta/trades')
```

```
spark.read.format("csv").schema(quote_schema).option("header", "true").option("delimiter", ",").load("/tmp/finserv/ASOF_Quotes.csv").write.mode('overwrite').format("delta").save('/tmp/finserv/delta/quotes')
```

4. After we have read and stored the data into the Delta tables, we can check that the data has been parsed correctly by displaying the resulting Delta table as a Spark Data Frame, as follows:

```
display(spark.read.format("delta").load("/tmp/finserv/delta/trades"))
```

Now that we have our transactional data available, we can use it to merge the trades and quotes, aggregate the trades, and perform windowing operations on it.

# Joining time-series data

When working with time-series data, an as-of join is a merge technique that commonly refers to obtaining the value of a given event in the exact moment of the timestamp. For most of this temporal data, different types of datetime series will be joined together. In our particular case, we want to know for each company in the dataset the particular state of trade at any given point in time that we have available. Here, these states can be—for example—the NBBO, which is an acronym for National Best Bid Offer in trading and investing.

In the following example, we will obtain the state of the NBBO for each company available in the dataset. We will work the data in order to have for each company the state of the latest bid and offer for each of the data points that we have as timestamps. Once we have this computed, we can visualize the difference between bids and offers to comprehend at which points in time the liquidity of the company may hit a low point:

+   1. First, we will merge the data on the symbol. We assume that we have already loaded the Delta tables into two Spark data frames named trades and offers, which we will merge on the symbol column. The code is shown in the following snippet:

```
un= trades.filter(col("symbol") == "K").select('event_ts', 'price', 'symbol', 'bid', 'offer', 'ind_cd').union(quotes.filter(col("symbol") == "K").select('event_ts', 'price', 'symbol', 'bid', 'offer', 'ind_cd'))
```