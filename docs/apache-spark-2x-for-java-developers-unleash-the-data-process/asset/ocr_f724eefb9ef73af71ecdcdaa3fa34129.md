by one can be executed as follows:

```
| Java 7:
intRDD .map( new Function<Integer, Integer>() {
@Override
public Integer call(Integer x ) throws Exception {
return x + 1;
}
});
Java 8:
intRDD.map(x -> x + 1);
```