
![](img/a77bd9f099223ea139d6d612f2b90712_350_0.png)

Figure 8.1: Unified dataset API Spark 2.x

## Data persistence

By default, data is persisted using Parquet file format in Spark unless the configuration spark.sql.sources.default is set to any other format. Fully qualified file format names should be used as a data source except for file formats (JSON, Parquet, JDBC, ORC, LIBSVM, CSV, and text) which are natively supported as of Spark 2.1. Though native file formats have built-in support with datasets, some limitations exist, such as: for saving the data in text file format only single column is supported; similarly for saving ORC file format, Hive support should be enabled while creating SparkSession. Data can be persisted using either of the options provided by a dataset.

```
deptDf.write().mode(SaveMode.Overwrite).json("DirectoryLocation");
deptDf.write().mode("overwrite").format("csv").save("src/main/resources/output/dept");
deptDf.write().mode(SaveMode.Overwrite).format("csv").save("DirectoryLocation");
```

Data persistence mode is a very important aspect of how data is saved on the disk if existing data already exists. SaveMode provides four different options to handle such scenarios, default being error which throws an exception if the data is already present, as shown in the following table:

| SaveMode | Any language | Meaning |
|---|---|---|
| SaveMode.ErrorIfExists | error | When saving a dataframe to a data source, if data already exists, an exception is expected to be thrown. |
| SaveMode.Append | append | When saving a dataframe to a data source, if data/table already exists, the contents of the dataframe are expected to be appended to existing data. |
| SaveMode.Overwrite | overwrite | When saving a dataframe to a data source, if a data/table already exists, the existing data is expected to be overwritten by the contents of the dataframe. |
| SaveMode.Ignore | ignore | When saving a dataframe to a data source, if data already exists, the save operation is expected to not save the contents of the dataframe and to not change the existing data. This is similar to a CREATE TABLE IF NOT EXISTS in SQL. |

## Spark SQL operations

Working in Spark SQL primarily happens in three stages: the creation of dataset, applying SQL operations, and finally persisting the dataset. We have so far been able to create a dataset from RDD and other data sources (refer to Chapter 5, Working with Data and Storage) and also persist the dataset as discussed in the previous section. Now let's look at some of the ways in which SQL operations can be applied to a dataset.

### Untyped dataset operation

Once we have created the dataset, then Spark provides a couple of handy functions which perform basic SQL operation and analysis, such as the following:

- show(): This displays the top 20 rows of the dataset in a tabular form. Strings of more than 20 characters will be truncated, and all cells will be aligned right:

```
emp_ds.show();
```

Another variant of the show() function allows the user to enable or disable the 20 characters limit in the show() function by passing a Boolean as false to disable truncation of the string:

```
emp_ds.show(false);
```

- printSchema(): This function prints the schema to the console in a tree format:

```
emp_ds.printSchema();
```

- select(): This selects a set of columns as passed as an argument from the dataset:

```
emp_ds.select("empName", "empId").show();
```

By statically importing the function org.apache.spark.sql.functions.col, one can add extra features to columns by using helper functions associated with col(), such as creating an alias, incrementing the value of the column, casting to a specific datatype, and so on:

```
emp_ds.select(col("empName").name("Employee Name") ,col("empId").cast(Di
```

- filter(): This function is used to select rows meeting the criteria passed as an argument to the `filter()` function:

```
emp_ds.sort(col("empId").asc()).filter(col("salary").gt("2500"));
```

- groupBy(): This groups the dataset using the specified columns, so that one can run aggregation on them:

```
emp_ds.select("job").groupBy(col("job")).count().show();
```

- join(): This joins with another dataframe using the given join expression. The function accepts three parameters: the first being another dataset; the second being the column on which the data will be joined; and the third being joinType--inner, outer, left_outer, right_outer, and leftsemi. The following performs a left-outer join between emp_ds and deptDf:

```
emp_ds.as("A").join(deptDf.as("B"),emp_ds.col("deptno").equalTo(deptDf.col('
```

### Temporary view

Spark SQL provides all the capabilities of processing a SQL query by using a temporary view. All the data and metadata information of a temporary view is dropped once the session or application ends. The result of queries in a temporary view is returned as Dataset<Row>.

```
deptDF.createOrReplaceTempView("dept");
Dataset<Row> result = sparkSession.sql("select dname,loc from dept where deptno > 2");
result.show();
```

Temporary views are accessible within the same session and will throw an error if accessed from another session. The following example will throw an exception Table or view not found: dept, since we are trying to access the temporary view from a new session.

```
deptDF.createOrReplaceTempView("dept");
Dataset<Row> result = sparkSession.sql("select loc,count(loc) from dept where deptn
```

In the following section we will discuss how we can avoid such an error and also share data across sessions using a global temporary view.

### Global temporary view

Global temporary views are primarily used to share within a session and then get automatically dropped once the session ends. These session-scoped views also serve as a temporary table on which SQL queries can be made and are stored in database global_temp. The database name can be changed by modifying the SQL configuration spark.sql.globalTempDatabase. However, qualified names should be used to access GlobalTempView(database.tableName) or else it throws an error Table or view not found:

```
deptDf.createGlobalTempView("dept_global_view");
sparkSession.newSession().sql("SELECT deptno,dname,loc, rank() OVER (PARTITION BY 1;
```

While using Thrift Server with a multi-session (spark.sql.hive.thriftServer.singleSession = false), a global temporary view can be used to access data from a different session. Prior to Spark 2.1, the only way to share data across sessions was to persist the data and then access it using catalog.

### Spark UDF

The UDF is a special way of enhancing the features of SQL in Spark SQL. It operates on a single row and returns a single output value corresponding to the single row. Some of the most common UDFs used are concatenating a string, formatting dates, changing case of string values, and so on. Apart from default UDFs, one can create custom UDFs and register them in Spark SQL with an alias. The alias can then be used as standard function in SQL queries.

Some of the points that need to be taken care of while writing a UDF include:

- In Java, depending on the number of input parameters, the UDF class implements changes so that the UDF having only one input parameter implements UDF1, with two input parameters the class changes to UDF2, and so on
- One can opt for arrays or struct as input parameters to avoid the limitation of having a maximum 22 input parameters
- Once the UDF class has been written and instantiated, it registers the instance in Spark SQL session
- Registered UDFs cannot be used with the **Domain Specific Language** (**DSL**) of a dataframe. The query optimization engine catalyst treats UDFs as a black box without offering any optimization

As an example, let's create a UDF that calculates the number of days between today and the date passed:

```
public class CalcDaysUDF implements UDF2<String,String, Long> {
private static final long serialVersionUID = 1L;
@Override
public Long call(String dateString,String format) throws Exception {
SimpleDateFormat myFormat = new SimpleDateFormat(format);
Date date1 = myFormat.parse(dateString);
Date date2 = new Date();
long diff = date2.getTime() - date1.getTime();
return TimeUnit.DAYS.convert(diff, TimeUnit.MILLISECONDS);
}
}
```

Now let's call this class in a SQL query:
