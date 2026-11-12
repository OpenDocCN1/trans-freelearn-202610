## 参考文献

- [1] N. V. Chawla, K. W. Bowyer, L. O. Hall, 和 W. P. Kegelmeyer，“SMOTE：合成少数类过采样技术，”人工智能研究杂志，第 16 卷，第 321–357 页，2002 年 6 月。

## 使用 LightGBM 和 FLAML 进行 AutoML

在前一章中，我们讨论了两个案例研究，展示了处理数据科学问题的端到端示例。在典型数据科学生命周期涉及的步骤中，通常最耗时的任务是准备数据、找到正确的模型以及调优模型。

本章将探讨自动机器学习的概念。自动机器学习系统旨在自动化机器学习生命周期的某些或全部部分。我们将探讨 **FLAML**，这是一个使用高效超参数优化算法来自动化流程的模型选择和调优步骤的库。

最后，我们将介绍一个使用 FLAML 和另一个名为 Featuretools 的开源工具的案例研究。将讨论和展示 FLAML 的实际用法。我们还将展示 FLAML 的零样本 AutoML 功能，该功能完全跳过调优。

本章的主要主题如下：

- 自动机器学习简介
- 用于 AutoML 的 FLAML
- 案例研究 – 将 FLAML 与 LightGBM 结合使用

### 技术要求

本章包含示例和代码摘录，展示了将 FLAML 与 LightGBM 用于 AutoML 用例。本章的完整示例和设置合适环境的说明可在 https://github.com/PacktPublishing/Practical-Machine-Learning-with-LightGBM-and-Python/tree/main/chapter-7 上找到。

### 自动机器学习

**自动机器学习 (AutoML)** 是一个新兴领域，旨在自动化 ML 工作流的复杂方面，从而实现 ML 模型更高效、更易访问的部署。AutoML 的出现反映了人工智能和 ML 技术日益复杂以及它们渗透到各个行业和研究领域。它旨在减轻数据科学流程中一些复杂且耗时的方面，从而实现 ML 技术的更广泛使用和更易访问的应用。

对于精通 ML 和数据科学流程的软件工程师而言，ML 模型日益增加的复杂性和不断扩大的算法宇宙可能构成重大挑战。构建强大、高性能的模型需要大量的专业知识、时间和计算资源，以选择合适的算法、调优超参数并进行深入比较。AutoML 已作为这些挑战的解决方案出现，旨在自动化这些复杂且往往劳动密集的任务。

AutoML 还致力于民主化 ML 领域。通过抽象掉数据工程、模型构建和调优中的一些复杂性，AutoML 使经验较少的个人和组织能够利用这些强大技术。结果是，ML 可以在更广泛的环境中发挥作用，更多个人和组织能够部署 ML 解决方案。

AutoML 系统有不同复杂程度的可用性。尽管所有 AutoML 系统都旨在简化 ML 工作流，但大多数系统和工具只关注工作流的部分内容。通常，数据预处理、特征工程、模型选择和超参数调优等步骤会被自动化。这种自动化节省了时间，并可以通过系统地探索更全面的选项来提高 ML 模型的鲁棒性和性能，这些选项可能会因人类偏见或时间限制而被忽视或未被探索。

### 自动化特征工程

如在 *第 6 章，使用 LightGBM 解决现实世界数据科学问题* 中讨论的，数据清理和特征工程是 ML 工作流的关键部分。它们涉及处理不可用数据、处理缺失值以及创建可以输入模型的有意义特征。手动特征工程尤其具有挑战性且耗时。AutoML 系统旨在有效处理这些任务，实现自动特征提取和转换，从而构建更强大的模型。

数据清理自动化通常通过遵循针对异常值和缺失值等问题的特定知名技术来实现。在前面的章节中，我们手动应用了一些这些技术：异常值可以进行统计检验并限制或截断。缺失值通常使用描述性统计量（如均值或众数）进行插补。AutoML 系统要么使用启发式算法和测试来选择最佳数据清理技术，要么采用多种方法和测试，通过针对数据训练模型来找出效果最好的方法。



自动化特征工程的方法通常相似：将许多可能的变换应用于所有现有特征，并在建模后测试生成特征的有用性。关于变换如何生成特征的示例，可以在以下案例研究中找到。

自动化特征工程的另一种方法是基于特征的数据类型使用规则来提取特征。例如，可以从日期字段中提取日、周、月和年。或者，可以计算单词或句子特征的字符数、词元、词干或词嵌入。

