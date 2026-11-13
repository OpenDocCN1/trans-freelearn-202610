executed per worker node.

When the client submits Spark Application requests to Spark Master, Spark driver starts in the client program or a separate JVM of the worker nodes (depends upon the deploy mode of the application). After the Driver of the application is started, it contacts spark master with the required cores for the application. Then, Spark master will start executors for the applications that register with the driver. After which, the Driver submits the physical plan of execution to executors to process. Spark master generally starts one executor each on every worker node to load balance the application processing.

Spark distribution provides a Binary spark-submit, which is used to deploy Spark applications on every cluster manager.

Spark application on standalone cluster can be deployed in the following two modes: