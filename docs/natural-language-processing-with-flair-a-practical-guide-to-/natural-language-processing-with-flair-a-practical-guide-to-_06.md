## 7 训练自己的嵌入

Flair 序列标注器之所以能取得如此出色的性能，主要是归功于其秘密武器 – Flair 嵌入。它们的上下文设计、基于字符的事实以及在反向-前向配置中使用的[content]
```
tag_type = 'pos'
### 将语料库下采样到原始大小的 1%
corpus.downsample(0.01)
```

在前面的代码片段中，我们将英语语料库加载到 corpus 变量中，将标签类型定义为 PoS 标注，然后将语料库下采样到原始大小的 1%，以加快演示目的的训练过程。

然后我们可以使用 hp.choice 参数表达式定义搜索空间：

```
from flair.embeddings import WordEmbeddings
from flair.hyperparameter.param_selection import \
    SearchSpace, Parameter
from hyperopt import hp

search_space = SearchSpace()
search_space.add(Parameter.HIDDEN_SIZE,
                 hp.choice,
                 options=[128, 256])
search_space.add(Parameter.LEARNING_RATE,
                 hp.choice,
                 options=[0.05, 0.1, 0.15, 0.2])
search_space.add(Parameter.EMBEDDINGS,
                 hp.choice,
                 options=([WordEmbeddings('en')]))
```

前面的代码片段定义了我们的搜索空间，由三个超参数组成。我们决定为学习率和模型的隐藏层大小想出一些可能有用的值。尽管使用诸如 hyperopt.hp.quniform 和 hyperopt.hp.uniform 之类的参数表达式可能会产生更好的结果，但它们也会大大增加搜索空间的大小。我们的搜索空间现在非常小。只有八种可能的超参数组合（两个乘以四个，因为有两个隐藏层大小参数选项和四个学习率选项）。

> **重要说明**
> 在进行超参数优化时，Flair 中的某些超参数是强制性的。这包括 `Parameter.EMBEDDINGS` 和 `Parameter.LEARNING_RATE` 参数。如果您不想优化这些参数，只需为这些超参数的搜索空间传递一个选项即可。

我们现在准备定义我们的 SequenceTaggerParamSelector 对象：

```
from flair.hyperparameter.param_selection import \
    SequenceTaggerParamSelector

param_selector = SequenceTaggerParamSelector(corpus,
                                              tag_type,
                                              'paramopt')
```

这使用我们加载的语料库、我们定义的 PoS 标签类型以及 `paramopt` 文件夹名称（相对于运行代码的目录）定义了我们的超参数优化对象，超参数调优结果将写入该文件夹。

我们现在只需通过传入搜索空间和要执行的评估次数来启动优化过程。在我们的例子中，搜索空间是离散的，数字选项明确定义为 8。然而，在大多数情况下，使用非离散参数值时，可能的超参数组合数量是无限的，评估次数需要通过经验来选择：

```
param_selector.optimize(search_space, max_evals=8)
```

前面的代码将使用 8 种不同的超参数组合训练 8 个不同的序列标注器。它将使用 loss 作为 `objective` 函数并在开发数据集上进行评估。它将为每次运行训练 50 个 epoch。Flair 对 Hyperopt 的实现使用 TPE 优化器。在普通 CPU 上，执行时间大约在 10 到 30 分钟之间。

超参数优化完成后，结果将写入 `paramopt/param_selection.txt`。该文件包含每次评估运行的详细信息以及以下摘要：

```
best parameter combination
    embeddings: 0
hidden_size: 1
learning_rate: 3
```

前面的摘要表明，最佳隐藏层大小是第二个选项（因为摘要显示为 1，我们使用零索引，这意味着 1 指的是第二个选项）：256。最佳学习率是第四个选项 – 0.2。对于嵌入，当然，因为我们只有一个选项可供选择，因此第一个也是唯一一个选项被选为最佳。

这是否意味着我们应该始终将隐藏层大小用作 256，并将 0.2 作为英语的最佳学习率？简短的回答是否定的。最佳超参数值将取决于使用的数据、试图解决的问题以及用于解决该问题的技术。还记得前面我们加载语料库并将其从 100% 减少到原始大小的 1% 的代码片段吗？通过这样做，我们大大提高了模型训练的速度，这使我们能够更快地进行超参数优化，但与此同时，我们也将模型集中在非常小且可能有偏的数据子集上。要对英语数据集进行适当的超参数调优，我们需要在 100% 语料库大小下进行，并考虑更多超参数选项。这将大大增加我们的搜索空间和执行时间，但也会提高我们模型的性能。这些发现导致了以下结论。

## 摘要

超参数调优是一种非常有用的技术，可用于寻找最佳超参数，不仅在序列标注中，而且在机器学习的许多领域中。然而，它是有代价的；它非常昂贵。从本质上讲，它会执行训练和评估过程数十次、数百次甚至数千次。但在现实世界中，探索每个可能有用的超参数值可能需要数月时间来执行，并且搜索空间通常会缩小到一组有限的可能有用的选项。因此，超参数调优是一个需要一定专业知识和经验的过程，这将帮助您在探索（尝试不同尚未探索的选项）和利用（将参数值缩小到通常似乎有效的那些）之间实现正确的权衡。

作为本章的一部分，我们学习了为什么超参数调优有用、何时使用它以及如何使用它。我们学习了如何在 Python 中以最纯粹的形式使用超参数调优库，以及如何通过 Flair 调优序列标注模型。

在下一章中，我们将介绍超参数调优证明有用的又一个 NLP 问题 – 训练嵌入。

## 7 训练自己的嵌入

Flair 序列标注器之所以能取得如此出色的性能，主要是归功于其秘密武器 – Flair 嵌入。它们的上下文设计、基于字符的事实以及在反向-前向配置中使用的[content]
```
tag_type = 'pos'
### 将语料库下采样到原始大小的 1%
corpus.downsample(0.01)
```

在前面的代码片段中，我们将英语语料库加载到 corpus 变量中，将标签类型定义为 PoS 标注，然后将语料库下采样到原始大小的 1%，以加快演示目的的训练过程。

然后我们可以使用 hp.choice 参数表达式定义搜索空间：

```
from flair.embeddings import WordEmbeddings
from flair.hyperparameter.param_selection import \
    SearchSpace, Parameter
from hyperopt import hp

search_space = SearchSpace()
search_space.add(Parameter.HIDDEN_SIZE,
                 hp.choice,
                 options=[128, 256])
search_space.add(Parameter.LEARNING_RATE,
                 hp.choice,
                 options=[0.05, 0.1, 0.15, 0.2])
search_space.add(Parameter.EMBEDDINGS,
                 hp.choice,
                 options=([WordEmbeddings('en')]))
```

