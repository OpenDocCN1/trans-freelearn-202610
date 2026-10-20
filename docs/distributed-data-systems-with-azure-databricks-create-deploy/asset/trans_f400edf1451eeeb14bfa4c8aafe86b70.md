

```
from hyperopt import fmin, tpe, hp, STATUS_OK
def objective(x):
    return {'loss': x, 'status': STATUS_OK }
best = fmin(objective,
    space=hp.uniform('x', -5, 5),
    algo=tpe.suggest,
    max_evals=50)
print(best)
```

我们可以通过使用 Trials 对象充分利用能够返回字典的能力，该对象允许我们在执行优化时存储更多信息。在下面的示例中，我们正在修改目标函数，使其返回更复杂的输出，该输出稍后将存储在 Trials 对象中。我们可以通过将其作为 Hyperopt fmin() 函数的参数传递来实现：

```
import pickle
import time
from hyperopt import fmin, tpe, hp, STATUS_OK, Trials
def objective(x):
    return {
        'loss': x,
        'status': STATUS_OK,
        'eval_time': time.time(),
        'more_data': {'type': None, 'value': [0, 1]},
        'attachments':
            {'time_module': pickle.dumps(time.time)}
    }
trials_obj = Trials()
best_params = fmin(objective,
    space=hp.uniform('x', -5, 5),
    algo=tpe.suggest,
    max_evals=50,
    trials=trials_obj)
print(best_params)
```

执行完成后，我们可以访问 Trials 对象以检查优化过程中目标函数返回的值。我们有不同的方式可以访问 Trials 对象中存储的值：

- trials.trials: 返回包含搜索所有参数的字典列表。
- trials.results: 目标函数返回的字典。
- trials.losses(): 所有成功试验的实际损失。
- trials.statuses(): 每个试验的状态。

此 Trials 对象可以保存为 pickle 对象或使用自定义代码进行解析，以便我们更深入地了解试验结果。

试验的附件可以这样访问：

```
msg = trials.trial_attachments(trials.trials[0])['time_module']
time_module = pickle.loads(msg)
```

在此示例中，我们正在获取第一个试验的 time_module 附件。

需要注意的一点是，在 Azure Databricks 中，我们可以选择将 Trials 和 SparkTrials 对象都传递给 fmin() 函数的 trials 参数。SparkTrials 对象在我们执行单机算法时传递，例如将 scikit-learn 模型用作目标函数。在这里，Trials 对象用于分布式训练算法，例如 MLlib 或 Horovod 模型。SparkTrials 类允许我们在无需引入任何自定义逻辑的情况下分发优化的执行。它还提高了计算向工作节点的分布。

不要将 SparkTrials 对象与为分布式训练设计的算法一起使用，因为这会导致执行在集群中并行化。

现在我们对如何配置 fmin() Hyperopt 函数的基本参数有了更清晰的认识，我们可以深入了解搜索空间是如何定义的。

## 定义搜索空间

我们可以将搜索空间表达为一组嵌套函数，它们定义了不同测试用例的各个搜索空间。例如，在以下代码块中，我们为 x 参数指定了搜索空间，其中包含两个测试空间 test_space_1 和 test_space_2：

```
from hyperopt import hp
```

```
search_space = hp.choice('a',
    [('test_space_1', 1 + hp.lognormal('x', 0, 1)),
     ('test_space_2', hp.uniform('y', -5, 5))]
)
```

这里，Hyperopt 将根据搜索算法从这些嵌套随机表达式中采样。此算法采用自适应探索策略，而不是仅从搜索空间中抽样。在前面的示例中，我们定义了三个参数：

- a: 选择要使用的情况
- test_space_1: 为 x 参数生成正值
- test_space_2: 为 y 参数生成正值

每个表达式的第一个参数是标签，算法在内部使用它将参数选择返回给调用者。在此示例中，代码的工作方式如下：

- 如果 a 变量为 0，我们将使用 x 而不使用 y。
- 如果 a 变量为 1，则将使用 y 而不使用 x。

