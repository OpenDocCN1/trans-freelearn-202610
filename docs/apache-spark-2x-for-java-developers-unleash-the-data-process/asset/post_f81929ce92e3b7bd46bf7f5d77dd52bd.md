
## Dataframe and dataset

Dataframes were introduced in Spark 1.3. Dataframe built on the concept of providing schemas over the data. An RDD basically consists of raw data. Although it provides various functions to process the data, it is a collection of Java objects and is involved in the overhead of garbage collection and serialization. Also, Spark SQL concepts can only be leveraged if it contains some schema. So, an earlier version of Spark provided another version of RDD called SchemaRDD.

### SchemaRDD

As its name suggests, it is an RDD with schema. As it contains schema, relational queries can be run on the data along with basic RDD functions. The SchemaRDD can be registered as a table so that SQL queries can be executed on it using Spark SQL. It was available in an earlier version of Spark. However, with Spark Version 1.3, the SchemaRDD was deprecated and dataframe was introduced.

### Dataframe

In spite of being an evolved version of SchemaRDD, dataframe comes with big differences to RDDs. It was introduced as part of Project Tungsten that allows data to be stored off-heap in binary format. This helps in unloading the garbage collection overhead.

A dataframe comes with the concept of always having schema with data (the Spark framework manages the schema and only passes data between the nodes). As Spark understands the schema and data is stored off-heap in binary format, there is no need to encode data using Java (or Kryo) serialization, which reduces the serialization effort as well.

Also, the dataframe API introduced the concept of building optimized query plans that can be executed with the Spark Catalyst optimizer. As dataframe, like RDD, also follows lazy evaluation, it allows the creation of an optimized query plan before execution. Finally, the operation happens on RDDs only, but it is transparent to the user. Consider the following query:

```
select a.name, b.title from a, b where a.id = b.id where b.title = 'Architect'
```

Here, instead of joining tables and then filtering the data based on title, the data will be first filtered and then joined as per the optimized query plan. This helps to reduce network shuffle.

With all this advancement, dataframe has some shortcomings. Dataframe does not provide compile-time safety. Consider the following example of dataframe from Spark 1.6:

```
JavaRDD<Employee> empRDD = jsc.parallelize(Arrays.asList(new Employee("Foo", 1), new
SQLContext sqlContext = new SQLContext(jsc));

DataFrame df = sqlContext.createDataFrame(empRDD, Employee.class);
```

Here we created a dataframe of Employee objects using sqlContext. The constructor of Employee objects accepts two parameters: name, ID.

JavaBean should contain a default constructor and should be serializable to be used as elements for RDD, dataframe, or dataset.

Notice that DataFrame does not contain any type. To filter elements, a filter operation can be executed on DataFrame as follows:

```
DataFrame filter = df.filter("id > 1");
```

Here, the compiler has no option to identify whether the column name specified in the filter condition exists or not. If it does not, it will throw exceptions at runtime.

Another disadvantage of dataframe is that it was Scala-centric. All APIs are not available in Java or Python.

### Dataset

Dataset was introduced in Spark 1.6. It is the combination of RDD and dataframe. Dataset brings compile-time safety, the object-oriented programming style of RDD, and the advances of dataframes together. Therefore, it is an immutable strongly typed object which uses schema to describe the data. It uses the efficient off-heap storage mechanism, Tungsten, and creates optimized query plans that get executed with Spark Catalyst optimizer.

Datasets also introduced the concept of encoders. Encoders work as translators among JVM objects and Spark internal binary format. The tabular representation of data with schema is stored in Spark binary format. Encoders allow operations on serialized data. Spark comes with various inbuilt encoders, along with an encoder API for JavaBean. Encoders allow the access of individual attributes without the need to de-serialize an entire object. Thus, it reduces serialization efforts and load.

### Creating a dataset using encoders

A dataset can be created using empRDD as follows:

```
Dataset<Employee> dsEmp = sparkSession.createDataset(empRDD.rdd(), org.apache.spa
```

Notice, a dataset is a strongly typed object. We've created a dataset object which contains elements of Employee.

We can run the following operation on dataset to filter employees with ID > 1:

```
Dataset<Employee> filter = dsEmp.filter(emp -> emp.getId() > 1);
filter.show();
```

Another advantage of a dataset is that it provides both Scala and Java APIs, which allows Java engineers to work efficiently with datasets.

### Creating a dataset using StructType

In the previous section, we discussed the creation of a dataset from RDD with the help of encoders. Similarly in Chapter 5, Working with Data and Storage, we also discussed a couple of different ways in which we can create dataframes, an alias for dataset (row), from JSON, CSV, and XML files. There is another way in which we can dynamically create a schema and hence form an object of dataset (row). The first step involves converting RDD(T) to RDD(row) using the factory pattern method of class RowFactory. Then we identify the number of fields in the data and create a schema of StructType representing the structure of RDD(row) and finally apply a schema to the createDataFrame method in SparkSession.

As an example, let's assume we have a text file whose first row represents the name of the column and the rest of the rows represents data:

```
//Create a RDD
JavaRDD<String> deptRDD = sparkSession.sparkContext()
    .textFile("src/main/resources/dept.txt", 1)
    .toJavaRDD();

//Convert the RDD to RDD<Rows>
JavaRDD<Row> deptRows = deptRDD.filter(str -> !str.contains("deptno")).map(new Funct.
private static final long serialVersionUID = 1L;
@Override
public Row call(String rowString) throws Exception {
    String[] cols = rowString.split(",");
    return RowFactory.create(cols[0].trim(), cols[1].trim(), cols[2].trim());
});

//Create schema
String[] schemaArr = deptRDD.first().split(",");
List<StructField> structFieldList = new ArrayList<>();
for (String fieldName : schemaArr) {
    StructField structField = DataTypes.createStructField(fieldName, DataTypes.String,
    structFieldList.add(structField);
}
StructType schema = DataTypes.createStructType(structFieldList);

Dataset<Row> deptDf = sparkSession.createDataFrame(deptRows, schema);
deptDf.printSchema();
deptDf.show();
```

### Unified dataframe and dataset API

With the evolution of Spark 2.x, the untyped characteristics of dataframe and the typed characteristics of dataset have been merged into dataset. So, fundamentally a dataframe is an alias of a collection of generic objects of a dataset (row), where row is a generic untyped JVM object. In Java, a dataframe is represented as a dataset (row). The unification of dataframe with dataset allows us to operate upon the data in multiple ways such as:

- Using strongly typed APIs: Java functions provide type safety while working on dataset, also referred as a typed transformation:

```
dsEmp.filter(newFilterFunction<Employee>() {
@Override
public boolean call(Employee emp) throws Exception {
return emp.getEmpId() > 1;
}
}).show();
//Using Java 8
dsEmp.filter(emp -> emp.getEmpId() > 1).show();
```

- Using untyped APIs: An untyped transformation provides SQL-like column specific names, and the operation can be used to operate upon data:

```
dsEmp.filter("empID > 1").show();
```

- Using DSL: Dataframes/dataset provide a domain-specific language for structured data manipulation. In Java, one can use the function col() by importing the Spark SQL package org.apache.spark.sql.functions.col which provides a statically typed function on untyped column names of a dataframe.

```
dsEmp.filter(col("empId").gt(1)).show();
```
