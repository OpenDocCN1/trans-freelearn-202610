

并且调整后 ROC 曲线的形式如下：

y = x^{θ(u)}

其中

θ(U) = β₁ + β₃U

正如第 5 章所述，协变量的 ROC 曲线不受 β₂ 影响，但必须在模型中包含年龄的主效应；否则，β₃ 的估计值，即 ROC 曲线的关键组成部分，可能会有偏倚。

继续以磁共振为例，年龄可能是这种类型的磁共振成像准确性的一个重要因素，该成像专注于组织中的水脂比。由于患者随着年龄增长自然会积累脂肪，该标记物在老年人中可能效果较差，存在将因年龄导致的脂肪误认为是良性病变导致的脂肪的可能性。

下面的 PHREG 调用提供了所需的估计值：

```
proc phreg data=tmp1.ph;
    model signal=grp age grpage/r1;
    if group='normal' then grp=0;
    else if group='benign' then grp=1;
    grpage=grp*age;
run;
```

再次注意编程语句的使用，不仅用于创建组变量，还用于创建交互项。输出 6.2 包含来自 PHREG 过程的相关数字。三个参数估计为 β₁=1.29，β₂=-0.003，和 β₃=-0.014。年龄系数的负号表明准确性随年龄增加而下降。输出 6.2 显示了结果。

## Output 6.2

| Analysis of Maximum Likelihood Estimates |
| Variable | DF | Parameter Estimate | Standard Error | Chi-Square | Pr > ChiSq | Hazard Ratio | 95% Hazard Ratio Confidence Limits |
| grp | 1 | 1.28789 | 0.44629 | 8.3276 | 0.0039 | 3.625 | 1.512 | 8.694 |
| Age | 1 | -0.00297 | 0.00320 | 0.8650 | 0.3523 | 0.997 | 0.991 | 1.003 |
| grpage | 1 | -0.01432 | 0.00706 | 4.1123 | 0.0426 | 0.986 | 0.972 | 1.000 |

将 β₁ 和 β₃ 代入 θ(u) = β₁ + β₃U 得到以下方程：

θ(AGE) = 1.288 − 0.014*AGE

将其代入 $y = x^{\theta(u)}$ 后，得到年龄调整后的 ROC 曲线：

$y = x^{1.288 - 0.014*AGE}$

你可以通过绘制几个选定年龄值的 ROC 曲线来获得图形表示（见图 6.4）。图 6.4 中的每条 ROC 曲线都是 $y = x^{1.288 - 0.014*AGE}$ 在不同年龄值下的绘制。最低曲线代表 70 岁患者，最高曲线代表 40 岁患者。中间的曲线代表以 5 年为增量的不同年龄水平。正如 $\beta_3$ 的负号所预期的那样，随着年龄的增加，ROC 曲线越来越接近对角线。

## Figure 6.4 ROC Curves for Different Age Values

![](img/ac26020abd4f22abfaafc0607939c6e4_85_0.png)

也可以回答是否需要协变量调整的问题。请注意，当 $\beta_3=0$ 时，调整后和未调整的 ROC 曲线重合。因此，对 $\beta_3=0$ 的检验是一种判断是否应报告调整后或未调整 ROC 曲线的方法。该检验的 $p$-值为 0.0426（见输出 6.2）；因此，基于此分析，年龄调整后的 ROC 曲线是合适的。

## 6.5 Using Estimating Equations to Handle Clustered Data

迄今为止描述的所有方法都要求各个数据点彼此独立。当对每个个体单元只进行一次观测时，通常满足这一要求。这里的个体单元可以是等待诊断的患者、申请贷款的债务人，或用于发布天气预报的日历日。

正如磁共振示例中所述，有时可能从一个个体单元获得多个观测值。在这种情况下，研究问题集中于识别异常椎体骨髓过程并将其分类为正常或良性。进行磁共振成像的放射科医生根据图像指示测量信号的椎体数量。有些患者可能只提供一个测量值，而其他患者则提供多个测量值。当患者对分析贡献多个观测值时， resulting 数据集被称为 *clustered*。该术语源于这样的观察：来自同一单元的数据表现相似并倾向于聚在一起。

思考聚类数据的直观方式是，两个独立数据点所包含的信息通常多于两个聚类数据点所包含的信息。聚类使得个体数据点彼此表现得更像，而不像原本那样。净结果通常是标准误差增加（与数据点独立相比），而点估计通常不会受到实质性影响。

可以在其他领域想到聚类数据的例子。某一天的天气预报通常是多维的：温度、降水和气压都可能同时被预测。同一天的预测会倾向于聚类，因为它们都与相同的潜在气象事件的各个方面相关。

聚类数据并不总是源于每个观测单元的多次测量。也可能有其他原因怀疑观测值的一个子集彼此表现相似。例如，同一邮政编码区的债务人可能表现出相似的违约行为。同一工厂制造的零件可能表现出相似的故障模式。所有这些都会影响 ROC 曲线的分析。

聚类数据的分析本身就是一个统计学主题。从 SAS 用户的角度来看的概述可在 Littell 等人（2006）所著的 *SAS for Mixed Models, Second Edition* 中找到，适用于连续结果，以及 Stokes、Davis 和 Koch（2001）所著的 *Categorical Data Analysis Using the SAS System, Second Edition*。

处理聚类数据的一种方法是基于广义估计方程。这是一种使用对数据点独立性假设的偏离具有稳健性的方法来估计模型参数的方法。这种方法在分析相关的二元数据时非常流行，并且已扩展到删失数据的情形。它可以使用 PROC PHREG 中的 COVSANDWICH 选项拟合。该选项以协方差的夹心估计量命名，后者是估计方程得到的估计量的特征之一。

如果我们按如下方式修改对 PROC PHREG 的调用，则模型将使用估计方程拟合，而不是通常的部分似然。ID 语句定义了标识聚类变量的变量。共享相同 ID 值的记录被聚类。

```
proc phreg data=tpm1.ph covsandwich(aggregate);
    model signal=group/r1;
    if group='Normal' then grp=0;
    else if group='Benign' then grp=1;
    id name;
run;
```

使用 COVSANDWICH 选项时，PHREG 输出的格式不会改变。特别是我们看到 θ 估计为 1/1.427=0.701（与第 6.3 节相同），支持聚类对点估计几乎没有影响甚至没有影响的观点。由于 θ̂ 没有改变，AUC 的估计值保持在 0.588 不变。另一方面，β̂ 的标准误差现在为 0.144，从 0.088 上升，增加了 63.5%，如 StdErrRatio 列所示，这是 COVS 选项存在时 PROC PHREG 输出的唯一新增内容。见输出 6.3。这导致 θ̂ 的标准误差增加到 0.101，AUC 的标准误差从 0.030 增加到 0.050。

## Output 6.3

| Analysis of Maximum Likelihood Estimates |
| Parameter | DF | Parameter Estimate | Standard Error | StdErr Ratio | Chi-Square | Pr > ChiSq |
| group benign | 1 | 0.35541 | 0.14413 | 1.635 | 6.0808 | 0.0137 |
| Analysis of Maximum Likelihood Estimates |
| Parameter | Hazard Ratio | 95% Hazard Ratio Confidence Limits | Variable Label |
| group benign | 1.427 | 1.076 1.892 | group benign |

图 6.5 在图 6.4 的基础上增加了基于估计方程的置信带。虚线均表示 95% 置信区间，一个基于假设独立性的标准误差（较窄的带），另一个基于估计方程（较宽的带）。该图无意帮助你选择估计方法。是否应使用估计方程取决于根据研究的性质是否合理地预期存在聚类。