前面的代码片段定义了我们的搜索空间，由三个超参数组成。我们决定为学习率和模型的隐藏层大小想出一些可能有用的值。尽管使用诸如 hyperopt.hp.quniform 和 hyperopt.hp.uniform 之类的参数表达式可能会产生更好的结果，但它们也会大大增加搜索空间的大小。我们的搜索空间现在非常小。只有八种可能的超参数组合（两个乘以四个，因为有两个隐藏层大小参数选项和四个学习率选项）。

> **重要说明**
> 在进行超参数优化时，Flair 中的某些超参数是强制性的。这包括 `Parameter.EMBEDDINGS` 和 `Parameter.LEARNING_RATE` 参数。如果您不想优化这些参数，只需为这些超参数的搜索空间传递一个选项即可。

我们现在准备定义我们的 SequenceTaggerParamSelector 对象：

```
from flair.hyperparameter.param_selection import \
    SequenceTaggerParamSelector

param_selector = SequenceTaggerParamSelector(corpus,
                                              tag_type,
                                              'paramopt')
```

这使用我们加载的语料库、我们定义的 PoS 标签类型以及 `paramopt` 文件夹名称（相对于运行代码的目录）定义了我们的超参数优化对象，超参数调优结果将写入该文件夹。

我们现在只需通过传入搜索空间和要执行的评估次数来启动优化过程。在我们的例子中，搜索空间是离散的，数字选项明确定义为 8。然而，在大多数情况下，使用非离散参数值时，可能的超参数组合数量是无限的，评估次数需要通过经验来选择：

```
param_selector.optimize(search_space, max_evals=8)
```

前面的代码将使用 8 种不同的超参数组合训练 8 个不同的序列标注器。它将使用 loss 作为 `objective` 函数并在开发数据集上进行评估。它将为每次运行训练 50 个 epoch。Flair 对 Hyperopt 的实现使用 TPE 优化器。在普通 CPU 上，执行时间大约在 10 到 30 分钟之间。

超参数优化完成后，结果将写入 `paramopt/param_selection.txt`。该文件包含每次评估运行的详细信息以及以下摘要：

```
best parameter combination
    embeddings: 0
hidden_size: 1
learning_rate: 3
```

前面的摘要表明，最佳隐藏层大小是第二个选项（因为摘要显示为 1，我们使用零索引，这意味着 1 指的是第二个选项）：256。最佳学习率是第四个选项 – 0.2。对于嵌入，当然，因为我们只有一个选项可供选择，因此第一个也是唯一一个选项被选为最佳。

这是否意味着我们应该始终将隐藏层大小用作 256，并将 0.2 作为英语的最佳学习率？简短的回答是否定的。最佳超参数值将取决于使用的数据、试图解决的问题以及用于解决该问题的技术。还记得前面我们加载语料库并将其从 100% 减少到原始大小的 1% 的代码片段吗？通过这样做，我们大大提高了模型训练的速度，这使我们能够更快地进行超参数优化，但与此同时，我们也将模型集中在非常小且可能有偏的数据子集上。要对英语数据集进行适当的超参数调优，我们需要在 100% 语料库大小下进行，并考虑更多超参数选项。这将大大增加我们的搜索空间和执行时间，但也会提高我们模型的性能。这些发现导致了以下结论。

## 摘要

超参数调优是一种非常有用的技术，可用于寻找最佳超参数，不仅在序列标注中，而且在机器学习的许多领域中。然而，它是有代价的；它非常昂贵。从本质上讲，它会执行训练和评估过程数十次、数百次甚至数千次。但在现实世界中，探索每个可能有用的超参数值可能需要数月时间来执行，并且搜索空间通常会缩小到一组有限的可能有用的选项。因此，超参数调优是一个需要一定专业知识和经验的过程，这将帮助您在探索（尝试不同尚未探索的选项）和利用（将参数值缩小到通常似乎有效的那些）之间实现正确的权衡。

作为本章的一部分，我们学习了为什么超参数调优有用、何时使用它以及如何使用它。我们学习了如何在 Python 中以最纯粹的形式使用超参数调优库，以及如何通过 Flair 调优序列标注模型。

在下一章中，我们将介绍超参数调优证明有用的又一个 NLP 问题 – 训练嵌入。

## 7 训练自己的嵌入

Flair 序列标注器之所以能取得如此出色的性能，主要是归功于其秘密武器 – Flair 嵌入。它们的上下文设计、基于字符的事实以及在反向-前向配置中使用的[content]
```
tag_type = 'pos'
### 将语料库下采样到原始大小的 1%
corpus.downsample(0.01)
```

在前面的代码片段中，我们将英语语料库加载到 corpus 变量中，将标签类型定义为 PoS 标注，然后将语料库下采样到原始大小的 1%，以加快演示目的的训练过程。

然后我们可以使用 hp.choice 参数表达式定义搜索空间：

```
from flair.embeddings import WordEmbeddings
from flair.hyperparameter.param_selection import \
    SearchSpace, Parameter
from hyperopt import hp

search_space = SearchSpace()
search_space.add(Parameter.HIDDEN_SIZE,
                 hp.choice,
                 options=[128, 256])
search_space.add(Parameter.LEARNING_RATE,
                 hp.choice,
                 options=[0.05, 0.1, 0.15, 0.2])
search_space.add(Parameter.EMBEDDINGS,
                 hp.choice,
                 options=([WordEmbeddings('en')]))
```

前面的代码片段定义了我们的搜索空间，由三个超参数组成。我们决定为学习率和模型的隐藏层大小想出一些可能有用的值。尽管使用诸如 hyperopt.hp.quniform 和 hyperopt.hp.uniform 之类的参数表达式可能会产生更好的结果，但它们也会大大增加搜索空间的大小。我们的搜索空间现在非常小。只有八种可能的超参数组合（两个乘以四个，因为有两个隐藏层大小参数选项和四个学习率选项）。

> **重要说明**
> 在进行超参数优化时，Flair 中的某些超参数是强制性的。这包括 `Parameter.EMBEDDINGS` 和 `Parameter.LEARNING_RATE` 参数。如果您不想优化这些参数，只需为这些超参数的搜索空间传递一个选项即可。

我们现在准备定义我们的 SequenceTaggerParamSelector 对象：

