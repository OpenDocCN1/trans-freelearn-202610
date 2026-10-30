

$$F(t)=\int_{t}^{\infty} f(s\mid D=0)\,ds$$ $$G(t)=\int_{t}^{\infty} f(s\mid D=1)\,ds$$

因为 $G(t)$ 中的所有患者按定义都是金标准阳性，$G(t)$ 描述的是所有阳性患者中 SUV 超过 $t$ 的阳性患者所占比例。这不过是当 $t$ 用作阈值时的灵敏度。类似地，$1-F(t)$ 就是特异性；因此，$F(t)$ 表示一减去特异性。因此，ROC 曲线是 $F(t)$ 与 $G(t)$ 随每个 $t$ 的作图。

尽管在数学上是正确的，但这并不能完全描述 ROC 曲线，因为它依赖于 $t$。然而，我们知道 $F(t)$ 是 ROC 曲线的 x 坐标。令 $x=F(t)$ 并求解 $t$，得到 $t=F^{-1}(x)$。对应于 $t$ 的灵敏度是 $G(t)$。因此，对应于 $x$ 的灵敏度由下式给出

$$y=G(F^{-1}(x))$$

这种表示有几个重要特征。首先，它是通用的。对标记物或金标准没有任何假设。它还明确了 ROC 曲线对整个分布函数的依赖性。这种表示也解释了为什么 ROC 曲线关注灵敏度和特异性而不是 NPV 和 PPV。阳性预测值和阴性预测值的分母会随阈值变化，并且不适合用预测变量的累积分布函数进行记号和分析。

这种关系也可以用来突出经验 ROC 曲线的一个基本性质，即它在标记物的单调变换下是不变的。为了理解这一概念，想象使用 $h(t)$ 而不是 $t$ 作为标记物，其中 $h(.)$ 是一个单调函数（单调函数保持数据的秩；统计学中使用的几乎所有变换，如对数或幂，都是单调的）。$F$ 和 $G$ 对 $h(t)$ 的形状与对 $t$ 的形状相同；唯一的区别在于它们的水平轴。这只会改变阈值，而不会改变由此产生的 ROC 曲线。

## 3.4 ROC 曲线下面积

ROC 曲线是对连续预测因子准确性的概括。然而，有时你可能想要对 ROC 曲线本身进行概括。ROC 曲线最常用的汇总统计量是 ROC 曲线下面积（AUC）。

在经验 ROC 曲线中，你可以通过所谓的梯形法则来估计 AUC：

- 1. 使用观察点作为角点形成梯形。
- 2. 用角点的坐标计算这些梯形的面积。
- 3. 将这些面积相加。

对于像图 3.3 中那样具有许多可能阈值的曲线，这可能是一项相当大的工作。幸运的是，AUC 与一些众所周知的统计量有关，例如一致性、Somers’ D 和 Mann-Whitney 检验统计量。这种联系与第 3.3 节中讨论的不变性属性有关。对单调变换的不变性意味着 ROC 曲线是一个基于秩的度量。从观察到的标记值中使用的唯一信息是它们相对的秩。

我们将利用这些关系不仅便于计算，而且有助于深入了解曲线下面积的含义并改善解释。特别是，我们将看到如何使用 PROC LOGISTIC 估计一致性以及使用 PROC FREQ 估计 Somers’ D。AUC 与 Mann-Whitney 统计量之间的关系在概念化方面与其他关系一样有用，但在 SAS 角度来看并不那么有用。这里未探讨它。

### 3.4.1 一致性及使用 PROC LOGISTIC 计算 AUC

一致性概率衡量预测和结果一致的频率。以 PET 示例继续，如果在随机选择的患者对中，SUV 较高的那位患有癌症，而 SUV 较低的那位没有癌症，则该对被称为一致对。SUV 较高的患者没有癌症但 SUV 较低的患者有癌症的对被称为不一致对。一些对的 SUV 相同，它们被称为结对。最后，一些对是非信息性的；例如，两位患者都可能患有癌症或都可能没有患癌症。这些对无法归类为一致或不一致。

