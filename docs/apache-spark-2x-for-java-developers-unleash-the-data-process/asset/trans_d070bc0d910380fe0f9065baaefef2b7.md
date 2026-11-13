

## Spark 2.X 有哪些新功能？

- **统一的 DataFrame 和 Dataset**：Spark 2.X 版本统一了这两个 API。现在 DataFrame 只是 Dataset 中的一行，没有任何隐式附加的数据类型信息。

- **SparkSession**：在 Spark 2.X 之前，不同的 Spark 作业有不同的入口；也就是说，对于 Spark SQL 我们使用 `sqlContext`，如果还需要 Hive 功能，则 `HiveContext` 是入口。随着 Spark 2.X 的推出，这种歧义已被消除，现在我们有一个名为 `SparkSession` 的单一入口。不过需要注意的是，所有模块特定的入口仍然存在，尚未被弃用。

- **Catalog API**：Spark 2.X 引入了 Catalog API 用于访问 Spark SQL 中的元数据信息。它可以看作是 Hive 中 **Hcatalog** 的对应物。这是统一 Spark SQL 周围元数据结构的重要一步，以便将相同的元数据暴露给非 Spark SQL 应用程序。它还有助于调试 Spark SQL 会话中的临时注册表。现在 `sqlContext` 和 `HiveContext` 的元数据都可用，因为 Catalog API 可以通过 `SparkSession` 访问。

- **结构化流**：结构化流通过持续运行 Spark SQL 作业并在流式数据集上聚合更新后的结果，使 Spark SQL 可用于流式作业。DataFrame 和 Dataset 可用于结构化流中的操作，并配合窗口函数使用。

- **整阶段代码生成**：代码生成引擎已进行修改，以生成更注重性能的代码，避免虚函数调度、将中间操作数据传输到内存等等。

- **Accumulator API**：Spark 2.X 版本添加了一个新的、更简单且性能更好的 Accumulator API，旧 API 已被弃用。

- 当前 Spark 构建中引入了一个原生 SQL 解析器，支持 ANSI-SQL 和 Hive SQL。

- Spark SQL 支持的 SQL 函数列表中还添加了 Hive 风格的分桶支持。

- Spark SQL 中添加了子查询支持，并支持 NOT IN、IN、EXISTS 等子句的其他变体。

- 基于 Databricks 实现的原生 CSV 数据源已纳入 Spark。

- 引入了基于 DataFrame 的新 spark.ml 包，其目标是在新引入的包功能足够成熟可以替换旧包时弃用 spark.mllib。

- 机器学习管道和模型现在可以在 Spark 支持的所有语言中持久化。

## 参考文献

- http://www.evl.uic.edu/cavern/rg/20040525_renambot/Viz/parallel_volviz/paging_outofc_viz97.pdf
- http://static.googleusercontent.com/media/research.google.com/en//archive/gfs-sosp2003.pdf
- http://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf
- 《精通 Apache Solr：掌握 Apache Solr 实用指南》，作者：Mr. Mathieu Nayrolles [第 6 页]
- https://web.archive.org/web/200510209085121/http://weblogs.java.net/blog/tomwhite/archive/2005/09/mapreduce.html
- Nutch：一种灵活且可扩展的开源 Web 搜索引擎，http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.105.5978&rep=rep1&type=pdf
- https://developer.yahoo.com/blogs/hadoop/yahoo-launches-world-largest-hadoop-production-application-398.html
- http://data-informed.com/doug-cutting-reflects-on-hadoops-impact-future/

## 总结

本章介绍了大数据的历史及其维度，以及 Hadoop 和 Spark 的基本概念。随后，你了解了 RDD，并学习了 Spark RDD 模型的基本概念。接着，你学习了 Spark 生态系统的各个组件以及 Spark 2.x 的新功能。

由于本书专注于使用 Java 实现 Spark 应用程序，下一章将回顾核心 Java 的概念，并重点介绍 Java 8 的新特性，这些特性将在开发 Spark 应用程序时发挥作用。

## 再探 Java

本章作为 Java 复习课程添加。本章将讨论在 Apache Spark 中创建应用程序时有用的一些 Java 概念。

本书假设读者已掌握 Java 基础。我们将讨论有用的 Java 概念，并主要关注 *Java 8 有哪些新特性？* 更重要的是，我们将涉及以下主题：

- 泛型
- 接口
- Lambda 表达式
- 流