如您所注意，特征工程自动化技术的应用依赖于手头特征的技术信息，通常是数据类型，以及与其他特征的相关性和关系。重要的是，在创建新特征时没有应用领域知识。这突显了 AutoML 的一个缺陷：它无法处理需要人类专业知识的特定、由领域驱动的决策。例如，考虑一个带有空腹血糖特征的糖尿病数据集。医疗专业人员（领域专家）知道，空腹血糖在 100 至 125 mg/dL 之间的个体被认为是糖尿病前期，而更高的则被认为是糖尿病。这个连续特征可以被工程化为特定类别：正常、糖尿病前期和糖尿病，从而简化建模数据。AutoML 系统无法实现这样的变换。

## 自动化模型选择与调优

AutoML 特别有用的领域是模型选择和超参数调优。鉴于可用算法众多，为特定数据集和问题选择最佳算法可能令人生畏。AutoML 系统使用各种技术，包括贝叶斯优化和元学习，来选择最佳模型。它们还自动调优超参数以最大化模型性能。

AutoML 系统可以提供自动交叉验证，降低过拟合风险，并确保模型对未见数据的泛化能力。一旦选择并训练了最佳模型，许多 AutoML 工具还可以帮助部署，使模型可用于对新数据进行推断。

在初始模型部署之外，一些 AutoML 解决方案还在持续模型监控和维护方面提供价值。随着现实世界数据的演变，模型可能会发生漂移，其性能可能会下降。AutoML 可以帮助监控模型性能并根据需要重新训练模型，确保您的 ML 系统在长期内保持有效。

## 使用 AutoML 系统的风险

如前所述，AutoML 系统通常不使用领域知识来辅助特征工程、模型选择或其他自动化。相反，采用的是一种蛮力或试错的尝试方法。

一些 AutoML 系统的“黑盒”特性也使得难以解释系统做出的决策，因此不太适合需要高度可解释性的应用。

因此，让数据科学家或领域专家参与其中，与 AutoML 系统协作，以识别并利用领域知识可以带来更好模型的机会，这仍然至关重要。然而，AutoML 系统有时会在科学家和数据之间创建额外层，从而阻碍而非支持数据科学家。

我们已经看到一个 AutoML 框架的实际应用。Optuna（我们在 *第 5 章，使用 Optuna 优化 LightGBM 参数* 中讨论过）是专注于超参数调优的 AutoML 框架示例。在下一节中，我们将讨论另一个 AutoML 框架：FLAML。

## 介绍 FLAML

**FLAML**（**快速且轻量级 AutoML**）是由 Microsoft Research 开发的 Python 库 [1]。它旨在以低计算成本自动生成高质量的 ML 模型。FLAML 的主要目标是尽量减少调优超参数和识别最佳 ML 模型所需的资源，使 AutoML 更加易于使用且成本效益更高，特别是对于预算有限的用户。

FLAML 提供了几个关键特性，使其与众不同。其中之一是其效率。它为 ML 任务提供了快速且轻量级的解决方案，最大限度地减少所需的时间和计算资源。它在不牺牲所生成模型质量的前提下实现了这一点。FLAML 还强调其在各种 ML 算法和各种应用领域中的多功能性。

FLAML 效率的核心在于其新颖且成本效益高的搜索算法。这些算法智能地探索超参数空间，最初专注于“廉价”配置。随着对搜索空间了解的深入，它会逐步探索更“昂贵”的配置。这确保了探索与利用之间的平衡，在用户指定的时间和资源预算内交付优化的模型。

FLAML 在模型选择过程方面也表现出色。它支持各种 ML 算法，包括 XGBoost、LightGBM、CatBoost、RandomForest 和各种线性模型。该库可以自动为给定数据集选择最佳算法并优化其超参数，无需大量手动干预即可为用户提供最佳模型。

FLAML 提供了简单直观的 API，可与现有的基于 Python 的数据科学和 ML 工作流无缝集成。用户指定数据集、时间预算（以秒为单位）和优化任务，FLAML 将处理其余事项。这种用户友好性和效率使其成为 ML 初学者和寻求加速工作流程的资深从业者的实用选择。

FLAML 效率背后的新颖之处在于其 **超参数优化**（HPO）算法。FLAML 提供两种 HPO 算法：**成本节约优化** 和 **混合搜索**。

### 成本节约优化

