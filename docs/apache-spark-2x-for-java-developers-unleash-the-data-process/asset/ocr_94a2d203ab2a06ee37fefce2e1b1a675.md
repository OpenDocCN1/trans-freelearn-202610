# # takeOrdered

Elements of the RDD can be returned based on natural or custom ordering of the dataset using the method takeOrdered(). An example for fetching the elements based on natural ordering of the elements is as follows:

```
//TakeOrdered()
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,4,3));
List<Integer> takeOrderedTwo= intRDD.takeOrdered(2);
for(Integer intVal:takeOrderedTwo){
System.out.println("The take ordered elements are :: "+intVal);
}
```

Similarly, if we are to fetch elements of the RDD based on custom ordering then we will have to implement and pass a custom comparator as an argument to the takeOrdered() method:

# ### Java 7:

```
//TakeOrdered
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,4,3));
List<Integer> takeCustomOrderedTwo= intRDD.takeOrdered(2, new
integerReverseComparator());
for(Integer intVal:takeCustomOrderedTwo){
System.out.println("The take custom ordered elements are :: "+intVal);
}
//Also create a Comparator class as below
static class integerReverseComparator implements
Comparator<Integer>,Serializable {
private static final long serialVersionUID = 1L;
@Override
public int compare(Integer a, Integer b) {
return b.compareTo(a);
}
}
```

# ### Java 8:

```
JavaRDD<Integer> intRDD = sparkContext.parallelize(Arrays.asList(1,4,3));
List<Integer> takeCustomOrderedTwo= intRDD.takeOrdered(2,
Comparator.reverseOrder());
for(Integer intVal:takeCustomOrderedTwo) {
System.out.println("The take custom ordered elements are :: "+intVal);
}
```