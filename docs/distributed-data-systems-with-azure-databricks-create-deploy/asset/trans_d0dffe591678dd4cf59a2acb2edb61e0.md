

- **writeLocality**：它将决定 TFRecord 文件是写入工作节点本地还是分布式文件系统。可选选项如下：
  a) **distributed**：这是默认选项，决定文件使用 Spark 默认文件系统写入。
  b) **local**：此选项以分区方式将文件写入各个工作节点的磁盘，每个工作节点本地磁盘上写入部分数据。

当我们使用本地模式时，它会将数据写入每个工作节点的本地磁盘上，每个工作节点存储数据的一个子集。哪个子集数据存储在哪个工作节点上取决于数据帧的分区方式。之后，这些数据子集将被合并成一个单一的 TFRecord 文件并写入分区所在的节点。这是一个在分布式方式训练机器和深度学习算法时很有用的功能，其中每个工作节点接收到一个数据子集进行训练。当指定本地模式作为选项时，传递给写入器的路径将被理解为每个工作节点上的基路径，数据子集将写入该路径。

## 使用 TFRecords 管理数据

在本节中，我们将演示如何将 Spark DataFrames 中的图像数据保存到 TFRecords 并使用 Azure Databricks 中的 TensorFlow 加载它。我们将以 Databricks 文件系统上可用的 flowers 示例数据集为例，其中包含存储在五个子目录下的花卉照片，每个类别一个。

我们将加载 flowers Delta 表，该表包含使用二进制文件数据源预处理的花卉数据集并存储为 Spark DataFrame。我们将使用此数据来演示如何将 Spark DataFrames 中的数据保存到 TFRecords：

- 1. 作为第一步，我们将使用 PySpark 加载数据：

```
from pyspark.sql.functions import col
import tensorflow as tf
spark_df = spark.read.format("delta").load("/databricks-datasets/flowers/delta") \
    .select(col("content"), col("label_index")) \
    .limit(100)
```

2. 接下来，我们将把加载的数据保存为 TFRecord 类型文件：

```
path = '/ml/flowersData/converted_data.tfrecord'
spark_df.write.format("tfrecords").mode("overwrite").save(path)
display(dbutils.fs.ls(path))
```

正如你所见，由于底层 spark-tensorflow-connector 库的存在，我们可以轻松地将 Spark DataFrames 保存到 TFRecord 文件，该库将 Spark 与 TensorFlow 集成：

```
from pyspark.sql.functions import col
import tensorflow as tf
spark_df = spark.read.format("delta").load("/databricks-datasets/flowers/delta") \
.select(col("content"), col("label_index")) \
.limit(100)
```

spark_df: pyspark.sql.dataframe.DataFrame = [content: binary, label_index: long]

```
path = '/ml/flowersData/converted_data.tfrecord'
spark_df.write.format("tfrecords").mode("overwrite").save(path)
display(dbutils.fs.ls(path))
```

|  | path | name | size |
|---|---|---|---|
| 1 | dbfs:/ml/flowersData/converted_data.tfrecord/_SUCCESS | _SUCCESS | 0 |
| 2 | dbfs:/ml/flowersData/converted_data.tfrecord/part-r-00000 | part-r-00000 | 17175166 |

Showing all 2 rows.

Fig.9.1 – Output location of the tfrecord file

3. 学习如何在 Azure Databricks 中使用 TFRecords 的下一步是加载 TFRecord 数据到 TensorFlow。首先，我们将创建一个 TFRecordDataset 作为输入管道：

```
import os
filenames = [f"/dbfs{path}/{name}" for name in os.listdir("/dbfs" + path) if name.startswith("part")]
```

```
dataset = tf.data.TFRecordDataset(filenames)
```

4. 为了能够使用数据，我们需要创建一个解码器函数来读取、解析和规范化数据。我们使用 map 函数将记录解析为张量，该函数将把我们的解码器函数应用于每个样本。首先，我们将定义解码器函数，它将以一个序列化示例作为必需参数，图像大小作为可选参数：

```
def decode_and_normalize(serialized_example, image_size = 224):
    # Parse from single example
    feature_dataset = tf.io.parse_single_example(
        serialized_example,
        features={
            'content': tf.io.FixedLenFeature([], tf.string),
            'label_index': tf.io.FixedLenFeature([], tf.int64),
        })
    # Decode the parsed data
    image = \
    tf.io.decode_jpeg(feature_dataset['content'])
    label = \
    tf.cast(feature_dataset['label_index'], tf.int32)
    # Resize the decoded data into the desired size
    image = tf.image.resize(image, [image_size, image_size])
    # Finally, normalize the data
    image = \
    tf.cast(image, tf.float32) * (1. / 255) - 0.5
    return image, label
```

5. 一旦我们定义了解码函数，我们就可以使用它通过映射数据集并将其应用于其中的每个样本来解析我们的数据：

```
parsed_dataset = dataset.map(decode_and_normalize)
```