**成本节约优化**（CFO）是一种利用随机直接搜索来探索超参数空间的局部搜索方法 [2]。CFO 算法从低成本超参数配置开始（例如，对于 LightGBM，低成本配置可能包含较少的增强树）。它在超参数空间中朝向更高代价参数区域进行固定次数迭代的随机步进。

CFO 的步长是自适应的，这意味着如果连续几次迭代没有改进，算法会降低步长。这样做可以避免在高代价且不具前景的方向上采取大步长。

CFO 还利用随机重启。作为一种局部搜索算法，CFO 可能会陷入局部最优解。如果没有进展且步长已经很小，算法会从随机点重新启动。

总之，CFO 会快速（使用大步长）尝试到达搜索空间中更有前景的区域，同时尽可能少地使用优化预算（通过从低成本区域开始）。只要优化预算允许，CFO 就会继续搜索，如果发生停滞，则在随机区域重新启动。FLAML 允许用户以秒为单位设置优化预算。

### BlendSearch

FLAML 在 BlendSearch 中提供了 CFO 算法的替代方案。BlendSearch 通过使用多线程方法同时运行全局和本地搜索过程，与 CFO 有所不同 [3]。

与 CFO 类似，BlendSearch 从低成本配置开始并进行本地搜索。然而，与 CFO 不同，BlendSearch 不会等待本地搜索停滞后再探索新区域。相反，全局搜索算法（例如贝叶斯优化）会不断建议新的起点。起点根据其与现有点的距离进行筛选，并按成本优先级排序。

BlendSearch 的每次迭代随后根据上次迭代的性能，选择是继续本地搜索还是在新的全局搜索点开始。与 CFO 类似，全局搜索方法提出的配置会经过可行性验证。

由于 BlendSearch 使用全局优化，它不太容易陷入局部最小值。如果超参数搜索空间高度复杂，则推荐使用 BlendSearch 而非 CFO。通常最好先尝试 CFO，仅在 CFO 遇到困难时才切换到 BlendSearch。

### FLAML 局限性

尽管有其优势，FLAML 也有其局限性。该库的自动化流程可能无法始终胜过专家的手动调优，尤其是针对复杂的特定领域任务。此外，与其他 AutoML 解决方案一样，生成模型的可解释性可能具有挑战性，尤其是在处理增强树或神经网络等模型时。

FLAML 只执行 ML 过程中的模型选择和调优部分。这些是模型开发中最耗时的部分之一，但 FLAML 不提供执行特征工程或数据准备的功能。




以下部分介绍了使用 FLAML 与 LightGBM 的案例研究，展示了日常使用场景、不同优化算法以及 FLAML 的零样本 AutoML。

## 案例研究 – 使用 FLAML 与 LightGBM

我们将使用前一章中的风力涡轮机数据集进行案例研究。数据集的清理方式与之前相同，填充缺失值并将异常值限制在适当范围内。然而，我们采用了不同的特征工程方法。为了进一步探索 AutoML，我们使用一个名为 Featuretools 的开源框架。

## 特征工程

**Featuretools**（`https://featuretools.alteryx.com/en/stable/#`）是一个用于自动化特征工程的开源框架。具体来说，Featuretools 非常适合转换关系数据集和时间序列数据。

如前一节所述，自动化特征工程工具通常使用特征的组合转换来为数据集生成新特征。Featuretools 通过其 **深度特征合成 (Deep Feature Synthesis, DFS)** 过程支持特征转换。

例如，考虑一个在线客户 Web 会话数据集。此类数据集中可能有用的典型特征包括客户访问网站的总会话次数，或客户注册的月份。使用 Featuretools 和 DFS，可以通过以下代码实现（由 `https://featuretools.alteryx.com/` 提供）：

```
feature_matrix, feature_defs = ft.dfs(
    entityset=es,
    target_dataframe_name="customers",
    agg_primitives=["count"],
    trans_primitives=["month"],
    max_depth=1,
)
feature_matrix
```

此处应用了两种转换：一种用于“月份”的转换，另一种用于“计数”的聚合。使用这些转换，会自动从客户存在的任何日期中提取月份（例如加入日期），并为每个客户计算计数聚合（例如会话或交易次数）。Featuretools 提供了丰富的转换和聚合集合。完整列表可在 `https://featuretools.alteryx.com/en/stable/api_reference.html` 上找到。

让我们看看如何使用 Featuretools 为风力涡轮机数据集工程化特征。

## 将 Featuretools 与风力涡轮机数据集结合使用

我们必须为我们的数据集执行两项特征工程任务：为日期时间字段工程化特征以及对分类特征进行编码。为了开始，我们为数据创建一个 EntitySet：