## 为什么为 Spark 使用 Java？

随着多核 CPU 的兴起，Java 无法跟上其设计变化的步伐，以利用其可用的额外算力，原因在于并发性和不可变性周围的复杂性。我们将稍后详细讨论这一点。首先，让我们了解 Java 在 Hadoop 生态系统中的重要性和可用性。随着 MapReduce 越来越流行，Google 引入了一个名为 **Flume Java** 的框架，用于流水线处理多个 MapReduce 作业。Flume Java 由不可变的并行集合组成，能够执行惰性求值的优化链式操作。这听起来可能与 Apache Spark 所做的事惊人地相似，但在 Apache Spark 和 Java Flume 之前，就有 Cascading，它在 MapReduce 之上构建了一个抽象层，以简化 MapReduce 任务的开发、测试和运行方式。所有这些框架在很大程度上都是 Java 实现，旨在简化 MapReduce 流水线等其他方面。

事实上，这些抽象很简单，**Apache Crunch**（Apache 对 Flume Java 的实现）非常简洁，以至于单词计数问题可以用四五行 Java 代码编写。这非常了不起！

尽管如此，这些 Java 实现永远无法摆脱沉重的 I/O 密集型操作。而正是在这个时候，Apache Spark 庞然大物诞生了，主要使用 Scala 编写，它不仅解决了 MapReduce 的局限性，还解决了在不使用任何抽象框架的情况下开发 MapReduce 作业时 Java 的冗长问题。在他们的论文 *内存集群计算的容错弹性分布式数据集抽象* 中，作者指出：

> “我们选择 Scala 是因为它结合了简洁性（便于交互式使用）和效率（归功于静态类型）。然而，RDD 抽象本身并不要求使用函数式语言。”

最终，Java 社区意识到，随着时间的推移，他们将不得不支持函数式编程，以充分利用集群计算的能力，因为企业硬件的纵向扩展并不是为未来开发的应用程序提供的解决方案。Java 开始围绕并发性发布特性。Java 7 引入了 fork join 框架，以利用现代硬件的能力；它还引入了指令集 `invokedynamic`，使得可以在运行时进行方法调用。这一单一指令集的变化使得动态类型语言可以在 JVM 上运行。Java 8 精妙地利用了 `invokedynamic` 中的晚期绑定概念，并围绕它开发了完整的 Lambda 表达式。它还发布了 Stream API，可以将其视为 Java 中函数式编程的集合框架，而无需实际存储元素。Java 8 中其他值得注意的变更包括：

- 函数式接口
- 接口中的默认方法和静态方法
- 新的日期和时间 API
- 脚本支持

Apache Spark 已添加对 Java 8 的支持，因此在每个版本中，使用 Scala 和 Java 编写代码之间的差异正在模糊。学习曲线也与其潜力相匹配，因为只需学习 Apache Spark API 而无需学习 Scala。Apache Spark 的另一个亮点是 Spark-SQL，如果需要交互式和动态编码，它会非常有用。

## 泛型

泛型是在 Java 1.5 中引入的。泛型帮助用户创建其定义中具有抽象类型的通用代码。该抽象类型可以在实现中替换为任何具体类型。

例如，List 接口或其实现（如 ArrayList、LinkedList 等）都是使用泛型定义的。用户在实现 List 时可以提供 Integer、Long 或 String 等具体类型：

```
List<Integer> list1 = new ArrayList<Integer>();
List<String> list2 = new ArrayList<String>();
```

在这里，list1 是整数列表，list2 是字符串列表。使用 Java 7 时，编译器可以推断类型。因此，前面的代码也可以写成如下形式：

```
List<Integer> list1 = new ArrayList<>();
List<String> list2 = new ArrayList<>();
```

泛型的另一个巨大好处是它带来了编译时类型安全。让我们创建一个不使用泛型的列表：

```
List list = new ArrayList<>();
list.add(1);
list.add(2);
list.add("hello");
list.add("there");
```

在这里，你可以看到列表同时包含整数和字符串。如果从中检索一个元素，默认情况下它将是对象类型：

```
Object object = list.get(0);
```

因此，用户必须将其专门强制转换为正确的类型：

```
Integer object = (Integer)list.get(0);
```

这会使代码容易出错。因为如果强制转换不正确，将在运行时抛出 ClassCastException。让我们使用泛型创建相同的列表：

