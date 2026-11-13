# reverse

reverse() transformation is used to reverse the direction of edges in the graph. This operation just reverses the source and destination indices of the edges without updating any vertex and edge properties. It can be executed as follows:

```
| Graph<string, string> reversedGraph = graph.reverse();
```

To verify, print the edgetriplet:

```
| reversedGraph.triplets().toJavaRDD().collect().forEach(System.out::println);
```