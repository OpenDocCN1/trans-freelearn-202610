# Finding the sum of all even numbers in an RDD of integers

Let's first create an RDD of integers as follows:

```
|scala>valintRDD = sc.parallelize(Array(1,4,5,6,7,10,15))
|intRDD: org.apache.spark.rdd.RDD[Int] = ParallelCollectionRDD[0] at parallelize at .
```

The next step is to filter all the even elements in this RDD. So, we will execute a filter operation on the RDD, as follows:

```
|scala>valevenNumbersRDD=intRDD.filter(i => (i%2==0))
|evenNumbersRDD: org.apache.spark.rdd.RDD[Int] = MapPartitionsRDD[3] at filter at <c
```

The preceding operation will fetch those elements for which 2 is a factor, that is, even elements .

Now, we will sum all the elements of this RDD as follows:

```
|scala>val sum =evenNumbersRDD.sum
|sum: Double = 20.0
|scala> sum
|res5: Double = 20.0
```

It is shown in the following screenshot:

![](bbox=[0.113, 0.649, 0.884, 0.843])