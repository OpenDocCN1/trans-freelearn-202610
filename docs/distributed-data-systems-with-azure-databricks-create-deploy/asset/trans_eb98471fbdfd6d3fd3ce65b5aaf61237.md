

```
dataset = make_petastorm_dataset(reader) \
.map(lambda x: (tf.reshape(x.features, [-1, 28, 28, 1]),
 tf.one_hot(x.label, 10)))

model = get_model()
optimizer = keras.optimizers.Adadelta()
model.compile(optimizer=optimizer,
    loss='categorical_crossentropy',
    metrics=['accuracy'])
model.fit(dataset, steps_per_epoch=10, epochs=10)
```

9. 在这里，我们可以看到在运行 ML Runtime 的 Azure Databricks 笔记本中训练过程的输出：

```
Cmd 16
> 1 with make_batch_reader(petastorm_dataset_url, num_epochs=100) as reader:
2     dataset = make_petastorm_dataset(reader) \
3 .map(lambda x: (tf.reshape(x.features, [-1, 28, 28, 1]), tf.one_hot(x.label, 10)))
4     model = get_model()
5     optimizer = keras.optimizers.Adadelta()
6     model.compile(optimizer=optimizer,
7         loss='categorical_crossentropy',
8         metrics=['accuracy'])
9     model.fit(dataset, steps_per_epoch=10, epochs=10)
10
s removed, simply drop this attribute
    column_as_pandas = column.data.chunks[0].to_pandas()
10/10 [==============================] - 1s 149ms/step - loss: 49.8006 - accuracy: 0.0885
Epoch 2/10
10/10 [==============================] - 1s 122ms/step - loss: 46.9740 - accuracy: 0.1024
Epoch 3/10
10/10 [==============================] - 1s 116ms/step - loss: 46.6869 - accuracy: 0.0794
Epoch 4/10
10/10 [==============================] - 1s 125ms/step - loss: 45.8081 - accuracy: 0.0927
Epoch 5/10
10/10 [==============================] - 1s 134ms/step - loss: 43.4356 - accuracy: 0.0897
Epoch 6/10
10/10 [==============================] - 1s 125ms/step - loss: 40.5104 - accuracy: 0.1079
Epoch 7/10
10/10 [==============================] - 1s 122ms/step - loss: 39.6522 - accuracy: 0.0952
Epoch 8/10
10/10 [==============================] - 1s 121ms/step - loss: 38.5819 - accuracy: 0.0915
Epoch 9/10
10/10 [==============================] - 1s 126ms/step - loss: 38.0751 - accuracy: 0.1042
Epoch 10/10
10/10 [==============================] - 1s 113ms/step - loss: 37.3983 - accuracy: 0.1063
Command took 15.50 seconds -- by
```

图 9.3 – 使用 Petastorm 训练模型

在本例中，我们定义了一个 Keras 深度学习模型，并使用读取为数据批处理的 Parquet 文件对其进行训练，以进一步优化流程。如本节所示，Petastorm 库为我们提供了高效的方法来使用 Parquet，该格式在 Spark 和 Azure Databricks 中原生受支持，以训练深度学习模型。在下一节中，我们将深入探讨一个具体示例，说明如何使用 Azure Databricks Machine Learning Runtime 为特征化训练深度学习模型。

## 数据预处理和特征化

特征化是我们用来将非结构化数据，如文本、图像或时间序列数据，转换为数值连续特征的过程，这些特征更易于机器和深度学习模型处理。它可以与特征工程区分开来，因为在特征工程中，变量已经以数值形式存在，或者具有更明确的结构，这导致我们需要重构或转换这些变量，使机器或深度学习算法更容易提取模式。在特征化中，我们需要首先定义一种方法，从我们拥有的非结构化数据中提取数值特征。

我们基本上需要进行特征化，因为我们的深度学习模型无法直接解释非结构化数据，因此，我们不仅需要提取它，而且需要以计算高效的方式进行。如果我们使用需要频繁重新训练的模型，此过程需要纳入我们的深度学习管道中。

Azure Databricks Machine Learning Runtime 为我们提供了对深度学习特征化模型的支持，可用于从数据中提取上述特征。我们有可用的预训练模型，可用于计算在其他下游流程或模型中使用的特征。这些预训练模型可以使用已包含的深度学习框架 TensorFlow 和 PyTorch 创建，这些框架已包含在 Azure Databricks Machine Learning Runtime 中。

在本节中，我们将举例说明如何在 Azure Databricks 中通过迁移学习进行数据特征化，迁移学习是一种深度学习方法，可使用特征化作为底层技术，将领域知识从一个问题重用到另一个问题。此特征化将使用预训练模型应用于从原始数据表示中提取数值变量。

## 使用预训练模型进行迁移学习的特征化

在本节中，我们将演示如何在 Azure Databricks Machine Learning Runtime 中使用预训练模型生成可用于下游模型和流程的特征。在这里，我们将通过使用这些提取的特征进行迁移学习来训练新模型来展示这一点。

如前所述，特征化是一种用于特征编码的技术，以便能够将一个模型的领域知识应用于不同但相关的问题。我们可以从实用角度看到这一点，即使用提取的模式来执行一项任务，以便能够将其外推到新任务，同时有可能提高效率并减少训练新模型所需的时间。

本节将演示如何使用 Azure Databricks Machine Learning Runtime 使用预训练的 TensorFlow 模型提取用于迁移学习的特征，使用以下工作流程：

- 1. 我们将从一个预训练的深度学习模型开始——在本例中，是来自 `tensorflow.keras.applications` 的图像分类模型。
- 2. 我们将移除预训练深度学习模型的最后一层，使其产生数值特征张量作为输出，而不是模型的实际预测。
- 3. 我们将把该模型应用于不同的领域知识图像数据集，以便能够用它来编码数值特征。
- 4. 然后，我们可以使用这些编码的特征来训练新的下游模型，尽管在本例中我们将省略此最后一步。

在本例中，我们将使用 pandas 库的 **用户定义函数（UDFs）** 以分布式方式将特征化应用于我们的数据集。pandas UDF 是一种高效的方法，可将此特征化作为数据集转换过程的一部分应用，并且与 `Scalar Iterator` 一样，pandas UDF 为深度学习应用提供了灵活且高性能的支持。

如前所述，在本节中我们将加载数据集并使用 pandas UDF 从中提取特征。我们将采取的步骤是使用 Spark DataFrame 从二进制数据源读取数据加载数据，加载将用于迁移学习的预训练模型，最后，使用 Scalar Iterator pandas UDF 将其应用于从数据集中编码特征。

## 使用 pandas UDF 进行特征化

使用预训练模型使用 pandas UDF 编码新特征的工作流程可以概括如下：

- 1. 首先，我们需要将数据加载到 DataFrame 中。
- 2. 之后，我们需要准备将用于特征化的预训练模型。
- 3. 接下来，我们将定义图像加载过程以及如何应用特征编码。
- 4. 最后，我们通过将预训练模型作为 pandas UDF 应用来提取特征。

此工作流程是一种概括，也可应用于图像预处理和自定义模型训练管道。它也很高效，因为它利用 pandas UDF 来提高特征提取的性能：

- 1. 我们的第一步是进行必要的库导入，其中包括实例化 ResNet50 预训练模型，该模型包含在 TensorFlow 框架中：

```
import pandas as pd
from PIL import Image
import numpy as np
import io
import tensorflow as tf
from tensorflow.keras.applications.resnet50 import ResNet50, preprocess_input
from tensorflow.keras.preprocessing.image import img_to_array
from pyspark.sql.functions import col, pandas_udf, PandasUDType
```

- 2. 我们将使用本例中的 flowers 数据集作为示例数据集。此数据集包含存储在五个文件夹下的花卉照片，每个文件夹代表一种花卉类型。此数据集也存储在 Azure Databricks 数据集示例目录中：

```
%fs ls /databricks-datasets/flower_photos
```

