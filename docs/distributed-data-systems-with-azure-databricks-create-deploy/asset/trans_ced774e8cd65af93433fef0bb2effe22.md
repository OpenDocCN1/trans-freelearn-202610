

笔记本状态会在任何修改环境的 %pip 或 %conda 命令后重置，因此必须考虑所有这些命令必须放在笔记本开头，以免丢失创建的变量和未保存的结果。因此，如果我们在创建方法、函数或变量之后再执行 %pip 或 %conda 命令，这些方法和变量将无法恢复。

有时，正如任何有经验的 Python 用户都会知道，使用 %pip 或 %conda 安装或卸载包可能会导致一些不稳定的行为，甚至导致某些功能停止工作。为了克服这个问题，最简单的解决方案之一是通过将笔记本从运行中的集群分离然后重新附加来重启环境。如果这不能解决问题，我们可以直接重启笔记本所连接的集群，以便驱动程序和执行程序节点从干净状态重新安装所有库：

- 使用 %pip 或 %conda 安装或卸载包等同于在任何其他 Python 环境中的使用。例如，要安装特定版本的库，我们可以在一个单元格中运行以下命令：

```
%pip install pandas==1.2.1
```

- 或者，另一方面，我们可以通过使用 %pip install 并指定我们包所在路径来安装 Wheel 包：

```
%pip install /path/to/my_package.whl
```

- 要卸载库，必须传递 -y 选项以将默认设置为 yes，因为运行此选项时没有键盘输入：

```
%pip uninstall -y pandas
```

虽然我们可以使用 %pip 命令安装和卸载库，但我们无法卸载包含在 Databricks Runtime 中的库或在集群级别安装的任何库。如果你在笔记本中安装了一个库，其版本与 Databricks Runtime 或集群中安装的版本不同，使用 %pip 卸载它将使版本恢复为 Databricks Runtime 或集群中安装的版本：

- 我们也可以使用 %pip 命令从 GitHub 等仓库安装库，指定版本或 Git 子目录的选项：

```
%pip install git+https://github.com/databricks/
databricks-cli
```

- 你也可以使用 `%pip` 命令从 DBFS 安装自定义包。为此，我们需要运行 `%pip install` 命令并指定 DBFS 中自定义包所在路径：

```
%pip install /dbfs/mypackage-0.0.1-py3-none-any.whl
```

- 此选项允许我们使用 Python Wheels 在本地安装包。需要注意的是，当文件上传到 DBFS 时，这会自动重命名文件，将空格、句点和连字符替换为下划线。这可能成为一个问题，因为 pip 希望 Wheel 文件使用句点来表示版本（如版本 1.0.1）并且使用连字符而不是空格或下划线。因此，要从上传到 DBFS 的 Wheel 安装 Python 包，我们必须先重命名文件，使其与 pip 预期的一致：

```
%pip install /dbfs/mycustompackage-0.0.1-py3-none-any.whl
```

- 我们也可以使用 `%pip` 列出 requirements 文件中的所有已安装库：

```
%pip freeze > /dbfs/sub_dir/requirements.txt
```

包含在文件路径中的任何子目录都应在运行 `%pip freeze` 之前创建。如果运行 `%pip freeze` 且目录未正确创建其所有子文件夹，命令将失败。

- 我们也可以使用 `%pip` 使用 requirements 文件安装库。requirements 文件包含使用 pip 安装的 Python 包列表，例如我们刚刚使用 `%pip freeze` 创建的文件，它始终是一个名为 `requirements` 的文本文件。以下是一个使用位于 DBFS 中的 requirements 文件安装库的示例：

```
%pip install -r /dbfs/requirements.txt
```

这样，我们可以安装所有必需的库，而无需逐个手动安装，这有助于我们重现环境以重现结果和流程。

关于在笔记本级别管理库的一些最终说明如下：

- 使用 `init` 脚本安装的库将可供连接到该集群的所有笔记本使用。
- 从集群 UI 或 API 安装的库可供集群上的所有笔记本使用，这些库是使用 pip 安装的，因此你应该只在这些笔记本中使用 `%pip` 命令。

- 运行 Databricks Runtime ML 或 Databricks Runtime for Genomics 的集群可以使用运行 `conda` 或 `pip` 命令的 `init` 脚本来安装库。但是，如果 `init` 脚本仅包含 `pip` 命令，则你应该只在笔记本中使用 `%pip` 命令。

在笔记本级别管理库允许我们在运行实验时提供关注点分离，而不会影响连接到集群的所有笔记本。了解这与位于集群或工作区级别的库的关系以及如何利用它来高效管理库非常重要。

在下一节中，我们将更深入地讨论如何在 Azure Databricks 中使用 PySpark API。

## PySpark API

我们在描述 Azure Databricks 的功能时一直在所有章节中使用 PySpark API，而没有过多讨论其功能以及我们如何利用它们在处理大数据时进行可靠的 ETL 操作。PySpark 是 Apache Spark 的 Python API，Apache Spark 是一个集群计算框架，是 Azure Databricks 的核心。

## PySpark 的主要功能

PySpark 允许你以 Python 的易用性利用分布式计算的力量，并且除非另有说明，否则它是本书记载我们计算的默认方式。

PySpark 的基础在于其子包的功能，其中最核心的是以下内容：

- **PySpark DataFrames**：按一组命名列存储的行数据。这些 DataFrames 是不可变的，允许我们执行延迟计算。
- **PySpark SQL 模块**：用于处理来自各种来源和格式的结构化和半结构化数据集的高抽象模块。
- **流式源**：用于读取流式数据的容错且可扩展的模块，该数据被转换为批处理并在预定义的时间间隔内读取，使用 map、reduce 等函数中表达的复杂算法进行处理，然后最终写入不同的文件系统。

- **MLlib**：一个包含多个针对集群计算优化的机器学习算法的库，用于分类、回归、聚类、数据准备、实用程序等。
- **RDDs**：这代表 **弹性分布式数据集**。这是一个位于分布式数据集之上的抽象层，是 Azure Databricks 中运行的任何流程的构建块。

由于我们的重点将放在数据处理上，因此了解 PySpark DataFrames 对我们非常重要；因此，我们将在下一节中回顾使用它们的基础操作。

## 操作 PySpark DataFrames

在本节中，我们将介绍使用 Python 的常见 PySpark DataFrame 函数，这些函数可在任何 Azure Databricks 笔记本中执行。步骤如下：

- 1. 我们第一步是创建 PySpark DataFrame。我们可以通过使用 `createDataFrame` 方法创建名为 `df` 的 DataFrame，其列名为 `id` 和 `value`，如下所示：

```
data = [(1, 'c1', 'a1'), (2, 'c2', 'a2')]
columns = ['id', 'val1', 'val2']
df = spark.createDataFrame(
    data,
    columns)
```

使用 `createDataFrame` 方法时，必须考虑以下几点：

- a) 如果模式由列名列表定义，则每列的类型将从数据中推断。
- b) 如果模式使用 `pyspark.sql.types.DataType` 或数据类型字符串定义，则必须与实际数据匹配。

- 2. 例如，要创建一个空白 DataFrame，我们可以先使用 `StructField` 定义数据模式并将信息存储在名为 `schema` 的变量中：

```
columns = [
    StructField("id", IntegerType(), True),
    StructField("value", StringType(), True),
]
schema = StructType(columns)
```

