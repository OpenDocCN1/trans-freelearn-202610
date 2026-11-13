# # Useful job configurations

In this section, we will discuss some useful job configurations that can be configured while submitting the job:

+   • --dirs: This parameter is used to provide external jar files that the Spark application is referring. If the user has not created uber jar of the application, external libraries can be provided to the job using this parameter:

```
| | . /bin/spark-submit --master spark://spark-master:7077 -jars /path/of/external
```

+   • --driver-memory: This parameter is used to specify the memory required for spark driver:

```
| | . /bin/spark-submit --master spark://spark-master:7077 -driver-memory 1G --de
```

+   • --executor-memory: This parameter is used to specify the memory required for each executor:

```
| | . /bin/spark-submit --master spark://spark-master:7077 -executor-memory 1G --
```

+   • --total-executor-cores: This parameter is used to specify the total number of cores needed for an application:

![](bbox=[0.143, 0.664, 0.214, 0.732])

In Spark standalone, there is no option to specify the total number of executors for an application. Spark master generally runs each executor per worker node to balance load of processing for an application.

```
| | . /bin/spark-submit --master spark://spark-master:7077 --total-executor-cores
```

+   • --supervise: When supervise configuration is provided, Spark master will supervise Spark Driver and if the Driver dies, it restarts the driver:

![](bbox=[0.143, 0.862, 0.214, 0.912])

If Spark driver is restarted, then all of the Spark executors also need to be restarted. In other words, all Spark applications are