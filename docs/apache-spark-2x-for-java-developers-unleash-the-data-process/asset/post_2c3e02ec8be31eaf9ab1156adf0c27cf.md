

| /applications/[app-id]/stages/[stage-id] | To fetch details of a specific stage executed within a Spark application. | The ?status=[active|complete|pending|failed] is used to list the stages which requested status. |
| --- | --- | --- |
| /applications/[app-id]/stages/[stage-id]/[stage-attempt-id] | To fetch details of a specific attempt of a particular stage. | |
| /applications/[app-id]/stages/[stage-id]/[stage-attempt-id]/taskSummary | To fetch the summary metrics of all tasks executed in a specific attempt of a particular stage. | The ?quantiles is used to summarize the metrics with the given quantiles. Example: ?quantiles=0.01,0.5,0.99. |
| /applications/[app-id]/stages/[stage-id]/[stage-attempt-id]/taskList | To list all the tasks of a specific attempt of a particular stage. | The ?offset=[offset]&length=[len] is used to list tasks in the given offset range. The ?sortBy=[runtime|-runtime] is used to sort the tasks by given value. |
| /applications/[app-id]/executors | To list all executors of a Spark application. | |
| /applications/[app-id]/storage/rdd | To list all the stored RDDs of a Spark application. | |
| /applications/[app-id]/storage/rdd/[rdd-id] | To fetch the details about the status of a particular stored RDD. | |
| /applications/[base-app-id]/logs | To download the event logs for all the attempts of a particular application. It will be in the form of a ZIP file. | |
| /applications/[base-app-id]/[attempt-id]/logs | To download the event logs for a particular attempt of an application. It will be in the form of a ZIP file. | |

In the next section, we will learn about various configurations that can help in the performance tuning of Spark applications.

# Summary

In this chapter, we learned how to set up an Apache Spark standalone cluster and interact with it using Spark CLI. We then focused on becoming familiar with various Spark components and how a Spark job gets executed in a clustered environment. Different Spark job configurations and Spark web UI were also discussed, along with REST API usage for job submission and status monitoring.

In the next chapter, we will discuss more on RDD and its operations.

# Understanding the Spark Programming Model

This chapter will progress by explaining the word count problem in Apache Spark using Java and simultaneously setting up an IDE. Then we will progress towards explaining common RDD actions and transformations. We will also touch upon the inbuilt capability of Spark caching and the persistence of RDD for improving Spark job performance.

# Hello Spark

In this section, we will create an Hello World program for Spark and will then get some understanding of the internals of Spark. The Hello World program in the big data world is also known as a Word Count program. Given the text data as input, we will calculate the frequency of each word or number of occurrences of each word in the text data, that is, how many times each word has appeared in the text. Consider that we have the following text data:

Where there is a will there is a way

The number of occurrences of each word in this data is:

| Word | Frequency |
| :--- | :--- |
| Where | 1 |
| There | 2 |
| Is | 2 |
| A | 2 |
| Will | 1 |
| Way | 1 |

Now we will solve this problem with Spark. So let's develop a Spark WordCount application.

## Prerequisites

The following are the prerequisites for preparing the Spark application:

- IDE for Java applications: We will be using the Eclipse IDE to develop the Spark application. Users can use any IDE as per their choice. Also, the IDE should have the Maven plugin installed (the latest version of Eclipse such as Mars and Neon consists of inbuilt Maven plugins).
- Java 7 or above, but preferably Java 8.
- The Maven package.

The following are the steps to get started with the Maven project in Java IDE:

- 1. The first step is to create a Maven project. In Eclipse, it can be created as File|New|Project.
- 2. Then search for Maven, as shown in the following screenshot:

![](img/a77bd9f099223ea139d6d612f2b90712_168_0.png)

- 3. Select Maven Project and click Next. On the next screen select the checkbox next to Create a simple project (skip the archetype selection) and click Next. On the next screen, add the Group Id and Artifact Id values as follows and then click Finish:

![](img/a77bd9f099223ea139d6d612f2b90712_169_0.png)

- 4. Eclipse will create a Maven project for you. Maven projects can also be created using the mvn command line tool as follows:

