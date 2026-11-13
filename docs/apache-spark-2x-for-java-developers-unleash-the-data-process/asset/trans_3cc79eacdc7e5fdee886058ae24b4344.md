

$eq$colon$eq<String, String> tpEquals = scala.Predef.$eq$colon$eq$.MODULE$.tpEquals

因此，可以按如下方式执行 mapVertices()：

```
Graph<String, String> mapVertices = graph.mapVertices(new AbsFunc1(), stringTag, tpl
```

要验证，可以按如下方式打印所有顶点：

```
mapVertices.vertices().toJavaRDD().collect().forEach(System.out::println);
```

mapVertices() 等同于对顶点 RDD 执行 map 操作，以获取转换后的顶点输出值，即：

```
JavaRDD<String> map = graph.vertices().toJavaRDD().map(x -> "Vertex:" + x);
```

但是请注意，它不会保留图索引或结构。它返回 JavaRDD，需要使用它来重新构建图。因此，推荐使用 mapVertices()。

## mapEdges

与 mapVertices 类似，mapEdges 用于转换边。mapEdges 的签名如下：

```
mapEdges(scala.Function1<Edge<ED>, ED2> map, scala.reflect.ClassTag<ED2> evidence$4)
```

先前创建的图对象的边具有字符串类型的属性。在此示例中，我们将把边属性转换为与字符串长度相等的整数值。转换函数的定义如下：

```
public class AbsFunc1 extends AbstractFunction1<Edge<String>, Integer> implements Serializable
@Override
public Integer apply(Edge<String> edge) {
return edge.attr().length();
}
```

因此，可以按如下方式执行 mapEdges：

```
graph.mapEdges(new AbsFunc1(), scala.reflect.ClassTag$.MODULE$.apply(Integer.class))
```

## mapTriplets

与 mapVertices 和 mapEdges 类似，mapTriplets 用于转换边三元组。该方法的签名如下：

```
mapTriplets(scala.Function1<EdgeTriplet<VD, ED>, ED2> map, scala.reflect.ClassTag<ED2>)
```

在此，我们将执行与 mapEdges 章节中相同的操作；即，将边属性转换为整数值。转换函数的定义如下：

```
public class AbsFunc1 extends AbstractFunction1<EdgeTriplet<String, String>, Integer>
@Override
public Integer apply(EdgeTriplet<String, String> triplet) {
  return triplet.attr().length();
}
```

mapTriplets 转换可以按如下方式执行：

```
graph.mapTriplets(new AbsFunc1(), scala.reflect.ClassTag$.MODULE$.apply(Integer.class))
```

## reverse

reverse() 转换用于反转图中边的方向。此操作仅反转边的源索引和目标索引，而不更新任何顶点和边属性。可以按如下方式执行：

```
Graph<String, String> reversedGraph = graph.reverse();
```

要验证，请打印三元组：

```
reversedGraph.triplets().toJavaRDD().collect().forEach(System.out::println);
```

## subgraph

顾名思义，子图操作返回图的一部分。此操作返回满足用户定义条件的顶点和边组成的图。在此示例中，我们将获取边属性为 Friend 的图的部分。

subgraph 方法的签名如下：

```
subgraph(scala.Function1<EdgeTriplet<VD, ED>, Object> epred, scala.Function2<Object, Object, Object> vpred)
```

第一个参数用于过滤边，第二个参数用于根据用户定义的条件过滤顶点。由于我们要过滤边属性为 Friend 的图，因此无需在第二个参数中指定任何过滤条件。两个参数的实现如下：

```
public class AbsFunc1 extends AbstractFunction1<EdgeTriplet<String, String>, Object>
@Override
public Object apply(EdgeTriplet<String, String> arg0) {
return arg0.attr().equals("Friend");
}
}

public class AbsFunc2 extends AbstractFunction2<Object, String, Object> implements Serializable
@Override
public Object apply(Object arg0, String arg1) {
return true;
}
}
```

因此，可以按如下方式执行子图操作：

```
Graph<String, String> subgraph = graph.subgraph(new AbsFunc1(), new AbsFunc2());
subgraph.triplets().toJavaRDD().collect().forEach(System.out::println);
```

前面操作在图上的逻辑输出如下：

![](img/a77bd9f099223ea139d6d612f2b90712_457_0.png)

