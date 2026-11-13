

| 方法 | 中间操作 | 终止操作 | 短路操作 | 有状态 | 无状态 |
|---|---|---|---|---|---|
| Filter | Y | | | | Y |
| Map | Y | | | | Y |
| flatMap | Y | | | | Y |
| Distinct | Y | | | Y | |
| Sorted | Y | | | Y | |
| forEach | | Y | | | Y |
| Peek | Y | | | | Y |
| Limit | Y | | Y | Y | |
| toArray | | Y | | | Y |
| Reduce | | Y | | | Y |
| Collect | | Y | | | Y |
| Max | | Y | | | |
| Min | | Y | | | |
| anyMatch | | Y | Y | | |
| findFirst | | Y | Y | | |
| sequential | Y | | | | |
| parallel | Y | | | | |

## 使用中间操作

以下是一些常用的中间操作：

- 1. filter()：filter() 方法首先对流中的每个元素进行求值，找出符合给定断言条件的元素，从而创建一个新流：

```
//Filter Operation
IntStream.rangeClosed(1, 10)
.filter(s -> s>4)
.forEach(p -> System.out.println(p));
```

- 2. map()：map() 是一种中间操作，其输出值的数量与输入值的数量相等。它是一个一元运算符，只需转换元素，因此每次将元素传递给 map 函数时都会返回一个对象，从而输入值与转换后的输出值之间存在一对一映射。当我们希望转换流中的元素时，通常需要使用 Map，例如，我们希望创建一个整数流，对应输入流中每个字符串的长度。可以通过使用 map() 实现此类转换：

```
int sumOfLength=streamSupplier.get().map(x ->x.toString().length()).peek(x->
.mapToInt(x->x.intValue()).sum());
```

您可能注意到，我们还使用了特殊的 map() 函数，例如 mapToInt()，它们对应于 int、long 和 double 等原始数据类型。这些特殊 map() 函数的目的是提供一些额外的管道操作，例如 sum()。

- 3. flatMap()：flatMap() 是两种操作（map() 和 flat()）的组合。当数据传递给 `flatMap()` 时，它可能会返回多个值以扁平化结果。从以下表示中可以得出一个简单的推论：

| 输入数据流 | 操作 | 结果 |
|---|---|---|
| `Stream<String[]>` | `flatMap()` 操作 | `Stream<String>` |
| `Stream<Set<String>>` | `Stream<String>` | `......` |
| `Stream<List<String>>` | `Stream<String>` | `......` |
| `Stream<List<T>>` | `Stream<T>` | `......` |

因此，`flatMap()` 本质上接受一个输入元素并对其进行转换，可能会返回多个值，从而使最终输出采用 `Stream<T>` 的形式：

```
//flatMap Example
Stream<List<String>> streamList = Stream.of(
Arrays.asList("FirstList-FirstElement"),
Arrays.asList("SecondList-FirstElement", "SecondList-SecondElement"),
Arrays.asList("ThirdList-FirstElement"));
//The streamList is of the form List<String>
Stream<String> flatStream = streamList
.flatMap(strList ->strList.stream());
// But after applying flatMap operation it translates into Stream<String>
flatStream.forEach(System.out::println);
```

与 `map()` 操作类似，`flatMap()` 操作也有针对 int、long 和 double 数据类型的 `flatMap()` 函数的原始变体。

- 4. Sorted：可以使用 sorted() 方法以自然顺序对流中的元素进行排序。但是，流中的元素必须可比较，否则将抛出 ClassCastException：

```
//Natural Sorting
streamSupplier.get().sorted().forEach(System.out::println);
```

除了自然排序外，还可以对元素进行自定义排序，此时可以使用接受比较器的 sorted() 方法的重写方法。如果要将自然排序更改为逆序，可以通过以下方式实现：

```
//Comparing elements with reverse order
streamSupplier.get().sorted(Comparator.reverseOrder()).forEach(System.out::
```

对数据进行排序的另一种方法是将 Lambda 函数作为比较器的参数：

```
//Sorting the element in reverse order based on their length streamSupplier.
```

还可以对多个字段进行排序，因为可以链式调用排序算法以特定顺序对数据进行排序：

```
//Sorting on multiple fields
streamSupplier.get().sorted(Comparator.comparing(x ->x.toString().length())).
```

- 5. Distinct：distinct() 方法返回流中元素的唯一值。可以通过使用重写的 equals 方法来更改比较属性：

```
//Distinct filters all the multiple records having same length streamSupplier
```

- 6. Limit：在某些场景中（尤其是无限流的情况下），可以使用 limit() 方法并指定适当的 maxSize 值来截断流：

```
//Limiting the size of the stream streamSupplier.get().limit(2).forEach(Syst
```

## 终止操作

终止操作充当管道流操作中的触发点以触发执行。终止操作要么返回 void，要么返回非流类型对象，一旦管道操作在流上执行完毕，流就变得多余。执行管道流操作始终需要终止操作。

流操作还可以进一步分类为短路操作。当无限输入产生有限流时，中间操作被称为短路，例如 `limit()` 方法的情况。同样，在终止操作中，短路操作是指无限输入可能在有限时间内终止，例如 `anyMatch()`、`findAny()` 等方法。

流还支持并行性，但在有状态操作的情况下，由于每个并行操作都可以有自己的状态，并行有状态操作可能需要多次传递才能完成操作。例如，如果一次并行处理结果，`distinct()` 方法将不会产生正确的结果。但是，对于无状态操作不存在此类多次传递的限制。串行和并行执行的整个操作集保持不变，唯一的区别在于流本身。如果流是并行的，操作将并行发生；否则，操作将串行执行。流 API 提供了一个辅助方法 `isParallel()` 来检查流是串行还是并行。并行流操作与串行流操作匹配所需的唯一前提是传递给操作的值必须是非干扰的，非干扰是指在任何给定操作集下，其底层值不会更改的数据。

流中数据的顺序可能取决于源、中间操作或终止操作。

## 使用终止操作

以下是一些需要详细关注的终止操作：

- forEach()：函数式编程与命令式编程之间最重要的范式差异之一在于，它关注的是实现什么，而不是如何实现。这一点比 Java 8 中新引入的 forEach() 方法更能说明问题。之前的迭代实现，无论每种实现多么独特，都关注迭代将如何进行，并且每次对于列表、HashMap、数组、集合等都不同。这种遍历元素的方式也称为外部迭代，它本质上不仅串行处理数据，还遵循集合的存储和检索顺序。在某些情况下，这会对性能产生影响，因此 Stream API 引入了 forEach() 方法来克服其中一些限制。在流 API 中使用 forEach() 方法进行迭代是内部迭代，这意味着我们不定义如何迭代，而是定义在迭代期间做什么。这还提供了序列化和并行化迭代的机会，并由 API 本身处理：

```
Supplier<Stream<String>>streamSupplier =()->Stream.of( new String[]{"Stream"
//Sequential For each
streamSupplier.get().sequential().forEach(P->System.out.println("Sequential
```

我们还可以按照以下方式并行迭代相同的元素：

```
Supplier<Stream<String>>streamSupplier =()->Stream.of( new String[]
{"Stream","from","an","array","of","objects"} );
//Parallel For each
streamSupplier.get().parallel().forEach(P->System.out.println("Parallel
output :: "+P));
```

然而需要注意的是，输出顺序已更改，因为并行迭代不保证集合

