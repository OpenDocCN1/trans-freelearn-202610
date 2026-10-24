
在下一节中，我们将使用Python编程探索实际的机器学习（ML）。

## 使用scikit-learn进行机器学习

要开发模型，我们需要数据集。网络抓取再次成为收集所需数据并存储为相关格式的理想技术。Python中有大量与机器学习相关的库和框架，而且数量还在不断增加。scikit-learn是一个Python库，能够处理并帮助完成大多数监督式机器学习功能。

scikit-learn也被称为并使用为`sklearn`。它构建于`numpy`、`scipy`和`matplotlib`之上。该库提供了大量与机器学习方面相关的功能，如*分类*、*聚类*、*回归*和*预处理*。我们将使用scikit-learn探索*监督学习*类型中的*回归*，涵盖初学者和中级概念。你还可以在https://scikit-learn.org/stable/user_guide.html查看`sklearn`用户指南。

在本章前面的章节中，我们已经介绍了大量关于**回归**的信息。回归是一种监督学习技术，用于基于标记数据做出预测。在接下来的章节中，将通过几个示例探讨简单线性回归（SLR）。

## 简单线性回归

SLR用于估计自变量与单个因变量之间的关系。它通常被用作默认的回归技术，基于另一个变量的值来预测一个变量的值，这个另一变量也被称为SLR的预测变量或输入变量。

现在我们将使用代码讨论一些SLR概念。代码文件可在此找到：https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter11/Chapter11_regression.ipynb。

示例中使用的数据（`Fish.csv`）来自**Kaggle**（https://www.kaggle.com/datasets/aungpyaeap/fish-market/download），并使用pandas读取到名为`input`的DataFrame中。图表将使用`plotly`绘制。

如下代码所示，我们导入常规库，如`pandas`、`numpy`和`plotly`。`Fish.csv`数据文件已使用pandas的`read_csv()`方法读取，这将创建一个名为`input`的DataFrame类型对象：

```
import pandas as pd
import numpy as np
import plotly.express as px
pd.options.plotting.backend = "plotly" # use plotly as plotting option
input = pd.read_csv("Fish.csv")      # load data
```

`input`包含几种鱼类物种的数据，包括与测量相关的数据，具体如下：

- 物种：物种名称
- 长度1：垂直长度（cm）
- 长度2：对角长度（cm）
- 长度3：水平长度（cm）
- 重量：鱼的重量（g）
- 宽度：对角宽度（cm）

pandas的`info()`方法，如图11.1所示，显示列名、Dtype（数据类型，如object或float）以及来自DataFrame `input`的null或非null计数等信息：

```
input.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 159 entries, 0 to 158
Data columns (total 7 columns):
#   Column   Non-Null Count   Dtype
---  ------   --------------   -----
0   Species  159 non-null     object
1   Weight   159 non-null     float64
2   Length1  159 non-null     float64
3   Length2  159 non-null     float64
4   Length3  159 non-null     float64
5   Height   159 non-null     float64
6   Width    159 non-null     float64
dtypes: float64(6), object(1)
memory usage: 8.8+ KB
```

图11.1：Fish.csv数据集信息

从图11.1可以看出，数据集中共有七列。其中六列是浮点类型，总共159行记录。数据集看起来很干净，因为没有null值。

研究数据集是必须的，以便关注预处理活动，如填充null值、删除重复项、转换数据类型和排序。这些活动是强制性的，因为在回归处理过程中，我们需要找到作为变量的独立数据和依赖数据，并且不能影响输出（预测）质量。

`input.describe()`代码只处理携带统计值的列。通过`include='all'`参数，甚至字符串和其他对象也可以包含在内，如图11.2所示。我们可以看到，在七个独特的物种中，Perch出现次数最多；在159次中出现56次。

```
input.describe(include='all')
```

