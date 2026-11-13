# outerJoinVertices

Joining datasets is a very popular operation in analytics systems. The outerJoinVertices operation is used to join graphs with external RDDs. Joining graphs to external datasets can be really useful at times when some external properties need to be merged in the graph.

The following is the signature of the outerJoinVertices transformation:

```
| outerJoinVertices ( RDD <scala.Tuple2<Object,U>> other, scala.Function3<Object, VD,
```

Here, the first parameter is the dataset to join with. The vertex ID of the graph vertices is joined with the Tuple object/key elements. The second parameter is the user-defined join function. As the name of the operation suggests, it works as an outer join operation. Therefore, the join function takes a scala.Option option that will be empty if there is no match found for the vertex ID in the external dataset.

The remaining parameters are Scala objects, classTag, and eq; we have already defined them in the previous section. Let's define the external RDD. The vertex property is the graph defined in the preceding sections as random names. Here we will define an RDD that will have last names for those names:

```
List<Tuple2<Object, string>> dataToJoin = new ArrayList<>();
dataToJoin.add(new Tuple2<Object, string>(11,"Wilson"));
dataToJoin.add(new Tuple2<Object, string>(21, "Harmon"));
dataToJoin.add(new Tuple2<Object, string>(31,"Johnson"));
dataToJoin.add(new Tuple2<Object, string>(41, "Peterson"));
dataToJoin.add(new Tuple2<Object, string>(51, "Adams"));
JavaRDD<Tuple2<Object, string>> dataToJoinRdd = javaSparkContext.parallelize(dataTo
```

The following is the user-defined function defining the operation executed with the outer join of vertices:

```
public class AbsFunc1 extends AbstractFunction3<Object, string, Option<string>, str.
@Override
public string apply(Object o, string s1, Option<string> s2) {
if (s2.isEmpty()) {
return s1 ;
```