

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