这样，我们就解析了数据集，现在它可以作为输入数据用于训练机器或深度学习模型。

在下一节中，我们将深入探讨如何在 Azure Databricks 中使用 TFRecord 的完整示例。

## 自动模式推断

spark-tensorflow-connector 库，该库将 Spark 与 TensorFlow 集成，支持在将 TensorFlow 记录读取到 Spark DataFrames 时自动进行模式推断。模式推断是一个昂贵的操作，因为它需要额外的一次数据读取，因此最好显式指定它，这将提高我们管道的整体性能。

以下 Python 代码示例演示了我们如何在作为示例创建的一些测试数据上执行此操作：

- 1. 我们首先要定义数据的模式：

```
from pyspark.sql.types import *
path = "test-output.tfrecord"
fields = [StructField("id", IntegerType()),
StructField("IntegerCol", IntegerType()),
StructField("LongCol", LongType()),
StructField("FloatCol", FloatType()),
StructField("DoubleCol", DoubleType()),
StructField("VectorCol", ArrayType(DoubleType(),
True)),
StructField("StringCol", StringType())]
schema = StructType(fields)
```

- 2. 接下来，我们创建一个包含测试数据的 Spark DataFrame，对其进行并行化，并将其作为 TFRecord 文件写入：

```
test_rows = [[11, 1, 23, 10.0, 14.0, [1.0, 2.0], "r1"],
[21, 2, 24, 12.0, 15.0, [2.0, 2.0], "r2"]]
rdd = spark.sparkContext.parallelize(test_rows)
df = spark.createDataFrame(rdd, schema)
```

```
df.write.format("tfrecords").option("recordType", "Example").save(path)
After this, we can read the TFRecord file back to a Spark Data frame in the next way:
df = spark.read.format("tfrecords").option("recordType", "Example").load(path)
df.show()
```

3. 这里，我们看到整个过程在 Azure Databricks 笔记本中运行：

```
from pyspark.sql.types import *
path = "test-output.tfrecord"
fields = [StructField("id", IntegerType()),
StructField("IntegerCol", IntegerType()),
StructField("LongCol", LongType()),
StructField("FloatCol", FloatType()),
StructField("DoubleCol", DoubleType()),
StructField("VectorCol", ArrayType(DoubleType(), True)),
StructField("StringCol", StringType())]
schema = StructType(fields)

test_rows = [[11, 1, 23, 10.0, 14.0, [1.0, 2.0], "r1"], [21, 2, 24, 12.0, 15.0, [2.0, 2.0], "r2"]]
rdd = spark.sparkContext.parallelize(test_rows)
df = spark.createDataFrame(rdd, schema)
path= 'dbfs:/tmp/dataset'
df.write.format("tfrecords").option("recordType", "Example").save(path)
display(df)
```

(3) Spark Jobs
df: pyspark.sql.dataframe.DataFrame = [id: integer, IntegerCol: integer ... 5 more fields]

| id | IntegerCol | LongCol | FloatCol | DoubleCol | VectorCol | StringCol |
|---|---|---|---|---|---|---|
| 1 | 11 | 1 | 23 | 10 | 14 | [1, 2] | r1 |
| 2 | 21 | 2 | 24 | 12 | 15 | [2, 2] | r2 |

Showing all 2 rows.

Fig.9.2 – TFRecord column example data types

正如你所见，Spark 与 Azure Databricks 中 TFRecord 文件格式的集成是无缝的，允许我们直接在 Spark DataFrames 中读写 TFRecord 文件。

## 使用 TFRecordDataset 加载数据

我们用于处理 TFRecord 文件的另一个可用选项是使用 TFRecordDataset TensorFlow 类。当我们需要标准化输入数据以提高加载过程的性能时，这很有用：

- 1. 你也可以传递一个包含我们希望 TFRecordDataset 类读取的文件路径的列表，如下一代码块所示：

```
filenames = [<your_tfrecode_file_1>,..., [<your_tfrecode_file_n>]
raw_dataset = tf.data.TFRecordDataset(filenames)
```

raw_dataset 是一个 TFRecordDataset 类，包含序列化的 tf.train.Example 记录。当我们迭代它时，它将以标量字符串张量的形式返回这些记录。

- 2. 在下一个示例中，我们将使用 take 方法仅显示 raw_dataset 中的前 10 条记录：

```
for raw_record in raw_dataset.take(10):
    print(repr(raw_record))
```

请记住，要迭代 TFRecordDataset，必须启用急切执行。

- 3. tf.train.Example 标量字符串张量然后可以使用 tf.io.FixedLenFeature 函数进行解析。请注意，我们需要定义 feature_description，因为数据集使用图执行，此描述用于构建它们的形状和类型签名：

```
feature_description = {
    'feature0': tf.io.FixedLenFeature([], tf.int64, default_value=0),
    'feature1': tf.io.FixedLenFeature([], tf.string, default_value=''),
    'feature2': tf.io.FixedLenFeature([], tf.float32, default_value=0.0),
}
```

