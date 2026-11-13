
如果运行它，它会将每个值乘以 3，并生成以下输出：

```
| 3 6 9 12 15
```

因此，函数 closure() 中的嵌套函数是一个示例，它记录了创建时所在环境的快照，然后可以访问创建时作用域内的变量 a。

不过，在 Java 中关于闭包有一点值得注意。它使用的、在其定义之外定义的任何变量，都被视为 final 或实际上 final 的。换句话说，在前面给出的示例中，如果你尝试更改嵌套函数内变量 a 的值，编译器将不允许：

```
int a=3;
Function<Integer, Integer> function = t->{
    // a++ // will not compile
    return t*a; // a is available to be accessed in this function
};
```

如果尝试更改值，编译器会抛出错误，指出：在封闭作用域中定义的局部变量 a 必须是 final 或实际上 final 的。

## 流

流表示一组元素，可对其执行惰性求值的聚合操作链。流经过优化，可同时适用于顺序计算和并行计算，充分考虑了 CPU 核心的硬件能力。Streams API 引入于 Java 8，旨在满足开发者的函数式编程需求。流不是基于 Java 的集合；不过，它们可以通过先将集合转换为流来操作集合。流的一些特性使其与 Java 集合 API 截然不同：

- 流不存储元素。它只传输从 I/O 通道、生成函数、数据结构（集合 API）等来源接收到的值，并对其执行一系列流水线计算。
- 流不会更改底层数据，只对其进行处理并生成新的结果数据集。当在流上调用 `distinct()` 或 `sorted()` 方法时，源数据不会改变，而是生成去重或排序后的结果。
- 一般来说，流是惰性求值的；这为代码优化留出了大量机会。惰性求值的经验法则是：如果一个操作返回流，则它是惰性的，否则是立即求值的。
- 与集合不同，流可以是无界的。这对处理流式数据尤其至关重要。无界流可以使用 `limit()`、`findFirst()` 等方法中断。
- 流是可消耗的，这意味着一旦在流上触发终端操作，就不能再重复使用。可以从迭代器中引出类似说法，迭代器遍历一次后需要重新生成。

## 生成流

创建流对象的方式有多种，不过生成流对象最常见的方式如下：

- 1. 使用用户/以编程方式指定的元素：可以使用 java.util.Stream 接口的方法生成流：

  ```
  //Creating Streams using user/programmatically specified elements
  Stream<String> Userstream = Stream.of("Creating", "Streams", "from", "Specified");
  //Creating Streams using array of objects
  Stream<String> ArrayStream = Stream.of(new String[]{"Stream", "from", "an", "array"});
  ```

- 2. 从数组：java.util.Arrays 类有一个 stream(array) 方法，可将数组作为参数来创建流：

  ```
  //Creating Streams from an array
  String[] StringArray = new String[]{"We", "can", "convert", "an", "array", "to", "a"};
  Stream<String> StringStream = Arrays.stream(StringArray);
  ```

- 3. 从集合：Java 类 java.util.Collection 也具有 stream() 和 parallelStream() 方法，分别用于构建串行流和并行流：

  ```
  //Creating Streams from Collection
  List<Double> myCollection = new ArrayList<>();
  for (int i = 0; i < 10; i++) {
      myCollection.add(Math.random());
  }
  //sequential stream
  Stream<Double> sequentialStream = myCollection.stream();
  //parallel stream
  Stream<Double> parallelStream = myCollection.parallelStream();
  ```

- 4. 从映射：由于哈希映射包含键和值，因此创建流的方式略有不同。流因此应用于 java.util.Map.entrySet().stream() 和 java.util.Map.values().stream()：

  ```
  //Stream from HashMap
  Map<String, Integer> mapData = new HashMap<>();
  mapData.put("This", 1900);
  mapData.put("is", 2000);
  mapData.put("HashMap", 2100);
  mapData.entrySet()
      .stream()
      .forEach(p -> System.out.println(p));
  mapData.keySet()
      .stream()
      .forEach(p -> System.out.println(p));
  ```

- 5. **基本类型流：** Stream API 还支持三种基本类型流 IntStream、LongStream 和 DoubleStream。从性能角度来看，这些流 API 更高效，因为元素不会发生装箱和拆箱，因此应尽可能优先使用。range() 方法生成的系列中，起始数字是包含的，而最后一个元素是不包含的。另一方面，rangeClosed() 方法生成的系列中，两端都是包含的：

  ```
  //primitive streams
  IntStream.range(1, 4)
      .forEach(p -> System.out.println(p));
  LongStream.rangeClosed(1, 4)
      .forEach(p -> System.out.println(p));
  DoubleStream.of(1.0, 2.0, 3.0, 4.0)
      .forEach(p -> System.out.println(p));
  ```

- 6. **无限流：** 与集合不同，流可以是无限长度的。流接口提供 generate() 和 iterate() 方法来生成无限流。generate() 方法传入一个函数，该函数始终获取序列的下一个元素：

  ```
  //Infinite Streams using generate()
  Stream<Double> sequentialDoubleStream = Stream.generate(Math::random);
  Stream<Integer> sequentialIntegerStream = Stream.generate(new AtomicInteger());
  ```

  在 iterate() 方法的情况下，会将一个初始元素种子传递给生成流的函数。在这样的无限流中，第一个元素始终是传递给 iterate 函数的种子值：

  ```
  //Infinite Streams using iterate()
  Stream<Integer> sequentialIntegerStream1 = Stream.iterate(Integer.MIN_VALUE);
  Stream<BigInteger> sequentialBigIntegerStream = Stream.iterate(BigInteger.ZERO);
  ```

- 7. **从文件创建流：** Java NIO 包还允许我们使用 lines() 方法从文件创建流。文件中的每一行都被视为流元素；解析文件时也可以定义解码字符集：

  ```
  //Streams from File
  Stream<String> streamOfStrings = Files.lines(Paths.get("Apology_by_Plato.txt"));
  Stream<String> streamWithCharset = Files.lines(Paths.get("Apology_by_Plato.txt"), StandardCharsets.UTF_8);
  ```

  流通常会流水线化，以执行一组操作，这些操作包括一个源（集合、IO 通道）、零个或多个中间操作以及一个终端操作。

## 中间操作

中间操作始终返回另一个流，并且仅在调用终端操作时才会惰性求值。惰性求值的特性在多个操作链式调用时优化了中间操作，因为求值仅在终端操作后进行。惰性求值另一个有用的场景是无限流或大流的用例，因为遍历整个流可能不是必需的，甚至是不可能的，例如 anyMatch、findFirst() 等。在这些场景中，会发生短路（下一节将讨论），终端操作在满足条件后立即退出流程，而不是遍历所有元素。

中间操作还可以进一步细分为有状态操作和无状态操作。有状态操作会保留上一次看到的值，例如 sorted()、limit()、sequential() 等方法，因为在处理当前记录时需要这些值。例如，limit() 方法需要跟踪该流中允许的最大元素数量，只有在掌握流中先前存储的记录数时才能实现。无状态操作没有管理流状态的义务，因此 peek()、map()、filter() 等操作不具备任何状态：