```
es = ft.EntitySet(id="wind-turbine")
es = es.add_dataframe(
    dataframe_name="wind-turbine",
    dataframe=df,
    index="tracking_id"
)
```

EntitySet 告诉 Featuretools 框架我们在数据中使用的实体和关系。前面示例中，Customer 是一个实体示例；在此例中，它是风力涡轮机。然后我们传递数据帧和要用作索引的列。

然后我们应用 dfs 和 encode_features 为我们的数据集工程化特征：

```
feature_matrix, feature_defs = ft.dfs(
    entityset=es, target_dataframe_name="wind-turbine",
    trans_primitives=["day", "year", "month", "weekday"],
    max_depth=1)
feature_matrix_enc, features_enc = ft.encode_features(
    feature_matrix, feature_defs)
```

前面的代码为我们的每个风力涡轮机测量提取了日期、年份、月份和星期。然后特征编码会自动对数据集的分类特征进行独热编码，包括新的日期字段。

以下是数据集列列表的摘录，显示了 Featuretools 创建的一些列：

```
...
'cloud_level = Low',
'cloud_level = Medium',
'cloud_level = Extremely Low',
'cloud_level is unknown',
'MONTH(datetime) = 1',
'MONTH(datetime) = 2',
...
'MONTH(datetime) = 8',
'MONTH(datetime) = 9',
'MONTH(datetime) = 11',
'MONTH(datetime) is unknown',
...
'YEAR(datetime) = 2019',
'YEAR(datetime) = 2018',
'YEAR(datetime) is unknown'
```

请注意分类特征的独热编码：每个值现在被拆分为单独的列。这包括未知值的列（例如，YEAR(datetime) is unknown），这说明了处理分类特征中缺失值的另一种方法。我们不是使用众数之类的方法填充值，而是有一个列向模型（true 或 false）发出值缺失的信号。

自动化特征工程已将我们的列数从 22 列增加到 66 列。这说明了自动化特征工程和 AutoML 的另一个一般性注意事项：自动化可能导致过于复杂的数据集。在第 6 章“使用 LightGBM 解决现实世界的数据科学问题”中，我们可以根据对学习算法的理解有选择地对特征进行编码。LightGBM 可以自动处理分类特征；因此，如果 LightGBM 是唯一使用的学习算法，独热编码就是多余的。

此外，日期字段可以数值化处理。通过应用我们对问题和算法的了解，我们可以减少学习问题的维度，从而简化它。自动化系统的易用性需要与专家特征工程的手动工作量相权衡，这日后可能节省时间。

数据集现在已准备好进行模型开发；我们只用两行代码就完成了数据集的特征工程。

## FLAML AutoML

我们现在将研究模型选择和调优。我们将使用 FLAML 比较五个不同的模型：LightGBM、RandomForest、XGBoost、ExtraTrees 和 XGBoost 的限制深度版本。我们还希望为最佳模型找到最优参数。使用 FLAML，整个过程只需两行代码即可完成：

```
automl = flaml.AutoML()
automl.fit(X, y, task="regression", time_budget=60)
```

前面的代码在 60 秒的时间预算内拟合一个最优回归模型。FLAML 自动使用保留集计算验证结果，然后继续优化，默认使用 CFO 作为调优器。

AutoML 类提供“面向任务的 AutoML”。用户设置学习任务，FLAML 处理其余事项。支持的任务包括：分类、回归、时间序列预测和时间序列分类、排名，以及 NLP 相关任务，如摘要化和单词标记分类。

对 fit 的调用是可自定义的。例如，我们可以按如下方式自定义：

```
automl = flaml.AutoML()
custom_hp = {
    "learning_rate": {
        "domain": flaml.tune.loguniform(0.0001, 0.05)
    }
}
automl.fit(X, y, task="regression", time_budget=120,
    metric="mse",
    estimator_list=['lgbm', 'xgboost', 'rf'],
    custom_hp={
        "lgbm": custom_hp
    },
    hpo_method="bs")
```

在此，我们通过显式将学习率声明为范围内的对数缩放均匀变量来自定义超参数搜索空间。设置参数搜索空间的其他选项包括均匀采样、随机整数采样以及针对分类参数的基于选择的采样。

此外，我们将估计器列表设置为仅关注三种建模算法：LightGBM、随机森林和 XGBoost。最后，我们可以自定义 HPO 算法，在此，我们将其设置为 BlendSearch，它使用前面讨论的多线程优化方法。

