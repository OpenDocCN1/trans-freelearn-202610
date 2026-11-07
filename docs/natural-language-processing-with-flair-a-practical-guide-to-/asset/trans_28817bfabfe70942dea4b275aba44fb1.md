
## 网格搜索

这是一种穷举搜索（意味着它会探索整个搜索空间），简单地遍历预定义参数选项的所有可能组合。它明显受维度灾难的影响。网格搜索的优点是它可以完全并行化。

## 随机搜索

一种非穷举搜索算法，简单地采样随机超参数值并评估每个组合。它重复此过程指定次数的步骤并返回导致最佳模型的一组超参数值。

## 贝叶斯优化

一种使用贝叶斯定理执行搜索以找到最佳超参数集的技术。它构建一个概率模型，将超参数值映射到 `objective`（损失）函数。

贝叶斯优化的一个特殊变体是 **Tree-structured Parzen Estimator** (**TPE**)。它常用于流行的 Python 超参数优化 `hyperopt` 库中。该库的源代码和文档可以在 `https://github.com/hyperopt/hyperopt` 找到。

## 进化算法

超参数优化方法的另一种思路是进化算法方法。该算法遵循以下步骤：

- 1. 生成一组随机选择的超参数值。
- 2. 为种群中的每个个体成员训练并评估模型。
- 3. 用新形成的个体替换得分最低的个体，这些个体是表现良好的个体的突变或交叉变体（意味着多个个体的组合）。
- 4. 跳到步骤 2 并重复，直到获得期望的分数、看不到改进或达到迭代限制。

我们现在对超参数调优的工作原理有了基本了解。但如果想象手动实现，所有这些新想法和概念听起来可能有点令人生畏。幸运的是，有很多方法可以利用 Python 中现成的库来实现这一点。

## Python 中的超参数调优

让我们感受一下超参数调优在实践中是什么样的。我们将使用最流行的 Python 超参数优化库之一 Hyperopt。首先，让我们对如何在实践中使用它有一个大致的了解。

Hyperopt 是一个 Python 库，提供易于使用的 API，需要以下三个对象：

- 搜索空间
- 目标函数
- 优化方法

让我们详细查看每个要求：

- **搜索空间** 只是优化器将在其中搜索不同超参数选项的空间。库提供以下参数表达式：
  - **分类参数** – 参数值纯粹是分类的，甚至可以是非标量（它们不需要是数字）。它们通过 `hyperopt.hp.choice` 方法提供。
  - **整数参数** – 通过诸如 `hyperopt.hp.quniform` 或 `hyperopt.hp.qlognormal` 等方法获得的整数值参数。
  - **浮点参数** – 通过诸如 `hyperopt.hp.normal`（从正态分布采样）和 `hyperopt.hp.uniform`（从均匀分布采样）等方法获得的十进制浮点数。
- **目标函数** 确定特定超参数值组合的性能如何。在深度学习中，这通常是损失函数。
- **优化方法** 是在搜索空间内执行搜索的算法。hyperopt 中的选项有：随机搜索（`hyperopt.rand.suggest`）、TPEs（`hyperopt.tpe.suggest`）和 Adaptive TPEs（`hyperopt.atpe.suggest`）。

前述三个概念实际上就是 Hyperopt 为您找到最佳超参数所需的一切。让我们将其作为 Python 中的动手练习付诸实践。

## Python 中超参数调优动手实践

为了说明超参数调优在其最简单形式中的工作原理，我们将提出一个非常简单的人工问题。我们将使用仅依赖于该单个超参数的简单目标函数来优化单个超参数，并且我们已经知道其最优（最小）值。让我们选择这个简单的二次函数：

$f(x) = x^2 + 1$

![](img/c56c48b8fd18a11ab705352d618ed976_102_0.png)

图 6.1 – $x^2 + 1$ 函数

很明显 $x^2 + 1$ 的最小值是 $x = 0$ 时的 1。这现在使我们能够在 Python 中定义目标函数：

```
def objective(value):
    return value ** 2
```

然后我们可以为单个超参数定义搜索空间。为了保持简单，我们将选择均匀分布并采样一个我们知道最小值所在的范围，例如 -100 到 100：

```
from hyperopt import hp
```

```
# define the search space for a single hyperparameter
space = hp.uniform('param1', -100, 100)
```

前面的代码片段将在您之前安装了 Flair 的情况下成功执行。如果没有，请确保在运行之前在控制台中运行 pip3 install flair==0.11。

最后，我们现在可以选择优化方法：

```
from hyperopt import tpe

# the Tree of Parzen Estimators (TPE) algorithm
optimization_method = tpe.suggest
```

定义了所有三个必需对象后，我们可以启动优化过程并打印结果：

```
from hyperopt import fmin

# minimize the objective function over 1000 evaluations
best_params = fmin(objective,
                space,
                algo=optimization_method,
                max_evals=1000)

# print the best hyperparameter value
print(best_params)
```

前面的代码片段打印出以下内容：

```
{'param1': 0.01084844145467706}
```

这表明，根据 Hyperopt，我们目标函数的最佳值是 0.01084844145467706。该值非常接近我们已知的实际最佳值 0。获得的值也比随机搜索的预期最佳值更接近实际最佳值（0）。这意味着 TPE 在优化我们的目标函数方面做得很好。

## Flair 中的超参数优化

我们现在对超参数调优的工作原理有了高层次的理解。当然，在现实世界中，超参数并不是用于寻找简单二次函数的最小值。让我们了解一下它们在 Flair 中用于训练序列标注模型的方式。

正如我们在 *第 5 章，训练序列标注模型* 中学到的，模型训练的成功往往取决于大量正确设置的超参数。这就产生了找到能产生最佳性能的超参数值集的需求。我们可以使用超参数优化来实现这一点。幸运的是，Flair 开箱即用地提供超参数调优。让我们学习如何操作。

Flair 中的超参数优化本质上是 Hyperopt 的一个包装器，我们在上一节中简要介绍了它。该包装器的额外优势是它已经将一些序列标注特定信息馈送到 Hyperopt，因此我们不必这样做。在简陋的 Hyperopt 编码练习中，我们必须提供所有三个对象：搜索空间、优化方法和目标函数。但在 Flair 中，我们只需要定义搜索空间，框架就会完成其余工作。

## Flair 中超参数调优动手实践

Flair 中的超参数优化最好通过代码来解释。因此让我们做一个简单的编码练习，我们训练一个英语 PoS 标注器并使用 Flair 的超参数优化功能找到最佳模型的隐藏层大小和学习率。

我们将通过以下步骤实现这一点：

- 1. 加载语料库
- 2. 定义搜索空间
- 3. 定义 `SequenceTaggerParamSelector` 对象
- 4. 启动优化过程

我们首先，就像在 *第 5 章，训练序列标注模型* 中的序列标注练习一样，需要将英语 Corpus 对象加载到内存中。它包含一个带标签的英语语料库，分为测试、训练和开发数据集：

```
from flair.data import Corpus
from flair.datasets import UD_ENGLISH

corpus = UD_ENGLISH()
```
