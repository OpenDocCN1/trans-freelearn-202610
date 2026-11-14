
# 我们向后移动一步，以确保不发生数据泄露

```python
df["rolling_3_mean"] = df["column"].shift(1).rolling(3).mean()
```

类似于滞后特征，我们需要对每个 `LCLid` 列分别执行此操作。我们在 `src.feature_engineering.autoregressive_features` 中提供了一个名为 `add_rolling_features` 的实用方法，它能以快速高效的方式为每个 `LCLid` 添加所有你想要的滚动特征。让我们看看如何使用它。

我们将导入此方法并使用其几个参数来按我们想要的方式配置滚动操作：

```python
from src.feature_engineering.autoregressive_features import add_rolling_features
full_df, added_features = add_rolling_features(
    full_df,
    rolls=[3, 6, 12, 48],
    column="energy_consumption",
    agg_funcs=["mean", "std"],
    ts_id="LCLid",
    use_32_bit=True,
)
```

现在，让我们看看在前面的代码片段中使用的参数：

*   **`rolls`**：该参数接受一个整数列表，表示我们需要计算聚合统计量的所有窗口。
*   **`column`**：要滞后的列的名称。在我们的例子中，是 `energy_consumption`。
*   **`agg_funcs`**：这是我们想要对 `rolls` 中声明的每个窗口执行的聚合操作列表。允许的聚合函数包括 `{mean, std, max, min}`。
*   **`n_shift`**：这是在进行滚动操作之前需要移动的时间步数。此参数可避免数据泄露。虽然我们现在只移动一步，但也存在需要移动多步的情况。这通常用于多步预测，我们将在*第 3 部分 - 预测机制*中介绍。
*   **`ts_id`**：包含时间序列唯一 ID 的列的名称。如果为 `None`，则假设 DataFrame 只包含单个时间序列。在我们的例子中，`LCLid` 是该列的名称。
*   **`use_32_bit`**：此参数在功能上不做任何操作，但能使 DataFrame 在内存中小得多，代价是牺牲浮点数的精度。

此方法返回添加了滚动特征的 DataFrame 以及一个包含新增特征列名的列表。

## 季节性滚动窗口聚合

季节性滚动窗口聚合与滚动窗口聚合非常相似，但它不是在窗口中取过去 *n* 个连续的观测值，而是取一个季节性窗口，在窗口中的每个项目之间跳过固定数量的时间步。下图将使这一点更清晰：

![图 6.4 - 季节性滚动窗口聚合（彩图）](media/6.4.jpg)

**图 6.4 - 季节性滚动窗口聚合（彩图）**

这里的关键参数是季节周期，通常称为 *m*。这是我们预期季节性模式重复所需的时间步数。当我们位于时间步 *t* 时，大小为 3 的滚动窗口将包含 ![](media/file174.png) 作为过去观测值的向量。但季节性滚动窗口会在窗口中的每个项目之间跳过 *m* 个时间步。这意味着季节性滚动窗口中的观测值将是 。和往常一样，一旦我们有了窗口向量，只需应用聚合函数即可得到一个标量值，并将其作为特征加入。

> **重要提示**
>
> 我们*不将* ![](media/file174.png) 作为季节性滚动窗口向量中的元素，以避免数据泄露。

这不是一个可以用 pandas 轻松高效完成的操作。一些花哨的 NumPy 索引和 Python 循环应该能搞定。我们使用来自 `github.com/jmoralez/window_ops/` 的实现，它利用 NumPy 和 Numba 使操作快速高效。

就像我们之前看到的特征一样，我们需要对每个 `LCLid` 分别执行此操作。我们在 `src.feature_engineering.autoregressive_features` 中提供了一个名为 `add_seasonal_rolling_features` 的实用方法，它能以快速高效的方式为每个 `LCLid` 添加所有你想要的季节性滚动特征。让我们看看如何使用它。

我们将导入该方法并使用其几个参数来按我们想要的方式配置季节性滚动操作：

```python
from src.feature_engineering.autoregressive_features import add_seasonal_rolling_features
full_df, added_features = add_seasonal_rolling_features(
    full_df,
    rolls=[3],
    seasonal_periods=[48, 48 * 7],
    column="energy_consumption",
    agg_funcs=["mean", "std"],
    ts_id="LCLid",
    use_32_bit=True,
)
```

现在，让我们看看在前面的代码片段中使用的参数：

*   **`seasonal_periods`**：这是一个应在季节性滚动窗口中使用的季节周期列表。在存在多重季节性的情况下，我们可以包含所有季节性的季节性滚动特征。
*   **`rolls`**：该参数接受一个整数列表，表示我们需要计算聚合统计量的所有窗口。
*   **`column`**：要滞后的列的名称。在我们的例子中，是 `energy_consumption`。
*   **`agg_funcs`**：这是我们想要对 `rolls` 中声明的每个窗口执行的聚合操作列表。允许的聚合函数为 `{mean, std, max, min}`。
*   **`n_shift`**：这是在进行滚动操作之前需要移动的季节性时间步数。此参数可避免数据泄露。
*   **`ts_id`**：包含时间序列唯一 ID 的列的名称。如果为 `None`，则假设 DataFrame 仅包含单个时间序列。在我们的例子中，`LCLid` 是该列的名称。
*   **`use_32_bit`**：此参数在功能上不做任何操作，但能使 DataFrame 在内存中小得多，代价是牺牲浮点数的精度。

和往常一样，该方法返回包含季节性滚动特征的 DataFrame 以及一个包含新增特征列名的列表。

## 指数加权移动平均 (EWMA)

使用滚动窗口均值操作时，我们计算的是窗口的平均值，这与**移动平均**同义。EWMA 是移动平均稍微聪明一点的“表亲”。移动平均考虑一个滚动窗口，并在计算平均值时同等对待窗口中的每个项目，而 EWMA 试图对窗口进行加权平均，且权重以指数速率衰减。有一个参数 ![](media/file177.png)，决定了权重衰减的速度。因此，我们可以将所有可用历史视为一个窗口，让该参数决定 EWMA 中包含多少近期信息。这可以简单地用递归形式写出，如下所示：

![](media/file179.png)

在这里，我们可以看到，![](media/file177.png) 的值越大，平均值越偏向近期值（请参阅*图 6.6* 以直观了解权重的分布）。如果我们展开递归，每一项的权重计算结果为：其中 *k* 是落后于 *t* 的时间步数。如果我们绘制权重图，可以看到它们呈指数衰减；![](media/file177.png) 决定了衰减的速度。理解 ![](media/file177.png) 的另一种方式是从**跨度** 的角度。跨度是衰减权重趋近于零所需的周期数（不是严格的数学意义上，而是直观上）。![](media/file177.png) 和跨度通过以下公式相关联：

下图将使这一点更清晰，我们在其中绘制了不同 ![](media/file177.png) 值下的权重衰减情况：

![图 6.5 - 不同值下的指数权重衰减](media/6.5.jpg)

**图 6.5 - 不同值下的指数权重衰减**

在这里，我们可以看到，当达到跨度时，权重变得很小。