x 和 y 变量是条件参数，取决于 a 的结果。如果我们以这种条件方式编码变量，我们就说明当 a 为 0 时 x 变量对目标函数没有影响，这有助于搜索算法更有效地分配信用。这利用了用户对要分析的目标函数的领域知识。

search_space 是一个变量空间，其定义为图表达式，描述如何在不生成搜索空间的情况下采样一个点。这优化了执行的内存和计算使用。这些图表达式称为 pyll 图，定义在 hyperopt.pyll 类中。

你可以通过对其进行采样来探索示例搜索空间：

```
import hyperopt.pyll.stochastic
print(hyperopt.pyll.stochastic.sample(search_space))
```

我们现在知道如何定义搜索空间，在该空间中我们将优化目标函数。需要注意的是，我们在搜索空间中定义测试用例的方式也会影响搜索算法在自适应搜索期间对试验的工作方式。在下一节中，我们将了解在 Azure Databricks 中使用 Hyperopt 的一些最佳实践。

## 在 Hyperopt 中应用最佳实践

到目前为止，我们已经讨论了如何指定要最小化的目标函数的执行以及我们将对其进行评估的搜索空间。此外，我们还看到在定义超参数可能值的探索方式方面有很大的灵活性，以及如何通过在搜索空间中定义条件参数来利用领域知识。

我们必须牢记 Hyperopt 库本身的一些方面，以及它在 Azure Databricks 中的执行方式。在 Azure Databricks 中使用 Hyperopt 优化目标函数时，这里有一些需要记住的重要概念：

- Hyperopt Tree of Parzen Estimator 算法是一种贝叶斯方法，比常见的网格和随机搜索方法更有效。它允许我们扫描更大的一组可能超参数。在定义搜索空间时应用领域知识以提高探索性能。
- 当我们使用 `hyperopt.choice()` 并使用 MLflow 跟踪优化进度时，MLflow 将记录选择列表的索引。我们可以使用 Hyperopt 的 `hyperopt.space_eval()` 函数获取参数值。
- 在处理大型数据集时，始终建议在数据的小子集上进行实验，以逐步了解最佳搜索空间。这有助于我们在整个数据集上定义将使用的搜索空间。
- 当我们使用 SparkTrials 对象时，建议使用仅 CPU 集群。这是因为在 Azure Databricks 中，与 CPU 集群相比，GPU 集群中的并行性会降低。
- 不要在自动缩放集群上使用 SparkTrials，因为并行值是在执行开始时选择的。因此，如果集群扩展，它不会提高执行的性能。

应用这些概念将确保我们在 Azure Databricks 中使用 Hyperopt 执行优化时充分利用其优势。在下一节中，我们将了解更多关于如何在 Azure Databricks 中改进深度学习模型的推理。

## 使用 scikit-learn、Hyperopt 和 MLflow 优化模型选择

正如我们在前几节中所看到的，Hyperopt 是一个 Python 库，允许我们在分布式计算环境（如 Azure Databricks）中跟踪可用于超参数模型调优的优化运行。在本节中，我们将通过一个训练 scikit-learn 模型的示例。我们将使用 Hyperopt 跟踪调优过程并将结果记录到 MLflow，即模型生命周期管理平台。

在 Azure Databricks Runtime for Machine Learning 中，我们拥有一个经过优化的 Hyperopt 版本，可支持 MLflow 跟踪。在这里，我们可以使用 SparkTrials 对象在并行执行期间记录单机模型的调优过程结果。我们将使用这些工具为多个 scikit-learn 模型找到最佳超参数集。

我们将执行以下操作：

- 准备训练数据集。
- 使用 Hyperopt 定义要最小化的目标函数。
- 定义 Hyperopt 搜索空间，我们将在其上扫描最佳超参数。
- 定义将用于扫描搜索空间的搜索算法。
- 执行优化并在 MLflow 中跟踪最佳参数集。

让我们开始：

1. 首先，我们将导入我们将使用的所有必要库：

```
import numpy as np
from sklearn.datasets import fetch_california_housing
from sklearn.model_selection import cross_val_score
from sklearn.svm import SVC
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression
from hyperopt import fmin, tpe, hp, SparkTrials, STATUS_OK, Trials
import mlflow
```

