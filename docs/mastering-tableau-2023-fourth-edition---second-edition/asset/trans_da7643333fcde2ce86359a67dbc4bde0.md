
# 分位数分析练习

## 步骤

### 1. 创建分位数参数
创建并显示一个名为 `Number of quantiles` 的参数，设置如下：

![](Images/B18435_15_25.png)

图 15.25：分位数参数

### 2. 显示参数
右键单击创建的参数，选择 `Show Parameter`。

### 3. 创建分位数计算字段
创建一个名为 `Quantiles` 的计算字段，代码如下：

```
SCRIPT_REAL("
x <- .arg1;
y <- .arg2[1];
m <- c(1:y)/y;
n <- length(x);
z <- c(1:n); for (i in c(1:n)) z[i] <- 0;
for (j in c(1:y)) for (i in c(1:n)) z[i] <- if (x[i] <= quantile(x,m)[j] && z[i] == 0 ) j else z[i];
z;"
, SUM(Sales), [Number of quantiles])
```

本练习结束时将解释该代码的详细内容。

### 4. 转换为离散型
右键单击新创建的计算字段 `Quantiles`，选择 `Convert to Discrete`。

### 5. 创建百分位数参数
创建并显示两个参数：`Select Percentile Bottom Range` 和 `Select Percentile Top Range`。两者使用以下设置：

![](Images/B18435_15_26.png)

图 15.26：百分位数参数

### 6. 显示百分位数参数
右键单击这两个新创建的参数，选择 `Show Parameter`。

### 7. 创建百分位数计算字段
创建一个名为 `Percentile` 的计算字段，代码如下：

```
RANK_PERCENTILE(SUM([Sales])) < [Select Percentile Top Range] 
AND
RANK_PERCENTILE(SUM([Sales])) > [Select Percentile Bottom Range]
```

### 8. 构建视图
将 `Quantiles` 拖至 `Columns` 货架，并将 `Compute Using` 设置为 `Customer Name`。

将 `Percentile` 拖至 `Filters` 货架，单击 `OK`，然后将 `Compute Using` 设置为 `Customer Name`。再次打开筛选器并选择值 `True`。

## 回顾
那么，让我们回顾一下迄今为止所做的工作。我们绘制了每个区域的销售值，然后添加了一个 R 脚本，从销售点云中创建分位数。我们还创建了一个参数，让我们选择要在视图中查看多少个分位数，因为我们了解到分位数不一定非要是四分位数，也可以是三分位数、五分位数或更多。

接下来我们将看到该视图的实际效果，并添加抖动。

## 使用抖动增强

### 1. 添加索引字段
您可能希望添加以下额外步骤以进一步增强可视化效果。要使用抖动，请创建一个索引计算字段 `Index()`，并将该字段放在 `Columns` 货架上。将 `Compute Using` 设置为 `Customer Name`。请务必取消选择 `Show Header`，以免 `Index` 在视图中显示。

### 2. 配置坐标轴
右键单击每个轴，选择 `Logarithmic` 并取消选择 `Include Zero`。您的屏幕上应显示如下内容：

![](Images/B18435_15_27.png)

图 15.27：分位数仪表板

## 仪表板解读
在上图中，您可以看到我们决定显示四个分位数（使用 `Number of quantiles` 参数），因此它们被称为四分位数。通过将 `Index()` 函数添加到列中，我们能够将分位数分成四个组成部分。从该仪表板得出的一个结论是，在每个四分位数中，西部地区的销售额最高，南部地区最低。然而，中部和南部地区在第三四分位数中非常接近。基于此信息，我们可以进一步分析这些特定地区发生了什么，以及为什么与其他地区相比，中部地区在该四分位数中的销售额下降了。

## 代码解释
现在我们已完成练习，让我们花点时间考虑一下本练习中所见代码的一些内容：

