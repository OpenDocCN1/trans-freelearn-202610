# # Approximate actions

In the previous chapter, we came across different methods in which RDD could be sampled to give a randomized output. This works fine as long as we want to debug or test our application. However, in other scenarios we might not want to get results which are accurate but which take a long time to execute, but rather require an approximate result within a certain percentage of error and in a time-bound manner. Spark has introduced approximate algorithms to cater to such needs, where the job can guarantee a result within a stipulated timeframe or/and within an error percentage. Some of the actions are:

+   * countApprox: Returns the approximate value of an RDD within a stipulated time, whether or not the job has completed or not. Usually if timeout (in milliseconds) is less, the error factor is higher.

```
// countApprox(long timeout)
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,4,3,5,7));
PartialResult<BoundedDouble> countAprx = intRDD.countApprox(2);
System.out.println("Confidence ::"+countAprx.getFinalValue().confidence());
System.out.println("high ::"+countAprx.getFinalValue().high());
System.out.println("Low ::"+countAprx.getFinalValue().low());
System.out.println("Mean ::"+countAprx.getFinalValue().mean());
System.out.println("Final ::"+countAprx.getFinalValue().toString());
```

There is another overridden version of countApprox which also takes into consideration the confidence of the approximate result, where confidence is the probability of the result being true to that percentage point. If the countApprox method has a confidence of 0.7, then one can expect 70% of the results to contain the true count. Also, while the value of timeout is in milliseconds, the value of confidence lies in the range [0,1].

```
//countApprox(long timeout, double confidence)
PartialResult<BoundedDouble> countAprx = intRDD.countApprox(1, 0.95);
System.out.println("Confidence ::"+countAprx.getFinalValue().confidence());
System.out.println("high ::"+countAprx.getFinalValue().high());
System.out.println("Low ::"+countAprx.getFinalValue().low());
System.out.println("Mean ::"+countAprx.getFinalValue().mean());
System.out.println("Final ::"+countAprx.getFinalValue().toString());
```