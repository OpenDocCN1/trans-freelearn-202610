units as well. For running Spark applications on a Spark cluster setMaster() should point to the Spark Master URL. (Note: we will learn more about it in detail in Chapter 6, Spark On Cluster.)

setAppName("WordCount") helps to provide a name to our Spark application. In this case, we have named the application as WordCount. However, the user can provide any name to the application as per the requirement.

All other configuration parameters can be set using the SparkConf object. Then with the help of the SparkConf object we initialized the JavaSparkContext:

```
new JavaSparkContext(conf);
```

JavaSparkContext is specifically for Java Spark applications. As described in previous chapters, SparkContext is the mandatory component of a Spark application. Also, one Spark application can contain only one SparkContext. Now, with the help of SparkContext, we can create RDDs and can create a DAG of RDD transformations.

+   2. Load the input data: Using the javaSparkContext, we loaded the data from a text file:

```
javaSparkContext.textFile("path_of_input_file");
```

There are other ways of loading the data as well. However, in this case we used text file as input. It is always advisable to provide an absolute path as a relative path depends upon the location from which the program is executed.

Also, we can create an RDD by executing the parallelize() function on SparkContext as follows:

```
JavaRDD<String> data =javaSparkContext.parallelize(Arrays.asList("where ther
a will there is a way"));
```

+   3. Flatten the input data and create tuples: Once the data was loaded,