| 代码 | 说明 |
|------|------|
| `SCRIPT_REAL` | 此 Tableau 函数调用 R 引擎并返回浮点值。 |
| `x <- .arg1;` | `x` 是我们将基于其创建分位数的变量。在本例中，该变量为 `[Sales]`。 |
| `y <- .arg2[1];` | 此变量设置分位数计数。`[1]` 强制设置单个数字而非向量。 |
| `m <- c(1:y)/y;` | `m` 将概率从 `1:y` 均匀分布。 |
| `n <- length(x);` | 这设置了循环的大小。下文将讨论循环。 |
| `z <- c(1:n); for (i in c(1:n)) z[i] = 0;` | `z` 通过将所有内容设置为 0 来设置初始响应向量。 |
| `for (j in c(1:y)) for (i in c(1:n)) z[i] = if (x[i] <= quantile(x,m)[j] && z[i] == 0 ) j else z[i];` | 对于每个分位数，我们遍历 `z` 向量，并对每个条目测试 `x` 的值是否小于该分位数的上限。如果 `x` 之前已设置，则保留它。否则，`z[i]` = 该分位数 (`j`)。 |

在本练习中，您看到通过使用 R，可以扩展 Tableau 的默认功能（如聚类），并为仪表板用户提供更大的自由度，以回答使用仪表板时出现的新问题，因为他们可以更改参数并查看可视化效果的变化。现在我们已经了解了一些可能的 R 使用案例，是时候讨论一个值得您关注的问题了：将编程集成与 Tableau 结合使用所带来的性能挑战。

# 性能挑战

R 脚本是表计算。与所有表计算一样，这意味着您只能使用视图中的字段。此外，正确设置分区和寻址非常重要；否则，您可能会得到意外的结果。

在*分位数简介*部分，您可能已经注意到，使用 `Number of quantiles` 参数设置的分位数越多，显示结果所需的时间就越长。这是因为 R 在 `Quantile` 计算字段中为每个分位数运行一次循环迭代。例如，如果 `Number of quantiles` 参数设置为 `1`，循环仅启动一次。如果设置为 `2`，则运行两次，依此类推。经验法则是：R 代码针对每个分区执行一次。分区越多，性能越慢。因此，在使用 R 代码时，应尽可能减少分区数量。

这是本章的 R 部分。希望您喜欢，并能从中获得灵感，提出自己的想法来扩展我们迄今共享的功能。现在我们将继续介绍另一种非常著名的编程语言：Python。幸运的是，Tableau 也有与 Python 的集成层。

# Python 安装与集成

Python 是一种解释型编程语言，以其可读性强而闻名。首次发布于 1991 年，距今已有一段时间（比大多数人猜测的要长），由 Guido van Rossum 开发。`TabPy` 是一项外部服务，允许您将 Python 与 Tableau 连接——类似于 `Rserve`。通过使用 `TabPy`，您可以将 Tableau 仪表板中的字段传递给 Python，执行计算，并将结果作为新计算字段发送回 Tableau。或者，您也可以在计算字段中调用在 Python 中实现的函数。关于 `TabPy` 的更详尽文章请见此处：[https://tableaumagic.com/tableau-and-python-an-introduction](https://tableaumagic.com/tableau-and-python-an-introduction)。

安装 Python 通常不难，但不仅仅是双击可执行文件那么简单。要成功将 Tableau 与 Python 连接，您可能需要安装一些库并在命令行上执行命令。以下段落将指导您完成此过程。

## 安装 Python

安装 Python 的最简单方法是执行以下步骤：

1.  下载例如带有 Python 3.10 版本的 Anaconda：[https://www.anaconda.com](https://www.anaconda.com)。任何其他 Python 解释器也可以。

## 集成 TabPy

要集成 Python 和 Tableau，需要一些额外步骤：

1.  打开例如 Anaconda 中的 Jupyter Notebook 并编写：

    ```
    !pip install tabpy
    ```

    或者，在您的 Python 目录中于命令行或终端（Mac 系统）执行以下命令：

    ```
    pip install tabpy
    ```
