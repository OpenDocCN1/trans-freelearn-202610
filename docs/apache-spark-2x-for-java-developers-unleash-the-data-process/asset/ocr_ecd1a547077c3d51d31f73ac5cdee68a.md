Now here in the example, the first parameter creates a result container in the form of an empty StringBuilder object, the second parameter accumulates the next stream elements to the StringBuilder object and finally, the combiner combines the partial StringBuilder object into one:

```
<R> R collect(Collector<? super T,R> collector)
```

The collect() method also has a simplified method signature in which the role of accumulator, combiner, and result container generation can be encapsulated into a single abstraction called the Collector interface. The Collectors class is an implementation of the Collector interface which has abundant factory methods to perform various reduction operations.