|  | Species | Weight | Length1 | Length2 | Length3 | Height | Width |
|---|---|---|---|---|---|---|---|
| 计数 | 159 | 159.000000 | 159.000000 | 159.000000 | 159.000000 | 159.000000 | 159.000000 |
| 唯一值数 | 7 | NaN | NaN | NaN | NaN | NaN | NaN |
| 众数 | Perch | NaN | NaN | NaN | NaN | NaN | NaN |
| 频数 | 56 | NaN | NaN | NaN | NaN | NaN | NaN |
| 均值 | NaN | 398.326415 | 26.247170 | 28.415723 | 31.227044 | 8.970994 | 4.417486 |
| 标准差 | NaN | 357.978317 | 9.996441 | 10.716328 | 11.610246 | 4.286208 | 1.685804 |
| 最小值 | NaN | 0.000000 | 7.500000 | 8.400000 | 8.800000 | 1.728400 | 1.047600 |
| 25% | NaN | 120.000000 | 19.050000 | 21.000000 | 23.150000 | 5.944800 | 3.385650 |
| 50% | NaN | 273.000000 | 25.200000 | 27.300000 | 29.400000 | 7.786000 | 4.248500 |
| 75% | NaN | 650.000000 | 32.700000 | 35.500000 | 39.650000 | 12.365900 | 5.584500 |
| 最大值 | NaN | 1650.000000 | 59.000000 | 63.400000 | 68.000000 | 18.957000 | 8.142000 |

图11.2：数据集列的详细描述

使用以下代码绘制各个物种的计数（即`value_counts()`方法的结果）：

```
species_count = input.Species.value_counts()
species_count.plot(title="Species Count", labels=dict(index="Species_Name", value="Species_Count", variable="Fish"))
```

如图11.3所示，从DataFrame `input`中统计Species_Count（即某些物种出现的次数），并以物种名称绘制：

# 物种计数

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_281_0.png)

图11.3：绘制物种计数

在分析数据集时，似乎有许多相关因素。因此，我们将仅针对名为Perch的目标物种，通过基于Length3值训练模型来预测Weight：

```
perch = input[input.Species=="Perch"]
```

Plotly散点图支持通过`trendline`参数显示回归趋势（https://plotly.com/python/linear-fits/#linear-fit-trendlines-with-plotly-express），如下代码所示。该代码使用`ols`作为`trendline`参数：

```
fig = px.scatter(perch, x="Length3", y="Weight", trendline="ols")
fig.show()
```

如图11.4所示，我们可以找到线性方程以及计算出的R2值、系数、截距和所选Length3的预测重量。`trendline`的`ols`参数为我们计算了这些，我们可以从图11.4所示的图表中解读Perch的Length3与重量关系：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_282_0.png)

图11.4：Perch的OLS趋势线、Length3和重量

从图11.4中，我们可以获得SLR方程的以下列出属性：

- **R2：** 0.920652（准确率得分最高为1）
- **线性方程：** Weight = 35.0009 * Length3 + -652.787
- **截距：** -652.787
- **系数：** 35.0009

**重要说明**

R2和系数等值有助于我们验证模型。还应注意到我们尚未使用sklearn。

要使用sklearn通过`Length3`值（针对名为`Perch`的物种）预测`Weight`，我们需要执行以下步骤：

1. 确定因变量和自变量。
2. 将数据拆分为训练集和测试集。
3. 创建线性回归模型。
4. 使用训练数据拟合线性回归模型。
5. 最后，对测试数据进行预测。

如下代码所示，`sklearn`拥有多个模块，如`model_selection`、`linear_model`和`metrics`：

```
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score, mean_absolute_error
```

`train_test_split()`接收Length3和Weight值。这将创建训练集和测试集。训练集将占用`train_size`设置的80%的值，其余20%由测试集使用。还使用了额外参数`shuffle`，它会随机打乱可用记录的顺序：

```
X_train, X_test, y_train, y_test = train_test_split(perch.Length3,
perch.Weight, train_size=0.8, shuffle=True)
```

通过使用训练数据拟合创建名为`lr`的模型。`fit()`方法传入训练数据：

```
lr = LinearRegression().fit(X_train, y_train)
type(lr)
sklearn.linear_model._base.LinearRegression

dir(lr)
['_abc_impl', '...', '_more_tags', '_parameter_constraints',
'coef_', 'copy_X', 'feature_names_in_', 'fit', 'fit_intercept',
'get_params', 'intercept_', 'n_features_in_', 'n_jobs', 'positive',
'predict', 'rank_', 'score', 'set_params', 'singular_']
```

从前面的代码可以看出，创建了`lr`模型后，我们可以探索一些重要的`lr`属性，如下所示：

```
lr.intercept_    # array([-634.55508041])
lr.coef_         # array([[34.68344575]])
lr.rank_         # 1
lr.score(X_train, y_train)        # 0.927768856667214
```
