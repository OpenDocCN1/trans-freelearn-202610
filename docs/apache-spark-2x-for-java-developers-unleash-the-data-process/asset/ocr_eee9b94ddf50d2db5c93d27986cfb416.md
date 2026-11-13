# Deploying applications on Spark standalone cluster

In the previous section, we learned how to start and stop Spark standalone cluster. Once the Spark standalone cluster is running, Spark applications can be deployed on the cluster. Before deploying the application on the cluster, let's first discuss how applications components run in Spark Standalone mode:

![](bbox=[0.23, 0.36, 0.76, 0.64])

Logical Representation of Spark Application on Standalone Cluster

The preceding figure shows a Spark standalone cluster of three nodes. Spark master is running on one node and workers on two other nodes. A Spark application is deployed on the cluster. Driver JVMs is running on one of the worker nodes. The application consists of two Executor JVMs running on each worker node. Executors JVMs show an RDD with two partitions cached in the memory and in each Executor four tasks are running, which execute the actual business logic of the application.

In Spark standalone mode, a maximum of one executor per application can be