```
mvn archetype:generate -DgroupId=com.spark.examples -DartifactId=Apache1
```

> Once the project is created using the mvn command line tool, it needs to be imported to the IDE.

- 5. After creating a Maven project, expand the Maven project and edit pom.xml by adding the following dependency:

```
<dependency>
    <groupId>org.apache.spark</groupId>
    <artifactId>spark-core_2.11</artifactId>
    <version>2.0.0</version>
</dependency>
```

- 6. The following are the contents of the pom.xml file:

![](img/a77bd9f099223ea139d6d612f2b90712_170_0.png)

The following are some useful tips about compilation of the Maven project:

- The Maven compiler plugin is added, which helps to set the Java version for compiling the project. It is currently set to 1.8 so Java 8 features can be used. The Java version can be changed as per the requirement.
- Hadoop is not a prerequisite to be installed in your system to run Spark jobs. However, for certain scenarios one might get an error such as Failed to locate the winutils binary in the hadoop binary path as it searches for a binary file called winutils in the bin folder of HADOOP_HOME.

The following steps may be required to fix such an error:

- 1. Download the winutils executable from any repository.
- 2. Create a dummy directory where you place the downloaded executable winutils.exe. For example, E:\SparkForJavaDev\Hadoop\bin.
- 3. Add the environment variable HADOOP_HOME that points to E:\SparkForJavaDev\Hadoop using either of the options given here: Eclipse | Your class which can be run as a Java application (containing the static main method) | Right click | Run as | Run Configurations | Environment Tab:

![](img/a77bd9f099223ea139d6d612f2b90712_171_0.png)

- 4. Add the following line of code in the Java main class itself:

```
System.setProperty("hadoop.home.dir", "PATH_OF_HADOOP_HOME");
```

The next step is to create our Java class for the Spark application. The following is the SparkWordCount application using Java 7:

```
public class SparkWordCount {
public static void main(String[] args) {
SparkConf conf = new SparkConf().setMaster("local").setAppName("WordCount");
JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
JavaRDD<String> inputData = javaSparkContext.textFile("path_of_input_file");
JavaPairRDD<String, Integer> flattenPairs = inputData.flatMapToPair(new PairFlatMapFunction<String, String, Integer>() {
@Override
public Iterator<Tuple2<String, Integer>> call(String text) throws
Exception {
    List<Tuple2<String,Integer>> tupleList =new ArrayList<>();
    String[] textArray = text.split(" ");
    for (String word:textArray) {
        tupleList.add(new Tuple2<String, Integer>(word, 1));
    }
    return tupleList.iterator();
}};
JavaPairRDD<String, Integer> wordCountRDD = flattenPairs.reduceByKey(new
Function2<Integer, Integer, Integer>() {
    @Override
    public Integer call(Integer v1, Integer v2) throws Exception {
        return v1+v2;
    }});
wordCountRDD.saveAsTextFile("path_of_output_file");
}
}
```

The following is the SparkWordCount application using Java 8:

```
public class SparkWordCount {
    public static void main(String[] args) {
        SparkConf conf =new
        SparkConf().setMaster("local").setAppName("WordCount");
        JavaSparkContext javaSparkContext = new JavaSparkContext(conf);
        JavaRDD<String> inputData =
        javaSparkContext.textFile("path_of_input_file");
        JavaPairRDD<String, Integer> flattenPairs =
        inputData.flatMapToPair(text -> Arrays.asList(text.split(" ")).stream().map(word ->new Tuple2<String,Integer>(word,1)).iterator());
        JavaPairRDD<String, Integer> wordCountRDD = flattenPairs.reduceByKey((v1,
        v2) -> v1+v2);
        wordCountRDD.saveAsTextFile("path_of_output_file");
    }
}
```

In the preceding example of the WordCount application, we have performed the following steps:

- 1. Initialize SparkContext: We started with creating a SparkConf object:

```
new SparkConf().setMaster("local").setAppName("WordCount");
```

Here, setMaster("local") defines that the application will run inside a single JVM; that is, we can execute it in the IDE itself. This is very useful for debugging Spark applications and to create test

