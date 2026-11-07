
```
tag_type = 'pos'
# downsample the corpus to 1% of original size
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
# downsample the corpus to 1% of original size
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
# downsample the corpus to 1% of original size
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
# downsample the corpus to 1% of original size
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
# downsample the corpus to 1% of original size
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
# downsample the corpus to 1% of original size
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