```
from flair.hyperparameter.param_selection import \
    SequenceTaggerParamSelector

param_selector = SequenceTaggerParamSelector(corpus,
                                              tag_type,
                                              'paramopt')
```

这使用我们加载的语料库、我们定义的 PoS 标签类型以及 `paramopt` 文件夹名称（相对于运行代码的目录）定义了我们的超参数优化对象，超参数调优结果将写入该文件夹。

我们现在只需通过传入搜索空间和要执行的评估次数来启动优化过程。在我们的例子中，搜索空间是离散的，数字选项明确定义为 8。然而，在大多数情况下，使用非离散参数值时，可能的超参数组合数量是无限的，评估次数需要通过经验来选择：

```
param_selector.optimize(search_space, max_evals=8)
```

前面的代码将使用 8 种不同的超参数组合训练 8 个不同的序列标注器。它将使用 loss 作为 `objective` 函数并在开发数据集上进行评估。它将为每次运行训练 50 个 epoch。Flair 对 Hyperopt 的实现使用 TPE 优化器。在普通 CPU 上，执行时间大约在 10 到 30 分钟之间。

超参数优化完成后，结果将写入 `paramopt/param_selection.txt`。该文件包含每次评估运行的详细信息以及以下摘要：

```
best parameter combination
    embeddings: 0
hidden_size: 1
learning_rate: 3
```

前面的摘要表明，最佳隐藏层大小是第二个选项（因为摘要显示为 1，我们使用零索引，这意味着 1 指的是第二个选项）：256。最佳学习率是第四个选项 – 0.2。对于嵌入，当然，因为我们只有一个选项可供选择，因此第一个也是唯一一个选项被选为最佳。

这是否意味着我们应该始终将隐藏层大小用作 256，并将 0.2 作为英语的最佳学习率？简短的回答是否定的。最佳超参数值将取决于使用的数据、试图解决的问题以及用于解决该问题的技术。还记得前面我们加载语料库并将其从 100% 减少到原始大小的 1% 的代码片段吗？通过这样做，我们大大提高了模型训练的速度，这使我们能够更快地进行超参数优化，但与此同时，我们也将模型集中在非常小且可能有偏的数据子集上。要对英语数据集进行适当的超参数调优，我们需要在 100% 语料库大小下进行，并考虑更多超参数选项。这将大大增加我们的搜索空间和执行时间，但也会提高我们模型的性能。这些发现导致了以下结论。

## 摘要

超参数调优是一种非常有用的技术，可用于寻找最佳超参数，不仅在序列标注中，而且在机器学习的许多领域中。然而，它是有代价的；它非常昂贵。从本质上讲，它会执行训练和评估过程数十次、数百次甚至数千次。但在现实世界中，探索每个可能有用的超参数值可能需要数月时间来执行，并且搜索空间通常会缩小到一组有限的可能有用的选项。因此，超参数调优是一个需要一定专业知识和经验的过程，这将帮助您在探索（尝试不同尚未探索的选项）和利用（将参数值缩小到通常似乎有效的那些）之间实现正确的权衡。

作为本章的一部分，我们学习了为什么超参数调优有用、何时使用它以及如何使用它。我们学习了如何在 Python 中以最纯粹的形式使用超参数调优库，以及如何通过 Flair 调优序列标注模型。

在下一章中，我们将介绍超参数调优证明有用的又一个 NLP 问题 – 训练嵌入。

## 7 训练自己的嵌入

Flair 序列标注器之所以能取得如此出色的性能，主要是归功于其秘密武器 – Flair 嵌入。它们的上下文设计、基于字符的事实以及在反向-前向配置中使用的[content]
```
tag_type = 'pos'
### 将语料库下采样到原始大小的 1%
corpus.downsample(0.01)
```

在前面的代码片段中，我们将英语语料库加载到 corpus 变量中，将标签类型定义为 PoS 标注，然后将语料库下采样到原始大小的 1%，以加快演示目的的训练过程。

然后我们可以使用 hp.choice 参数表达式定义搜索空间：

```
from flair.embeddings import WordEmbeddings
from flair.hyperparameter.param_selection import \
    SearchSpace, Parameter
from hyperopt import hp

search_space = SearchSpace()
search_space.add(Parameter.HIDDEN_SIZE,
                 hp.choice,
                 options=[128, 256])
search_space.add(Parameter.LEARNING_RATE,
                 hp.choice,
                 options=[0.05, 0.1, 0.15, 0.2])
search_space.add(Parameter.EMBEDDINGS,
                 hp.choice,
                 options=([WordEmbeddings('en')]))
```

前面的代码片段定义了我们的搜索空间，由三个超参数组成。我们决定为学习率和模型的隐藏层大小想出一些可能有用的值。尽管使用诸如 hyperopt.hp.quniform 和 hyperopt.hp.uniform 之类的参数表达式可能会产生更好的结果，但它们也会大大增加搜索空间的大小。我们的搜索空间现在非常小。只有八种可能的超参数组合（两个乘以四个，因为有两个隐藏层大小参数选项和四个学习率选项）。

> **重要说明**
> 在进行超参数优化时，Flair 中的某些超参数是强制性的。这包括 `Parameter.EMBEDDINGS` 和 `Parameter.LEARNING_RATE` 参数。如果您不想优化这些参数，只需为这些超参数的搜索空间传递一个选项即可。

我们现在准备定义我们的 SequenceTaggerParamSelector 对象：

```
from flair.hyperparameter.param_selection import \
    SequenceTaggerParamSelector

param_selector = SequenceTaggerParamSelector(corpus,
                                              tag_type,
                                              'paramopt')
```

这使用我们加载的语料库、我们定义的 PoS 标签类型以及 `paramopt` 文件夹名称（相对于运行代码的目录）定义了我们的超参数优化对象，超参数调优结果将写入该文件夹。

我们现在只需通过传入搜索空间和要执行的评估次数来启动优化过程。在我们的例子中，搜索空间是离散的，数字选项明确定义为 8。然而，在大多数情况下，使用非离散参数值时，可能的超参数组合数量是无限的，评估次数需要通过经验来选择：

```
param_selector.optimize(search_space, max_evals=8)
```

前面的代码将使用 8 种不同的超参数组合训练 8 个不同的序列标注器。它将使用 loss 作为 `objective` 函数并在开发数据集上进行评估。它将为每次运行训练 50 个 epoch。Flair 对 Hyperopt 的实现使用 TPE 优化器。在普通 CPU 上，执行时间大约在 10 到 30 分钟之间。

