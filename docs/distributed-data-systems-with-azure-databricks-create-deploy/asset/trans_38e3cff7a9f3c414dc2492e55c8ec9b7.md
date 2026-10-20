

feature_description 是一个字典，用于保存特征的描述，这对于我们的解析函数是必要的。

4. 这里，我们下一步将定义解析函数，该函数将使用之前定义的 feature_description 字典读取 tf.train.Example 类输入：

```
def _parsing_function(proto_input):
    return tf.io.parse_single_example(proto_input,
    feature_description)
```

5. 作为读取 TFRecord 文件的下一步，我们将使用 tf.data.Dataset.map 方法将我们定义的解析函数应用于数据集中的每个项目：

```
parsed_dataset = raw_dataset.map(_parsing_function)
parsed_dataset
```

我们也可以使用 tf.io.parse_example 函数以批量的方式解析数据，作为逐个读取的替代方案。

6. 我们可以通过在启用急切执行时显示数据集中的观测值来检查数据。这里，无论数据集中有多少个观测值，我们只会显示其中前 10 个，它们将显示为特征的字典：

```
for parsed_record in parsed_dataset.take(10):
    print(repr(parsed_record))
```

显示的每个项目都是一个 tf.Tensor，它是 TensorFlow 的一个类，其值是该张量的 numpy 元素。

7. 要将观测值写入 TFRecord 文件，我们需要在写入文件之前将每个观测值转换为 tf.train.Example 类。这里，我们将把 n_obs 个 tf.train.Example 记录写入 TFRecord 文件：

```
with tf.io.TFRecordWriter(filename) as writer:
    for i in range(n_obs):
        example = serialize_example(feature0[i],
                                    feature1[i],
                                    feature2[i])
        writer.write(example)
```

TFRecord 文件格式在处理需要使用并行或分布式方法处理的大量数据时优化了我们的训练管道。如本节所示，我们可以使用 TensorFlow 的原生工具来操作 TFRecord 文件，以利用 Azure Databricks 等分布式计算系统的优势。在下一节中，我们将介绍 Petastorm，这是一个用于启用使用 Parquet 文件作为机器和深度学习应用训练数据的库。

## 使用 Petastorm 进行分布式学习

Petastorm 是一个开源库，允许我们使用存储为 Apache Parquet 文件的数据集对机器和深度学习算法进行单机或分布式训练。它支持流行的框架，如 PyTorch、TensorFlow 和 PySpark，也可用于其他 Python 应用。Petastorm 为我们提供了一个简单的函数，用于将 Parquet 格式的功能增强为 Petastorm 特定的数据，以便在机器和深度学习模型训练中使用。我们只需通过创建来自 Databricks 文件系统的读取器对象并对其进行迭代即可读取数据。底层 Petastorm 库使用 PyArrow 库读取 Parquet 文件。

在本节中，我们将讨论如何使用 Petastorm 进一步扩展 Azure Databricks 中机器和深度学习训练管道的性能。

## 介绍 Petastorm

如前所述，Petastorm 是一个开源库，它使单机或分布式训练和评估机器和深度学习模型成为可能，使用存储在 Apache Parquet 格式中的数据以及已加载到 Spark DataFrames 中的数据。该库支持流行的科学数据框架，如 TensorFlow、PyTorch 和 PySpark。当我们需要使用存储在 Parquet 格式中的数据训练机器和深度学习模型时，Petastorm 是最佳选择。

Petastorm 库具有各种功能，如优化的数据共享、数据分区、洗牌和行过滤，以及使用 n-gram 支持时间序列。

当模型利用观察到的环境变量来更好地理解环境并预测该环境中的未来事件时，需要时间上下文时使用 n-gram。

当输入数据是时间序列时，Petastorm 可以提供这种时间上下文。当从 Petastorm Reader 对象请求 n-gram 时，连续的行将被合并为单个训练样本，以提供时间上下文，同时保持数据结构化。