子图操作输出的逻辑表示

## aggregateMessages

在基于图的计算中，汇总顶点数据非常常见。例如，查找用户在 Facebook 上的好友总数或查找用户在 Twitter 上的关注者总数。aggregateMessages 转换是 GraphX 的主要聚合函数。该方法的签名如下：

```
aggregateMessages(scala.Function1<EdgeContext<VD, ED, A>, scala.runtime.BoxedUnit>)
```

aggregateMessages 作为 MapReduce 函数工作。sendMessage 函数可视为 mapFunction，用于从源向目标或从目标向源发送消息。它以 EdgeContext 对象作为参数，该对象公开了在源顶点和目标顶点之间发送消息的方法。mergeMsg 函数可视为 reduce 函数，用于聚合使用 sendMessage 函数发送的消息。

Triplets 对象可用于指定 sendMsg 函数中使用的 EdgeContext 的哪一部分（例如源属性或目标属性），以优化行为。该参数的默认值为 TripletFields.All。

在此示例中，我们将统计一个顶点是多少条有向边的目标。这类似于统计用户在 Twitter 上有多少关注者。

sendMsg 函数的定义如下：

```
public class AbsFunc1 extends AbstractFunction1<EdgeContext<String, String, Integer>, BoxedUnit>
@Override
public BoxedUnit apply(EdgeContext<String, String, Integer> arg0) {
  arg0.sendToDst(1);
  return BoxedUnit.UNIT;
}
```

此处我们正在从源向目标顶点发送值为 1 的消息。mergeMsg 函数可定义如下：

```
public class AbsFunc1 extends scala.runtime.AbstractFunction2<Integer, Integer, Integer>
@Override
public Integer apply(Integer i1, Integer i2) {
  return i1 + i2;
}
```

在此函数中，我们正在对顶点接收到的所有消息求和。这可以被视为 MapReduce 中的单词计数程序。

使用上述用户定义的函数，可以按如下方式执行 aggregateMessages 操作：

```
VertexRDD<Integer> aggregateMessages = graph.aggregateMessages(new AbsFunc4(), new AbsFunc5());
aggregateMessages.toJavaRDD().collect().forEach(System.out::println);
```

如上例所示，它返回一种 VertexRDD 类型，包含顶点 ID 和聚合结果。未作为任何有向边目标的顶点不包含在生成的 RDD 中。

## outerJoinVertices

在分析系统中，连接数据集是一项非常流行的操作。outerJoinVertices 操作用于将图与外部 RDD 进行连接。在某些外部属性需要合并到图中时，将图与外部数据集连接非常有用。

outerJoinVertices 转换的签名如下：

```
outerJoinVertices(RDD<scala.Tuple2<Object, U>> other, scala.Function3<Object, VD, U, Object>)
```

在此，第一个参数是要连接的数据集。图顶点的顶点 ID 与 Tuple 对象/键元素进行连接。第二个参数是用户定义的连接函数。正如操作名称所暗示的，它作为外连接操作工作。因此，连接函数接受一个 scala.Option 选项，如果在外部数据集中未找到顶点 ID 的匹配项，则该选项为空。

其余参数是 Scala 对象、classTag 和 eq；我们已在前一节中定义过它们。让我们定义外部 RDD。顶点属性是前几节中定义的随机名称的图。在此，我们将定义一个为这些名称提供姓氏的 RDD：

```
List<Tuple2<Object, String>> dataToJoin = new ArrayList<>();
dataToJoin.add(new Tuple2<Object, String>(11, "Wilson"));
dataToJoin.add(new Tuple2<Object, String>(21, "Harmon"));
dataToJoin.add(new Tuple2<Object, String>(31, "Johnson"));
dataToJoin.add(new Tuple2<Object, String>(41, "Peterson"));
dataToJoin.add(new Tuple2<Object, String>(51, "Adams"));
JavaRDD<Tuple2<Object, String>> dataToJoinRdd = javaSparkContext.parallelize(dataToJoin);
```

以下是定义与顶点外连接一起执行的操作的用户定义函数：

```
public class AbsFunc1 extends AbstractFunction3<Object, String, Option<String>, String>
@Override
public String apply(Object o, String s1, Option<String> s2) {
  if (s2.isEmpty()) {
    return s1;
  }
```