超参数优化完成后，结果将写入 `paramopt/param_selection.txt`。该文件包含每次评估运行的详细信息以及以下摘要：

```
best parameter combination
    embeddings: 0
hidden_size: 1
learning_rate: 3
```

前面的摘要表明，最佳隐藏层大小是第二个选项（因为摘要显示为 1，我们使用零索引，这意味着 1 指的是第二个选项）：256。最佳学习率是第四个选项 – 0.2。对于嵌入，当然，因为我们只有一个选项可供选择，因此第一个也是唯一一个选项被选为最佳。

这是否意味着我们应该始终将隐藏层大小用作 256，并将 0.2 作为英语的最佳学习率？简短的回答是否定的。最佳超参数值将取决于使用的数据、试图解决的问题以及用于解决该问题的技术。还记得前面我们加载语料库并将其从 100% 减少到原始大小的 1% 的代码片段吗？通过这样做，我们大大提高了模型训练的速度，这使我们能够更快地进行超参数优化，但与此同时，我们也将模型集中在非常小且可能有偏的数据子集上。要对英语数据集进行适当的超参数调优，我们需要在 100% 语料库大小下进行，并考虑更多超参数选项。这将大大增加我们的搜索空间和执行时间，但也会提高我们模型的性能。这些发现导致了以下结论。

## 摘要

超参数调优是一种非常有用的技术，可用于寻找最佳超参数，不仅在序列标注中，而且在机器学习的许多领域中。然而，它是有代价的；它非常昂贵。从本质上讲，它会执行训练和评估过程数十次、数百次甚至数千次。但在现实世界中，探索每个可能有用的超参数值可能需要数月时间来执行，并且搜索空间通常会缩小到一组有限的可能有用的选项。因此，超参数调优是一个需要一定专业知识和经验的过程，这将帮助您在探索（尝试不同尚未探索的选项）和利用（将参数值缩小到通常似乎有效的那些）之间实现正确的权衡。

作为本章的一部分，我们学习了为什么超参数调优有用、何时使用它以及如何使用它。我们学习了如何在 Python 中以最纯粹的形式使用超参数调优库，以及如何通过 Flair 调优序列标注模型。

在下一章中，我们将介绍超参数调优证明有用的又一个 NLP 问题 – 训练嵌入。

## 7 训练自己的嵌入

Flair 序列标注器之所以能取得如此出色的性能，主要是归功于其秘密武器 – Flair 嵌入。它们的上下文设计、基于字符的事实以及在反向-前向配置中使用的[content]
```
tag_type = 'pos'
### 将语料库下采样到原始大小的 1%
corpus.downsample(0.01)
```

在前面的代码片段中，我们将英语语料库加载到 corpus 变量，将标签类型定义为 PoS 标注，然后将语料库下采样到原始大小的 1%，以加快演示训练。

然后我们可以使用 hp.choice 参数表达式定义搜索空间：

```
from flair.embeddings import WordEmbeddings
from flair.hyperparameter.param_selection import \
    SearchSpace, Parameter
from hyperopt import hp

search_space = SearchSpace()
search_space.add(Parameter.HIDDEN_SIZE,
                 hp.choice,
                 options=[128, 256])
search_space.add(Parameter.LEARNING_RATE,
                 hp.choice,
                 options=[0.05, 0.1, 0.15, 0.2])
search_space.add(Parameter.EMBEDDINGS,
                 hp.choice,
                 options=([WordEmbeddings('en')]))
```

前面的代码片段定义了搜索空间，包含三个超参数。我们为学习率和模型隐藏层大小选择了一些可能有用的值。虽然使用 hyperopt.hp.quniform 和 hyperopt.hp.uniform 可能效果更好，但会大幅增加搜索空间。当前搜索空间很小，只有八种组合，即两个隐藏层选项乘以四个学习率选项。

> **重要说明**
> 进行超参数优化时，Flair 中的某些超参数是强制性的，包括 `Parameter.EMBEDDINGS` 和 `Parameter.LEARNING_RATE` 参数。若不想优化这些参数，只需为这些超参数的搜索空间传递单个选项即可。

我们现在准备定义 SequenceTaggerParamSelector 对象：

```
from flair.hyperparameter.param_selection import \
    SequenceTaggerParamSelector

param_selector = SequenceTaggerParamSelector(corpus,
                                              tag_type,
                                              'paramopt')
```

这使用加载的语料库、定义的 PoS 标签类型以及 `paramopt` 文件夹名称定义了超参数优化对象，调优结果将写入该文件夹。

我们只需传入搜索空间和评估次数即可开始优化。在本例中搜索空间离散，次数明确为 8。但在大多数非离散参数情况下，可能的组合数无限，评估次数需经验选择：

```
param_selector.optimize(search_space, max_evals=8)
```

前面的代码将用 8 种超参数组合训练 8 个序列标注器。以 loss 作为 `objective` 函数在开发集上评估，每个运行训练 50 个 epoch。Flair 的 Hyperopt 实现使用 TPE 优化器。在普通 CPU 上执行约需 10 到 30 分钟。

优化完成后，结果写入 `paramopt/param_selection.txt`。文件包含每次评估的细节及以下摘要：

```
best parameter combination
    embeddings: 0
hidden_size: 1
learning_rate: 3
```

摘要表明最佳隐藏层大小为第二个选项，即 256；最佳学习率为第四个选项 0.2。嵌入因只有一个选项而被选中。

这是否意味着英语中应始终使用隐藏层大小 256 和学习率 0.2？简短回答是否。最优超参数取决于数据、问题和技术。前面我们将语料库从 100% 降到 1%，虽加快训练和优化，但也使模型聚焦于很小且可能有偏的子集。对英语数据集进行恰当调优需在 100% 语料库上进行并考虑更多选项，这会大幅增加搜索空间和时间，但也能提升性能。由此得出以下结论。

## 摘要

超参数调优是寻找最优超参数的有用技术，不仅适用于序列标注，也适用于机器学习的许多领域。但代价高昂，本质上需重复训练和评估数十、数百甚至数千次。现实中探索所有可能值可能需数月，搜索空间通常被限制为有限的有用选项。因此超参数调优需要一定专业知识和经验，以在探索和利用之间取得正确平衡。

本章我们学习了超参数调优为何有用、何时使用、如何使用，以及如何在 Python 中以纯形式使用调优库并通过 Flair 调优序列标注模型。

下一章将介绍超参数调优同样有用的另一个 NLP 问题 – 训练嵌入。

## 7 训练自己的嵌入

Flair 序列标注器出色性能主要归功于其秘密武器 – Flair 嵌入。其上下文设计、基于字符以及反向-前向配置的使用，使其非常适合序列标注任务。但到目前为止，本书尚未重点介绍这些嵌入的实际训练方法。

上一章介绍模型训练和词嵌入时，我们仅使用了 Flair Python 包提供的预训练嵌入。但在许多 NLP 问题中，预训练嵌入可能不足。

使用 flair 时，您可能需要处理 Flair 预训练嵌入尚未覆盖的语言，或需要针对特定语料库训练的嵌入。此时可使用 Flair 的内置类训练自己的 Flair 嵌入，后续用于训练序列标注器或解决其他下游 NLP 任务。

我们将先解释为何以及在何种情况下训练自定义嵌入有用。介绍 Flair 嵌入的工作原理及其在下游序列标注任务上的出色表现。然后聚焦词嵌入的评估方法。最后通过在世界上最小语言上的动手练习训练自己的嵌入。

本章将涵盖训练 Flair 词嵌入的以下部分：

- 理解自定义 Flair 嵌入的原理和原因
- 评估词嵌入
- 在世界上最小的语言上训练 Flair 嵌入

## 技术要求

本章练习需要 Flair 版本 0.11 以及 `requests` Python 包已安装在开发机器上。

本章代码示例可在本书官方 GitHub 仓库的以下 Jupyter 笔记本中找到：`https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/tree/main/Chapter07.`

## 理解自定义 Flair 嵌入的原理和原因

词嵌入在序列标注模型性能中起着重要甚至必不可少的作用。嵌入的细节在 *第 3 章，Flair 中的嵌入* 中介绍。但本章需了解嵌入本质上是词表示，通常以实值向量形式存在。这些向量可作为下游任务的输入，如 **词性** (**PoS**) 标注和 **命名实体识别** (**NER**)。首先简要介绍 Flair 如何生成嵌入及训练方式。

## 为什么训练嵌入很少真正意味着训练嵌入

术语 **训练嵌入** 常令人困惑，可追溯到旧方法，当时训练嵌入的结果是一组词嵌入。在 Flair 的训练方式下该术语意义不大，尤其嵌入是上下文且基于字符。对于传统嵌入如 **GloVe**，训练后可为字典中每个词记住一个嵌入。此时不完全理解 Flair 嵌入设计的人可能会问：

*鉴于 Flair 的嵌入是上下文的，这是否意味着训练嵌入的过程会导致为字典中每个词在每个可能上下文中预先计算一个嵌入？*



答案是否定的。 对于基于词的、非上下文词嵌入，可能的嵌入数量确实受限于词典中的词数。 但是，由于 Flair 嵌入可以对任何给定上下文中的几乎任何字符序列（不一定是已知词）进行建模，因此可能的嵌入数量是难以想象的庞大。 这是由于以下原因：

- Flair 嵌入是上下文相关的，这意味着同一个词放在不同的上下文中会产生不同的嵌入。
- Flair 嵌入是在没有明确的词概念的情况下训练的。 词被建模为字符序列。

当我们谈论训练 Flair 嵌入时，我们实际上并不是在谈论预先计算嵌入本身。 相反，我们是在谈论训练语言模型。 让我们进一步了解这一点。

## Flair 嵌入背后的思想

训练 Flair 嵌入实际上是一个训练语言模型的过程。 Flair 嵌入模型本质上是语言模型，它们根据某些上下文（ surrounding text）预测下一个字符。 Flair 使用基于长短期记忆（LSTM）循环神经网络节点的语言模型。 这些节点能够处理序列数据。

> **什么是语言模型？**
> 简单来说，语言模型是一种对文本字符串的概率分布进行建模的工具。 它通常根据某些上下文（前文）为每个可能的字符、词或词序列分配一个概率。 直到深度学习的出现，流行的语言模型类型是统计 n-gram 模型； 但是，更近期的基于神经网络的方法 consistently outperform n-gram 模型。 经典 n-gram 模型仍然有一些优势。 与大多数神经模型不同，n-gram 模型不需要太多数据，并且在大小方面可以非常轻量。

基于字符的语言模型将输入文本作为字符序列接收，并被训练以预测下一个字符。 然后模型被训练以学习根据过去的字符预测可能下一个字符的概率分布。

*那么，预测下一个字符与词嵌入有什么关系？*

训练好的 Flair 语言模型本质上是生成词嵌入的模型。 首先，我们将整个上下文（围绕我们词的文本），包括我们想要计算嵌入的词，作为输入传递给模型。 然后，我们不关注预测的字符，而是关注 LSTM 单元的一个特殊值，称为隐藏状态。 我们提取词中最后一个字符之后的隐藏状态，这就是我们的词嵌入。

很明显，上下文会影响某些词的语义。 但是，你可能会想，是文本在词 *之前* 影响词的含义，还是词 *之后* 的文本影响其含义。 答案是两者兼有。

## 前向-后向嵌入堆栈

Flair 嵌入中一个精妙的设计概念是它们使用后向-前向嵌入堆栈的方式，这有助于它们实现出色的性能。

**前向模型** 考虑词之前的整个上下文以及词本身。 它捕捉了受词前文本影响的词的语义含义。 但是，该模型实际上并没有考虑到我们试图计算嵌入的词之后的全部词。 为了实现这一点，我们需要构建一个反向工作的模型。 我们需要构建一个 **后向模型**。

后向模型以相反的方向处理词。 对于英语而言，这意味着从右到左。 它考虑词之后的全部上下文加上词本身。 该模型捕捉了受词后文本影响的词的语义含义。

一旦两个模型都训练完成，我们就可以计算前向嵌入和后向嵌入。 然后我们通常将它们结合起来形成一个新的嵌入，我们称之为 **嵌入堆栈**。

## Flair 嵌入的语料库

训练任何类型的机器学习模型都需要数据。 幸运的是，训练嵌入是一个无监督学习任务。 用于训练的数据是未标记的。 这意味着任何包含有意义构造词的文本都可能被用作我们语料库的一部分。 重要的是我们使用足够大的语料库。

还有版权问题。 目前仍有一个持续的法律问题，即语料库的版权相关权利是否也适用于语言模型本身，以及在什么条件下适用。 例如，如果分发原始语料库需要署名，那么对在同一语料库上训练的语言模型是否也适用相同的署名要求？ 这个问题的答案取决于许多因素，但目前没有一种明确的答案方式适用于所有类型的场景。 但是，仍然有一些通用规则适用。 如果你在家用个人电脑学习 NLP，如果你在任何公开可用的文本上训练模型，你不太可能惹上麻烦。 然而，这些实际豁免在商业环境中并不适用。 随着你向大型科技公司靠拢，版权法的执行会变得越来越严格。

