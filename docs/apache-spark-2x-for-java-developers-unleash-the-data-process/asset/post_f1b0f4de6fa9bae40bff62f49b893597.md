
writes them in a circular buffer.

![](img/a77bd9f099223ea139d6d612f2b90712_56_0.png)

- 3. Shuffle and Sort Phase: Considering that circular buffers have a size constraint, after a certain percentage, the default being 80, a thread gets spawned which spills the data from the buffer. But, before copying the spilled data to disk, it is first partitioned with respect to its reducer and then the background thread also sorts the partitioned data on a key and if the combiner is mentioned it then combines the data too. This process optimizes the data once it is copied to its respective partitioned folder. This process is continued until all the data from circular buffer gets written to disk. A background thread again checks if the number of spilled files in each partition is within the range of the configurable parameter or else the files are merged and the combiner is run over them until it falls within the limit of the parameter.

A Map task keeps updating the status to ApplicationMaster for its entire life cycle. It is only when 5 percent of a Map task has been completed that the Reduce task starts. An auxiliary service in the NodeManager serving the Reduce task starts a Netty web server that makes a request to MRAppMaster for Mapper hosts having specific Mapper partitioned files. All the partitioned files that pertain to the Reducer are copied to their respective nodes in a similar fashion. Since multiple files get copied as data from various nodes representing that Reduce nodes gets collected, a background thread merges the sorted map file and again sorts them and if the combiner is configured, then combines the result too.

- 4. Reduce Stage: It is important to note here that at this stage every input file of each reducer should have been sorted by key. This is the presumption with which the reducer starts processing these records and converts the key value pair into an aggregated list. Once the reducer has processed the data, it writes them to the output folder as was mentioned during the job submission.

- 5. Clean-up Stage: Each reducer sends a periodic update to MRAppMaster about the task completion. Once the Reduce task is over, the ApplicationMaster starts the clean-up activity. The submitted job status is changed from running to successful, and all the temporary and intermediate files and folders are deleted. The application statistics are archived to a job history server.

## Why Apache Spark?

MapReduce is on its way to being a legacy. We've got Spark, says the man behind Apache Hadoop, Doug Cutting. MapReduce is an amazingly popular distributed framework but it comes with its fair amount of criticism as well. Since its inception, MapReduce has been built to run jobs in batch mode. Although it supports streaming, it's not very well suited for ad-hoc queries, machine learning, and so on. Apache Spark is a distributed in-memory computing framework, and somehow tries to address some of the major concerns that surround MapReduce:

- **Performance**: A major bottleneck in MapReduce jobs are disk I/Os, and it is considerably visible during the shuffle and sort phase of MR, as data is written to disk. The guiding principle that Spark follows is simple: share the memory across the cluster and keep everything in memory as long as possible. This greatly enhances the performance of Spark jobs to the tune of 100X when compared to MR (as claimed by their developers).

- **Fault tolerance**: Both MR and Spark have different approaches in handling fault tolerance. The AM keeps a track of mappers and reducers while executing MR jobs. As and when these containers stop responding or fail upfront, the AM after requesting the RM, launches a separate JVM to run such tasks. While this approach achieves fault tolerance, it is both time and resource consuming. Apache Spark's approach of handling fault tolerance is different; it uses Resilient Distributed Datasets (RDD), a read-only fault-tolerant parallel collection. RDD maintains the lineage graph so that whenever its partition gets lost it recovers the lost data by re-computing from the previous stage and thus making it more resilient.

- **DAG**: Chaining of MapReduce jobs is a difficult task and along with that it also has to deal with the burden of writing intermediate results on HDFS before the next job starts its execution. Spark is actually a **Directed Acyclic Graph (DAG)** engine, so chaining any number of jobs can easily be achieved. All the intermediate results are shared across memory, avoiding multiple disk I/O. Also these jobs are lazily evaluated and hence only those paths are processed which are explicitly called for computation. In Spark such triggers are called *actions*.

- **Data processing**: Spark (aka Spark Core) is not an isolated distributed compute framework. A whole lot of Spark modules have been built around Spark Core to make it more general purpose. RDD forms the main abstraction in all these modules. With recent development dataframe and dataset have also been developed, which enriches RDD by providing it a schema and type safety. Nevertheless, the universality of RDD is ubiquitous across all the modules of Spark making it simple to use and it can easily be cross-referenced in different modules. Whether it is streaming, querying capability, machine learning, or graph processing, the same data can be referenced by RDD and can be interchangeably used. This is a unique appeal of RDD, which is lacking in MR, as different concept was required to handle machine learning jobs in **Apache Mahout** than was required in **Apache Giraph**.

- **Compatibility**: Spark has not been developed to keep only the YARN cluster in mind. It has amazing compatibility to run on Hadoop, Mesos, and even a standalone cluster mode. Similarly, Spark has not been built around HDFS and has a wide variety of acceptability as far as different filesystems are concerned. All Apache Spark does is provide compute capabilities while leaving the choice of choosing the cluster and filesystem to the use case being worked upon.

- **Spark APIs**: Spark APIs have wide coverage as far as functionality and programming languages are concerned. Spark's APIs have huge similarities with the Scala collection, the language in which Apache Spark has been majorly implemented. It is this richness of functional programming that makes Apache spark avoid much of the boilerplate code that eclipsed MR jobs. Unlike MR, which dealt with low-level programming, Spark exposes APIs at a higher abstraction level with a scope of overriding any bare metal code if ever required.

## RDD - the first citizen of Spark

*The very first paper on RDD Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing described it as follows:*

**Resilient Distributed Datasets (RDDs)**, a distributed memory abstraction that lets programmers perform in-memory computations on large clusters in a fault-tolerant manner. As Spark is written in a functional programming paradigm, one of the key concepts of functional programming is immutable objects. Resilient Distributed Dataset is also an immutable dataset.

Formally, we can define an RDD as an immutable distributed collection of objects. It is the primary data type of Spark. It leverages cluster memory and is partitioned across the cluster.

The following is the logical representation of RDD:

![](img/a77bd9f099223ea139d6d612f2b90712_60_0.png)

RDDs can consist of (key, value) pairs as well. The following is the logical representation of pair of RDDs:

![](img/a77bd9f099223ea139d6d612f2b90712_61_0.png)

Also, as mentioned, RDD can be partitioned across the cluster. So the following is the logical representation of partitioned RDDs in a cluster:

![](img/a77bd9f099223ea139d6d612f2b90712_61_1.png)

### Operations on RDD

An RDD supports only two types of operations. One is called transformation and the other is called action. The following are the explanations of both of these:

- **Transformation**: If an operation on an RDD gives you another RDD, then it is a transformation. Consider you have an RDD of strings and want to filter out all values that start with H as follows:

![](img/a77bd9f099223ea139d6d612f2b90712_62_0.png)

So, a filter operation on an RDD will return another RDD with all the values that passes through the filter condition. So, a filter is an example of a transformation

- **Action**: If an operation on an RDD gives you a result other than an RDD, it is called an action: for example, the sum of all values in an RDD, or the count of all the values or retrieving all values of RDD in form of a list, and so on. The following is the logical representation of an action sum of an RDD:
