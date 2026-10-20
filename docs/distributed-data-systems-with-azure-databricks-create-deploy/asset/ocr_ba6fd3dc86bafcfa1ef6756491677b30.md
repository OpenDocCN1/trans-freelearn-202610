We can perform transformations such as selecting rows and columns, accessing values stored in cells by name or by number, filtering, and more thanks to the PySpark application programming interface (API).

We will use these transformations in combination with SQL statements to transform and persist the data in our file.

# Querying using SQL

We will use the voting turnout election dataset that we have used before. We will create a view of the data and use SQL to query it.

# Creating views

We can create views using the data in the Spark Dataframe to be able to query it using SQL, as follows:

+   1. To create a view that we will call voter_turnout, we can use the following command:

```
df.createOrReplaceTempView("voter_turnout")
```

+   2. After this, we can use a SELECT command to get every record in the view that we have just created, as follows:

![](bbox=[0.125, 0.545, 0.86, 0.735])

Figure 3.12 – Creating a temporary view from a Spark dataframe

Now that we have created a view of the data, we can query it using SQL commands.

+   3. We will select everything, and then we will filter using a WHERE clause to get all the results from the state of Arizona, as illustrated in the following screenshot: