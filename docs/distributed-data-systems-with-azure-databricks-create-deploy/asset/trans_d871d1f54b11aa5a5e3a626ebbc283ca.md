

strategy = tf.distribute.experimental.MultiWorkerMirroredStrategy(
    tf.distribute.experimental.CollectiveCommunication.NCCL)
with strategy.scope():
    import uuid
    BUFFER_SIZE = 10000
    BATCH_SIZE = 32
    def make_datasets():
        (mnist_images, mnist_labels), _ = \
            tf.keras.datasets.mnist.load_data(path=str(uuid.uuid4())+'mnist.npz')

        dataset = tf.data.Dataset.from_tensor_slices((
            tf.cast(mnist_images[..., tf.newaxis] / 255.0,
                tf.float32),
            tf.cast(mnist_labels, tf.int64))
        )
        dataset = dataset.repeat().shuffle(BUFFER_SIZE).batch(BATCH_SIZE)
        return dataset

    def get_model():
        model = tf.keras.Sequential([
            tf.keras.layers.Conv2D(32, 3, activation='relu',
                input_shape=(28, 28, 1)),
            tf.keras.layers.MaxPooling2D(),
            tf.keras.layers.Flatten(),
            tf.keras.layers.Dense(138, activation='relu'),
            tf.keras.layers.Dense(10, activation='softmax'),
        ])
        model.compile(
            loss=tf.keras.losses.sparse_categorical_crossentropy,
            optimizer=tf.keras.optimizers.SGD(learning_rate=0.001),
            metrics=['accuracy'],
        )
        return model

train_datasets = make_datasets()
worker_model = get_model()
options = tf.data.Options()
options.experimental_distribute.auto_shard_policy = \
    tf.data.experimental.AutoShardPolicy.DATA
train_datasets = train_datasets.with_options(options)
worker_model.fit(x=train_datasets, epochs=3,
    steps_per_epoch=5)

现在，我们可以通过运行 MirroredStrategyRunner 并将 local_mode 和 use_custom_strategy 参数设置为 True 来在本地执行指定的训练策略，从而分发模型的训练，如下所示：

```
runner = MirroredStrategyRunner(num_slots=1,
                                use_custom_strategy=True,
                                local_mode=True,
                                use_gpu=USE_GPU)
runner.run(train_custom_strategy)
```

我们可以在本地模式下运行以在扩展训练之前验证脚本是否正常工作。我们可以通过验证日志中是否打印了 CollectiveCommunication.NCCL 来检查这一点。

正如我们所见，Spark TensorFlow Distributor 是 HorovodRunner 的替代方案，用于管理 TensorFlow 深度学习模型的分布式训练。我们可以使用此库以非常简单的方式在单节点和集群上训练模型，能够利用 GPU 或 CPU 集群的强大功能进行深度学习模型的分布式训练。

## 摘要

在本章中，我们学习了如何使用 Horovod 和 Azure Databricks 中的原生 TensorFlow for Spark 的分布式学习来提高深度学习算法训练管道的性能。我们讨论了驱动有效分发关键操作（例如梯度下降和模型权重更新）能力的核心算法，这些算法在随 Azure Databricks Runtime for Machine Learning 提供的 horovod 库中是如何实现的，以及我们如何使用 TensorFlow 框架中现在可用的原生 Spark 支持来进行深度学习模型的分布式训练。

本章结束了本书。希望它能让你更轻松地学习 Azure Databricks 中用于数据工程和数据科学的令人难以置信的众多功能。如前所述，大多数代码示例都是官方库的修改或取自 Azure Databricks 文档，以便提供有良好文档记录的示例，因此向帮助构建这些库和工具的所有开发人员致敬。

![](img/7e45bd08916c2a17d4e75cbb898f41e8_399_0.png)

Packt.com

订阅我们的在线数字图书馆，全面访问超过 7,000 本书籍和视频，以及帮助您规划个人发展和推进职业生涯的行业领先工具。更多信息，请访问我们的网站。

## 为什么订阅？

- 使用来自 4,000 多名行业专业人士的实用电子书和视频，花更少时间学习，花更多时间编码
- 使用专为你打造的技能计划改善学习
- 每月免费获得一本电子书或视频
- 完全可搜索，便于轻松访问重要信息
- 复制、粘贴、打印和书签内容