使用 Petastorm 加载数据时，我们有两个可用的选项来并行化数据加载和解码操作：

- **线程池**：此实现用于观测值是编码数据或高分辨率图像时。选择此模式时，大部分处理时间都花费在通过 C++ 代码解码图像，几乎没有 Python 代码。
- **进程池**：此实现适用于行大小较小时。选择此模式时，大部分解码仅使用 Python 代码在并行运行的多个进程中完成。

为并行化数据加载和解码操作而选择的选项将取决于您想要读取的数据类型。

正如我们在本节中介绍的，Petastorm 是一个开源库，帮助我们利用 Parquet 文件格式的效率，并允许我们以分布式方式使用它来训练深度学习模型。在下一节中，我们将学习如何使用 Petastorm 创建数据集并使用它准备图像数据，以在 Azure Databricks 机器学习运行时中训练我们的深度学习模型。

## 生成数据集

同样，我们为其他数据集文件格式所做的那样，当我们想要使用 Petastorm 生成数据集时，我们需要首先定义一个数据模式，称为 Unischema。这是我们唯一需要这样做的时候，因为 Petastorm 然后会将其广播到所有受支持的框架，如 PySpark、TensorFlow 和纯 Python。

定义的 Unischema 被序列化为 Parquet 存储元数据中的自定义字段，因此只需指定数据集的路径即可读取它。

以下示例展示了如何创建 Unischema 实例，该实例将用于定义数据集的模式。为了正确定义模式，需要传递到字段属性中的必需参数是字段名称、表示为 NumPy 数据类型的数据类型、数组的形状、用于编码和解码数据的编解码器，以及字段是否可为空：

```
first_schema = Unischema('first_schema', [
    UnischemaField('id', np.int32, (),
    ScalarCodec(IntegerType()), False),
    UnischemaField('image1', np.uint8, (128, 256, 3)
    CompressedImageCodec('png'), False),
```

```
UnischemaField('array_4d', np.uint8, (None, 128, 30, None), Ndar rayCodec(), False),
]
```

我们也可以使用 PySpark API 来写入 Petastorm 数据集。以下代码示例演示了如何使用 Petastorm 和之前定义的数据模式创建 10 行数据集：

```
rows_count = 10
with materialize_dataset(spark, output_url, 'first_schema', rowgroup_size_mb):
    rows_rdd = sc.parallelize(range(rows_count))\
    .map(row_generator)\
    .map(lambda x: dict_to_spark_row(first_schema', x))
    spark.createDataFrame(rows_rdd,
                          first_schema'.as_spark_schema()) \
    .write \
    .parquet('file:///tmp/example_dataset')
```

让我们更详细地解释一下这个示例中发生了什么。materialize_dataset 是上下文管理器，它将根据指定的数据模式处理所有必要的配置，并最终写入 Petastorm 特定的元数据。输出文件路径可以指向本地或 Databricks 文件系统中的位置。rowgroup_size_mb 定义 Parquet 行组的目标大小（以兆字节为单位），row_generator 是一个返回与指定数据模式匹配的 Python 字典的函数。dict_to_spark_row 将根据指定的模式验证数据类型并将字典转换为 pyspark.Row 对象。

我们可以使用 Petastorm Spark 转换器 API 从 Spark DataFrames 加载数据，该 API 使我们能够将 Spark DataFrames 转换为 TensorFlow 或 PyTorch 格式。这是通过首先将输入 Spark DataFrame 写入 Parquet 文件，然后可以使用 tf.data.Dataset 或 torch.utils.data.DataLoader 加载该文件来完成的。

当我们进行分布式训练深度学习模型时，推荐的工作流程是在 Azure Databricks 中加载和处理数据，使用 Petastorm spark_dataset_converter 方法将数据从 Spark DataFrame 转换为 TensorFlow 数据集或 PyTorch DataLoader，最后使用它来训练我们的深度学习模型。

