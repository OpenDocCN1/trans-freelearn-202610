# # First

The first method returns the first element of the RDD:

```
//first()
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,2,3));
Integer first=intRDD.first();
System.out.println("The first value of RDD is "+first);
```