## 训练 Flair 嵌入的词典

Flair 嵌入训练最好的事情之一是为语言模型准备词典所需的努力，或缺乏努力。 对于大多数传统的基于词的语言模型，你需要为给定语言制定一个完整的词列表。

幸运的是，Flair 嵌入是基于字符的。 这意味着语言模型的输入不是词，而是字符序列。 我们无需担心制定完整的词列表。 相反，我们只需要列出我们语言中使用的所有可能字符。 这意味着如果我们处理的是使用拉丁字母的语言，我们的词典只会包含少数几项。 Flair 还提供了带有预定义常用词典的类，例如 Latin。

Flair 的预训练嵌入在许多语言的许多下游 NLP 任务上取得了最先进的结果。 但是，在训练新的嵌入模型时，性能将取决于许多因素，并非我们训练的每个嵌入模型都会表现良好。 我们需要找到方法来判断我们新训练的嵌入是否达到标准。

## 评估词嵌入

在上一节中，我们介绍了使用语言模型的 Flair 嵌入的设计。 训练这些语言模型的过程与任何其他类型的深度学习训练并没有太大不同。 但是，性能良好的语言模型并不一定意味着能产生下游任务优秀结果的好嵌入。

相反，我们通常需要依赖以下两种评估词嵌入的方法：

- **内在评估** 旨在测试嵌入词表示的质量，而不依赖于任何自然语言处理任务。 这是通过衡量词之间的语义关系来完成的。 最简单的内在嵌入评估类型是词相似度。 它 simply 使用相似度指标，例如余弦距离，来衡量词嵌入之间的相似度，并将其与人类感知的语义相似度进行比较。 例如，词 *开始* 和 *起始* 在语义上非常相似。 如果我们的嵌入训练良好，那么我们期望这两个词在余弦距离上彼此接近。

其他类型的内在评估方法利用词之间的类比关系，类似于我们在 *国王对男人如同女王对女人* 类比练习中学习词嵌入的方式，该练习在 *第 3 章，Flair 中的嵌入* 中。

- **外在评估** 旨在通过将词嵌入用于下游 NLP 任务（例如 PoS 标注、NER 或分块）来测试词嵌入的质量。 我们将嵌入作为输入提供给 NLP 模型。 然后我们评估 NLP 模型的质量，并假设其性能的改善/退化可以归因于我们嵌入的性能。 Flair 拥有训练序列标注模型的所有工具，并为常用语言提供标记数据集。 如果你发现自己正在训练 Flair 嵌入，很可能你计划将它们用作下游序列标注 NLP 任务的输入。 这通常使外在评估成为一个直接的选择。

让我们现在进入本章的下一部分，在那里我们训练自己的嵌入，并稍后使用此处描述的技术之一进行评估。

## 在世界上最小的语言上训练 Flair 嵌入

Flair 包为训练 Flair 嵌入提供了一个简单、直接的 API。 该过程涉及训练任何语言模型所需的所有必要步骤。 步骤包括以下内容：

- 准备词典
- 准备语料库
- 定义语言模型
- 训练语言模型

让我们通过一个实践动手练习来介绍训练 Flair 嵌入的过程。

对大多数语言进行嵌入训练并不是一个快速的过程。 一台 decent 的配备 GPU 的机器需要超过一周的训练时间才能产生与英语或类似语言上已发表的最先进 Flair 结果相当的结果。 大多数预训练的 Flair 嵌入，例如 en 英语嵌入模型，产生长度为 2,048 的嵌入。

我们需要 2,048 维向量部分原因是像英语这样的语言在词典中有大量词，并且它们的语义会根据使用的上下文而变化。 但是，如果我们在本次练习中，在一种非常小的语言上训练嵌入会怎样？ 如果我们在一种如此小的语言上训练嵌入，以至于我们甚至可以在没有专用图形处理单元（GPU）的机器上完成它会怎样？



作为本次练习的一部分，我们将为世界上最小的语言——**托基·波纳**训练 Flair 上下文词嵌入。

托基·波纳是一种人造（自创）语言，以其极小的词汇量而闻名。该语言的原始版本仅有约 120 个词。该语言围绕极简主义的概念创建，旨在简化思想和交流。它只有九个辅音和五个元音，且托基·波纳词的发音与世界上大多数语言兼容。

现在让我们为托基·波纳训练一个微小的 Flair 嵌入模型。

## 准备词典

在训练经典词嵌入时，为一种语言定义完整词典的过程往往繁琐且耗费精力。对于 Flair 嵌入来说并非如此。

这是因为 Flair 嵌入建模的是字符，而不是词。这意味着为了定义词典，我们不需要所有可能词的完整列表。我们只需要所有可能字符的列表即可。

在托基·波纳语言中，仅使用了以下 14 个字符：`ptksmnljwaeiou`。这些字符是拉丁字符集的子集。Flair 预编译的拉丁词典只需运行以下代码即可加载：

```
from flair.data import Dictionary

dictionary = Dictionary.load('chars')
```

但是，需要知道的是，该字符集包含许多在托基·波纳中从未使用的字符。这意味着语言模型需要更长时间的训练才能学会某些在词典中定义的字符是无意义的。

相反，如果我们想保持词典最小化，仅包含我们语言中使用的字母，我们可以通过使用 Dictionary.add_item(c) 方法手动构建自己的 Dictionary 类：

```
from flair.data import Dictionary

dictionary = Dictionary()
toki_pona_symbols = 'ptksmnljwaeiou'
toki_pona_symbols += toki_pona_symbols.upper()

for c in toki_pona_symbols + '?. ':
    dictionary.add_item(c)
```

在之前的代码片段中，我们定义了一个词典，其中包含托基·波纳中使用的所有可能字符的大写和小写形式，以及空格和两个标点符号。Dictionary 类还会自动添加一个特殊的 <unk> 符号，用于标记未知字符。

## 准备语料库

如果没有数据，我们的嵌入模型就无法学习任何东西。训练嵌入模型的一般规则是数据越多越好。实际上，这意味着一个好的嵌入模型需要超过一百万句句子。但就本次练习而言，并且由于我们处理的是大约只有 120 个词的语言，我们可以选择一个小得多的语料库。我们将选择一个公开可用的托基·波纳 1,000 句语料集合，可通过以下地址获取：https://raw.githubusercontent.com/tadejmagajna/TokiponaCorpus/master/bot4/corpus.txt。

