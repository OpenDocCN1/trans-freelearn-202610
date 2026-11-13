# # Min

The method min() computes the smallest value in the RDD. A comparator is passed as an argument to the method min() to decide upon the comparison of elements in the RDD:

# Java 7:

```
//min()
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,2,3));
Integer minVal=intRDD.min(new IntegerComparator));
System.out.println("The min value of RDD is "+minVal);
//Also create a Comparator class as below
static class integerComparator implements Comparator<Integer>,Serializable{
private static final long serialVersionUID = 1L;
@Override
public int compare(Integer a, Integer b) {
return a.compareTo(b);
}
}
```

# Java 8:

```
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,2,3));
Integer minVal=intRDD.min(Comparator.naturalOrder()));
System.out.println("The min value of RDD is "+minVal);
```