你知道 Packt 为每本出版的书籍提供电子书版本，文件格式为 PDF 和 ePub 吗？你可以在 packt.com 升级到电子书版本，作为印刷书籍的客户，你有权享受电子书的折扣。欲了解更多详情，请联系 customercare@packtpub.com。

在 www.packt.com，你还可以阅读大量免费技术文章，订阅多种免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

## 你可能喜欢的其他书籍

如果你喜欢这本书，你可能对 Packt 的这些其他书籍感兴趣：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_400_0.png)

### Azure Data Factory Cookbook

Dmitry Anoshin, Dmitry Foshin, Roman Storchak, Xenia Ireton

ISBN: 978-1-80056-529-6

- 在 ADF 中创建编排和转换作业
- 使用 Azure Synapse 开发、执行和监控数据流
- 使用 Azure Data Lake 和 ADF 创建大数据管道
- 使用 Apache Spark 和 ADF 构建机器学习应用
- 将本地 SSIS 作业迁移到 ADF
- 将 ADF 与常用的 Azure 服务集成，如 Azure ML、Azure Logic Apps 和 Azure Functions
- 在 HDInsight 和 Azure Databricks 中运行大数据计算作业
- 使用 ADF 的内置连接器将数据从 AWS S3 和 Google Cloud Storage 复制到 Azure Storage

![](img/7e45bd08916c2a17d4e75cbb898f41e8_401_0.png)

### Azure Data Engineering Cookbook

Ahmad Osama

ISBN: 978-1-80020-655-7

- 使用 Azure Blob 存储存储大量非结构化数据
- 对 Cosmos Table API 执行 CRUD 操作
- 使用 Azure SQL Database 实现弹性池和业务连续性
- 使用 Azure Synapse Analytics 摄取和分析数据
- 开发 Data Factory 数据流以从多个来源提取数据
- 管理、维护和保护 Azure Data Factory 管道
- 使用 Azure Stream Analytics 和 Data Explorer 处理流数据

## Packt 正在寻找像你这样的作者

如果你有兴趣成为 Packt 的作者，请访问 authors.packtpub.com 并立即申请。我们已与成千上万的开发人员和技术专业人士合作，就像你一样，帮助他们与全球技术社区分享他们的见解。你可以提交通用申请、申请我们正在招聘作者的特定热门主题，或提交你自己的想法。

## 留下评论 - 让其他读者知道你的想法

请通过你在购买书籍的网站上留下评论，与他人分享你对这本书的想法。如果你是在 Amazon 购买的书籍，请在此书的 Amazon 页面上留下诚实的评论。这对其他潜在读者做出购买决策、我们了解客户对产品的看法以及作者看到他们与 Packt 合作的书名的反馈都至关重要。它只需要你几分钟的时间，但对其他潜在客户、我们的作者和 Packt 来说都很有价值。谢谢！

## 索引

### A

access token
  用于配置 Azure Databricks CLI 59, 60
all-purpose clusters 9
Apache Kafka 174-176
Apache Spark 4, 5
application programming interfaces (APIs) 230
area under the curve (AUC) 267
Atomicity, Consistency, Isolation, and Durability (ACID) 107
authentication 34
authorization 34
Auto Compaction 144
Auto Loader 173, 174
AutoML
  关于 303
  超参数调优 302
Avro data 176-178
AWS services 6
Azure AD token
  用于配置 Azure Databricks CLI 60
Azure Blob storage
  关于 79
  访问密钥，上传 81, 82
  连接，设置到 83
  文件，上传 81, 82
  设置 80, 81
  与 Azure Databricks 配合使用 79
Azure Cloud Shell
  Databricks CLI 64, 65
Azure Databricks
  关于 5, 6
  从中访问 ADLS Gen2 76, 77
  架构，检查 6, 7
  使用 Azure Blob storage 79
  使用 Bloom filters 153
  集群库 200-204
  Clusters CLI 63
  处理连续数据流 168
  使用 Horovod 分布式学习库 361-364
  Jobs CLI 63
  使用作业编排作业 91
  安装库 200
  笔记本作用域库 200-205
  与 S3 配合使用 78
  用于调度作业 101
  与 VNets 配合使用 53, 54