然后我们将把语料库拆分为训练集、测试集和验证集，并将语料库目录传递给 TextCorpus 类的构造函数。

让我们先下载语料库并将其拆分为测试集、训练集和验证集：

```python
import requests

url = "https://raw.githubusercontent.com/tadejmagajna/TokiponaCorpus/master/bot4/corpus.txt"
response = requests.get(url)

sentences = response.text.splitlines()
one_tenth_corp_len = int(len(sentences)/10)

test, valid, train = (
    sentences[:one_tenth_corp_len],
    sentences[one_tenth_corp_len:one_tenth_corp_len*2],
    sentences[one_tenth_corp_len*2:])
```

前面的代码片段首先从 GitHub 下载整个语料库。然后按换行符拆分句子，并将语料库的前 10% 载入 test，将另外 10% 载入 valid，剩余的 80% 载入 train。

另外值得一提的是，在实践中，数据集拆分几乎总是使用辅助 NLP 框架函数来完成。这些辅助函数可以确保数据集被正确打乱，以便数据在各集合之间得到良好分布。但在这个代码示例中，我们可以假设数据已经分布良好，这意味着我们可以简单地将句子列表切片为三个部分。

> **重要说明**
> 运行前面的代码片段，您需要安装本地 Python 3 以及 Flair 版本 0.11 和 requests 包。requests 包可通过运行 `pip3 install requests` 安装。

现在语料库已存储在内存中（如果处理的是包含数百万句子的语料库则不可能）。在初始化 `TextCorpus` 类之前，我们需要将语料库集合存储在本地目录中，该目录按以下文件夹和文件组织语料库：

- 包含验证集的 `valid.txt` 文件
- 包含测试集的 `test.txt` 文件
- 包含训练集的 `train/` 文件夹，训练集被拆分为编号文件，命名模式如下：`train_split_1`、`train_split_2`、`train_split_3`，以此类推

我们通过运行以下代码将语料库组织成前面定义的结构：

```python
from tempfile import TemporaryDirectory
from os.path import join
from os import mkdir

dataset_dir_obj = TemporaryDirectory()
dataset_dir = dataset_dir_obj.name
train_dir = join(dataset_dir, 'train')
mkdir(train_dir)

with open(join(dataset_dir, "test.txt"), "w") as file:
    file.write(' '.join(test))

with open(join(dataset_dir, "valid.txt"), "w") as file:
    file.write(' '.join(valid))

with open(join(train_dir, "train_split_1"), "w") as file:
    file.write(' '.join(train))
```

在前述代码片段的第一部分中，我们使用 tempfile 库创建一个临时目录，在其中存储我们的语料库。在 Python 中使用临时目录是个好主意，因为该库提供了在完成后轻松清理的接口。

在前述代码片段的第二部分中，我们随后将 test、valid 和 train 语料库集合分别写入 train.txt、test.txt 和 train_split_1 文件。整个语料库现在存储在 dataset_dir 目录中。

现在语料库已正确组织，我们可以初始化 TextCorpus 类：

```
from flair.trainers.language_model_trainer import TextCorpus

corpus = TextCorpus(dataset_dir,
                    dictionary,
                    forward=True,
                    character_level=True)
```

前面的代码片段初始化 TextCorpus 类，我们提供数据集目录和词典，并明确告诉构造函数我们正在为前向模型准备语料库（该模型会考虑词及其之前的上下文）。

## 训练语言模型

现在我们可以定义并训练我们的语言模型：

```
from flair.models import LanguageModel
from flair.trainers.language_model_trainer import (
    LanguageModelTrainer)

language_model = LanguageModel(dictionary,
                               is_forward_lm=True,
                               hidden_size=64,
                               nlayers=1)

trainer = LanguageModelTrainer(language_model, corpus)
trainer.train('forward_model_directory',
    sequence_length=25,
    mini_batch_size=10,
    max_epochs=100)
```

在前述代码片段的第一部分中，我们通过告知它我们正在构建一个具有单层和隐藏大小为 64 的前向模型来初始化 LanguageModel 类。hidden_size 是一个重要参数，定义了 LSTM 节点隐藏层的大小，这实际上定义了我们的嵌入向量的维度。hidden_size 通常需要设置得高得多，例如 2,048。但由于我们是在大约只有 120 个词的语言上训练模型，并且这只是练习的一部分，因此可以使用低维嵌入。

在前一个片段的第二部分中，我们通过初始化 LanguageModelTrainer 类并告知其使用序列长度 25 并运行 100 个 epoch 来开始模型训练过程。这里选择的参数 purposely 远低于在生产环境中训练大型嵌入模型时所使用的参数。实际上，使用比这更长的序列长度并进行更多 epoch 是个好主意。

然后我们通过调用 LanguageModelTrainer.train() 开始训练，其中指定了训练后模型的输出目录，在我们的例子中是 forward_model_directory/。该目录将相对于我们运行此代码的目录创建。

如果一切做对，模型训练应该开始，Flair 应该开始向标准输出打印一些调试文本和指标值：

```
ms/batch  5.88 | loss  0.77 | ppl    2.15
Epoch    97: reducing learning rate of group 0 to 1.2207e-03.
best loss so far  0.98
('\n ni li sike. mi pini e ma mije? kala ni li sin e pan lawa
en kena t
---------------------------------------------------------------
end of split 1/1 | epoch  97 | time:  1.75s | valid loss 0.99
---------------------------------------------------------------
Epoch time: 1.92
read text file with 1 lines
```



前面的输出提供了有价值的信息。它告诉我们当前正在处理的 epoch（本例中为 epoch 97），训练损失是多少（本例中为 0.77），验证损失是多少（本例中为 0.99），并且打印出了由我们的语言模型生成的一些文本：ni li sike. mi pini e ma mije? kala ni li sin e pan lawa en kena t.

在最初几个 epoch 中，你会注意到我们的语言模型产生的是一堆胡言乱语。大多数词都是词汇表外的词，输出完全随机。但随着训练的进行，你会注意到损失在下降，生成的文本开始看起来像 Toki Pona.

在普通机器上，用 CPU 训练这 100 个 epoch 不应超过 10 分钟。训练完成后，Flair 会打印出最终的验证损失和困惑度：

```
TEST: valid loss 0.92 | valid ppl 2.52
```

就这样！

我们的 Flair 嵌入模型现在已经训练完成。我们可以开始生成一些嵌入。但在这样做之前，我们可以先稍微玩一下语言模型，并使用 generate_text() 方法生成一些文本：

