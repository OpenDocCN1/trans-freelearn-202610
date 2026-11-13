# Streaming

As you may have guessed, the Streaming tab shows information about Spark Streaming jobs. This tab appears when you execute a streaming job in SparkContext.

We will learn about the working of Spark Streaming framework in Chapter 9, Near Real-Time Processing with Spark Streaming. For now, let us run the following job that will listen to port 10000 and run word count on the incoming messages.

First, open a connection on port 10000:

```
| sparkuser@~$ nc -lk 10000
```

Now, execute a NetworkWordCount example bundled in Spark Package:

```
| sparkuser@~$ $SPARK_HOME/bin/run-example streaming.NetworkWordCount localhost 10000
```

When you run it, the Streaming tab appears and it can be accessed at http://localhost:4040/streaming/: