same seed value with return same result. It is very useful in replicating testing scenarios by keeping the sampled data the same:

```
```java
//takeSample()
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,4,3));
List<Integer> takeSamepTrueSeededList=intRDD.takeSample(true, 4,9);
for(Integer intVal:takeSamepTrueSeededList) {
System.out.println("The take sample vals with seed are : "+intVal);
}
```
```