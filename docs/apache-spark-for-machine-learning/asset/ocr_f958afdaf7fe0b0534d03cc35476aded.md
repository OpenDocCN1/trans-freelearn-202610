The following code creates a Spark DataFrame, df, containing four columns, named emp_id, region, sales, and customer:

```
df = spark.createDataFrame(
 [(1,"north",100,"walmart"),(2,"south",300,"apple"),
 (3,"west",200,"google"),(1,"east",200,"google"),
 (2,"north",100,"walmart"),(3,"west",300,"apple"),
 (1,"north",200,"walmart"),(2,"east",500,"google"),
 (3,"west",400,"apple")], ["emp_id", "region", "sales", "customer"])
```

Now that we have created a DataFrame, we can apply the aggregate function, sum, on the sales column:

```
df.agg({"sales": "sum"}).show()
```

The sum function adds all the values in the column to give an output value of 2300:

```
+-----------+
|sum(sales) |
+-----------+
|       2300|
+-----------+
```

The following code calculates the minimum value in the sales column:

```
df.agg({"sales": "min"}).show()
```

Here is the output:

```
+-----------+
|min(sales) |
+-----------+
|       100 |
+-----------+
```

The following code calculates the maximum value in the sales column:

```
df.agg({"sales": "max"}).show()
```

Here is the output:

```
+-----------+
|max(sales) |
+-----------+
|       500 |
+-----------+
```