完整的自定义列表可在 https://microsoft.github.io/FLAML/docs/reference/automl/automl/#automl-objects 上找到。

调用 fit 后，我们可以像使用其他模型一样使用 AutoML 训练的模型。FLAML 提供了 scikit-learn 风格的 API，用于预测和基于概率的预测（用于分类问题）。

以下代码从给定数据创建预测并计算指标和特征重要性：

```
y_pred = automl.predict(X)
print (f"r2: {1 - sklearn_metric_loss_score('r2',
    y_pred, y)}")
print (f"MSE: {sklearn_metric_loss_score('mse',
    y_pred, y)}")

r2: 0.9878605489721696
MSE: 0.08090827806554425
```

我们还可以通过调用以下内容获取获胜模型以及每个试用模型的最佳超参数配置：

```
print(automl.best_config)
print(automl.best_config_per_estimator)
print(automl.time_to_find_best_model)
```



FLAML的一个值得注意的功能是零样本AutoML，它完全绕过了对模型调优的需求。

## 零样本 AutoML

**零样本AutoML** 是 FLAML 的一项功能，其中不执行超参数优化。相反，通过分析算法在各种数据集上的性能，离线确定合适的超参数配置。该过程可描述如下：

- 1. 在构建模型之前：
  - 使用 AutoML 在多个数据集上训练模型
  - 将所有数据集的超参数配置、评估结果和元数据存储为零样本解决方案

- 2. 为新问题构建模型时：
  - 使用 FLAML 将新数据集与零样本解决方案结果进行对比分析，并确定合适的超参数
  - 使用超参数在新数据集上训练模型

对于给定的模型类型（例如 LightGBM），第一步仅执行一次。此后，可以为任何新问题构建新模型而无需调优。该解决方案被称为“零样本”，因为合适的参数在新数据集的首次拟合时即被使用。

FLAML 的零样本 AutoML 方法具有许多优势：

- 如前所述，不涉及调优，在解决新问题时节省了大量计算工作量和时间
- 由于不需要调优，也不需要验证数据集，更多数据可用于训练
- 用户的参与度更低
- 通常不需要更改代码，如我们将在下一部分看到

当然，为模型类型创建零样本解决方案仍然艰巨，需要各种数据集和大量计算来训练许多模型。幸运的是，FLAML 为许多流行模型提供了预训练的零样本解决方案，包括 LightGBM、XGBoost 和 scikit-learn 的随机森林。

要使用零样本解决方案，请将常规的 LightGBM 导入替换为 FLAML 包装的版本：

```
from flaml.default import LGBMRegressor

zs_model = LGBMRegressor()
zs_model.fit(X, y)
```

调用 fit 会分析 X 中的数据，选择合适的参数，并使用这些参数训练模型。训练仅执行一次，不进行调优。

这结束了我们对 FLAML 的案例研究。正如我们所见，FLAML 为复杂的模型选择和调优功能提供直观的 API，这在处理机器学习问题时可以节省大量精力。

## 摘要

总而言之，本章讨论了 AutoML 系统及其用途。讨论了自动化特征工程、模型选择和调优的典型方法。我们还提到了使用这些系统相关的风险和注意事项。

本章还介绍了 FLAML，这是一个用于 AutoML 的库，提供用于自动化模型选择和调优过程的工具。我们还介绍了 FLAML 提供的两种高效超参数优化算法 CFO 和 BlendSearch。

本章以案例研究的形式展示了应用 FLAML 的实际操作。除了 FLAML，我们还展示了一个名为 Featuretools 的开源工具，它提供自动化特征工程的功能。我们展示了如何使用 FLAML 在固定时间预算内开发优化模型。最后，我们提供了使用 FLAML 零样本 AutoML 功能的示例，该功能通过针对已知问题的配置分析数据集来确定合适的超参数，从而消除了模型调优的需求。

下一章将讨论围绕 LightGBM 模型构建机器学习管道，重点是将 LightGBM 模型导出、打包并部署到生产环境。

## 参考文献

- [1] C. Wang, Q. Wu, M. Weimer, and E. Zhu, “FLAML: A Fast and Lightweight AutoML Library,” in MLSys, 2021.
- [2] Q. Wu, C. Wang and S. Huang, Frugal Optimization for Cost-related Hyperparameters, 2020.
- [3] C. Wang, Q. Wu, S. Huang, and A. Saied, “Economical Hyperparameter Optimization With Blended Search Strategy,” in ICLR, 2021.