一致性概率定义为一致对数加上结对数的一半除以所有信息对数（即排除非信息对）。本质上，每个结对被计为一半不一致和一半一致。这是在许多秩程序中一个基本概念，有时被称为随机打破结。

表达一致性的等价方式是 P(SUV₊>SUV₋)，其中 SUV₋ 表示金标准阴性患者的 SUV，SUV₊ 表示金标准阳性患者的 SUV。图 3.1 有助于可视化这一过程。如果从上部直方图中随机选择一个 SUV，从下部直方图中选择一个，选自下部的那个高于选自上部的那个的几率是多少？

对我们而言最重要的结果是，ROC 曲线下面积等于各种 SAS 过程报告的一致性概率。例如，PROC LOGISTIC 在其标准输出的“Association of Predicted Probabilities and Observed Responses”标题下报告一致性。使用生成 ROC 曲线绘图数据集的同一 PROC LOGISTIC 调用，你可以从标记为“c”的输出中获得 AUC。在 PET 示例中，这为 0.871（或 87.1%），如输出 3.1 所示。输出还包含计算的元素（即一致对、不一致对和结对的数量）。

### Output 3.1

| Association of Predicted Probabilities and Observed Responses |
| Percent Concordant 86.1 Somers' D 0.743 |
| Percent Discordant 11.8 Gamma 0.758 |
| Percent Tied 2.1 Tau-a 0.348 |
| Pairs 7638 c 0.871 |

AUC 是从数据估计的，因此此估计必须有一个标准误差。使用 PROC LOGISTIC 估计 AUC 的主要缺点是相关的标准误差不可用。幸运的是，PROC FREQ 为此提供了间接替代方案。

### 3.4.2 Somers’ D 和使用 PROC FREQ 的 AUC

PROC LOGISTIC 输出还包括 Somers’ D。D 和 AUC 通过方程 D=2*(AUC-0.5) 相关。Somers’ D 只是 AUC（或一致性）的一个重新缩放版本，其取值在 -1 到 1 之间，像通常的相关系数一样，而不是 0 到 1。PROC LOGISTIC 不报告 Somers’ D 的标准误差。然而，PROC FREQ 报告 Somers’ D 及其标准误差，如以下示例所示：

```
proc freq data=HeadNeck;
    table suv*gold_standard / noprint measures;
run;
```

NOPRINT 选项抑制频率表的打印，因为 SUV 的每个唯一值将被解释为表的行，频率表可能长达数页。MEASURES 选项确保打印各种关联度量，包括 Somers’ D。输出 3.2 显示了此 PROC FREQ 调用的结果。

### Output 3.2

| Statistic | Value | ASE |
|---|---|---|
| Gamma | 0.7585 | 0.0498 |
| Kendall's Tau-b | 0.5374 | 0.0402 |
| Stuart's Tau-c | 0.6925 | 0.0561 |
| Somers' D C|R | 0.3889 | 0.0323 |
| Somers' D R|C | 0.7426 | 0.0523 |
| Pearson Correlation | 0.6187 | 0.0395 |
| Spearman Correlation | 0.6311 | 0.0466 |
| Lambda Asymmetric C|R | 0.6716 | 0.0634 |
| Lambda Asymmetric R|C | 0.0081 | 0.0180 |
| Lambda Symmetric | 0.2408 | 0.0318 |
| Uncertainty Coefficient C|R | 0.6252 | 0.0521 |
| Uncertainty Coefficient R|C | 0.1225 | 0.0104 |
| Uncertainty Coefficient Symmetric | 0.2049 | 0.0172 |
| Sample Size = 181 | | |

报告了两个 Somers’ 统计量：C|R 使用列变量作为预测因子，行变量作为金标准，而 R|C 使用行变量作为预测因子，列变量作为金标准。这里相关的统计量是 R|C，报告为 0.7426，标准误差为 0.0523。如果表是这样构造的，即标记物（SUV）是列变量，金标准是行变量，那么我们将使用 Somers’ D C|R 统计量。