```
t = language_model.generate_text(number_of_characters=40)[0]
print(t)
```

这将打印出语言模型生成的 40 个字符（由于权重随机初始化，每次训练的结果都会不同）：

```
sike ni li pona mute. soweli ni li pana
```

虽然这些 Toki Pona 语句对人类来说意义不大，但至少我们可以欣慰的是，生成的词都是真正的 Toki Pona 词，词的结构似乎遵循某种逻辑模式。还请注意，我们只生成了 40 个字符，这意味着最后一个词可能会被截断。如果你自己训练模型，你的随机初始化模型权重会不同，模型产生的文本几乎肯定会与所示示例不同。

## 使用自定义嵌入进行下游任务

训练 Flair 嵌入模型的主要目的并不是玩弄生成的文本和打印嵌入向量。很可能，我们训练嵌入是为了在下游 NLP 任务中使用它们。

幸运的是，加载自定义训练的 Flair 嵌入的语法与加载任何其他预训练 Flair 嵌入模型相同。我们只需将模型路径传递给构造函数即可。

如果传递给 LanguageModelTrainer 类的输出路径是 forward_model_directory/，我们将在 forward_model_directory/best-lm.pt 找到训练好的模型。

然后我们可以通过运行以下代码实例化一个 FlairEmbeddings 对象：

```
from flair.embeddings import FlairEmbeddings
fw = FlairEmbeddings('forward_model_directory/best-lm.pt')
```

前面的代码片段将我们的自定义嵌入模型加载到 fw 变量中。

请注意，在本练习中，我们只训练了前向嵌入模型，它考虑的是词之前的上下文。为了获得更好的性能，你现在可能希望使用 forward 和 is_forward_lm 参数设为 False 重复整个嵌入训练过程，这将改为训练反向模型。然后你可以将嵌入模型堆叠在一起形成一个 StackedEmbeddings 对象，这是第 5 章 Training Sequence Labeling Models 中深入介绍的过程的一部分。

你现在可以在任何使用词嵌入的 Flair 下游 NLP 任务中使用此模型。这样做还可以让你通过评估使用这些自定义嵌入的标注器的性能来对嵌入模型进行外部评估。

但训练标注器需要很长时间。因此，在这样做之前，你可能只想做一个合理性检查，以验证我们的嵌入是否至少包含一些有用的信息。

## 对自定义 Flair 嵌入进行内在评估

在我们进行冗长的序列标注模型训练过程以验证我们的嵌入是否有意义之前，我们可以改做一个非常简单的内在评估作为合理性检查。

我们可以做的一个简单的内在检查是为几个同义词计算嵌入，然后使用相似度指标判断 resulting 嵌入是否相似。同样，我们可以为语义上不相似的词计算嵌入并检查它们的嵌入是否也不相似。为了简化操作，我们将对不相似的词使用一个词汇表外（OOV）词。因此，我们的测试将验证两个相似词的嵌入是否比一个词典词和一个 OOV 词的嵌入更接近（以余弦距离衡量）。

对于相似词，我们将选择 lukin 和 oko。虽然不是真正的同义词，但在 Toki Pona 中它们的含义都与视觉和眼睛有关。我们将为这两个词和另一个 OOV 词生成嵌入，然后测量这些嵌入之间的相似度。如果我们的嵌入质量不错，lukin 和 oko 在相似度指标上应该得分很高，而 OOV 词则不期望与前两者相似。

让我们使用我们的自定义嵌入模型为这三个词全部计算嵌入：

```
from flair.embeddings import FlairEmbeddings
from flair.data import Sentence

synonym_1 = Sentence('lukin')
synonym_2 = Sentence('oko')
rand_word = Sentence('oovword')

fw = FlairEmbeddings('forward_model_directory/best-lm.pt')
fw.embed(synonym_1)
fw.embed(synonym_2)
fw.embed(rand_word)

embedding_syn_1 = synonym_1[0].embedding.tolist()
embedding_syn_2 = synonym_2[0].embedding.tolist()
embedding_rnd_wrd = rand_word[0].embedding.tolist()
```

在前述代码片段的第一部分，我们只是为词 lukin、oko 和 oovword（随机 OOV 词）初始化 Sentence 类。

然后我们可以使用余弦相似度指标来衡量这三个嵌入之间的相似度：

```
from sklearn.metrics.pairwise import cosine_similarity as sim

s_synonym = sim([embedding_syn_1], [embedding_syn_2])[0][0]
s_rand_1 = sim([embedding_syn_1], [embedding_rnd_wrd])[0][0]
s_rand_2 = sim([embedding_syn_2], [embedding_rnd_wrd])[0][0]
```

前面的片段计算了三个词对之间的相似度，并将 lukin 和 oko（同义词）之间的相似度存储到 s_synonym 中。它还计算了 lukin 与 oovword 之间，以及 oko 与 oovword 之间的相似度。

让我们看看我们的假设是否正确：

```
print(s_synonym , s_rand_1 , s_rand_2)
```

如果你的嵌入训练得不错，前面的代码片段将打印出类似以下内容：

```
0.6111595281781783 0.2691232575537895 0.18826418665007352
```

太好了！

事实证明我们的假设得到了支持。相似词在相似度得分上得分很高（打印出的第一个数字），而不相关的词（打印出的第二个和第三个数字）得分较低。请注意，我们从这些结果中唯一可以得出的结论是我们的嵌入中存储了一些程度的 Toki Pona 信息，但我们不应就这些嵌入在下游 NLP 任务中的有用性做出任何结论。要正确训练 Flair 嵌入以供实际使用，我们需要在更大的数据集上训练，使用更高的嵌入维度，训练更多 epoch，并使用更长的序列长度。

## 总结

训练自定义嵌入是 Flair 中较为复杂的功能之一。它需要所有有助于我们选择正确参数并正确准备数据的知识和理解，以便利用训练较大语言嵌入所需的大量计算能力。它也是一个关键概念，必须正确理解和执行，因为 Flair 的几乎所有其他功能都以某种方式依赖于嵌入。

在本章中，我们介绍了训练自定义 Flair 词嵌入的动机，并概述了嵌入设计。我们通过为世界上最小的语言 – Toki Pona 训练前向词嵌入，介绍了训练这些嵌入所需的语法。

到目前为止，我们将嵌入呈现为可作为下游 NLP 序列标注任务输入的东西。但嵌入也可以用于其他 NLP 应用，例如文本分类。让我们在下一章学习关于那方面的内容。


