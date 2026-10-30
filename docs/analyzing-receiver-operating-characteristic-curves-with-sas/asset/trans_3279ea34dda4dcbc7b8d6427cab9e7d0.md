

Output 4.1 显示了此调用的结果。每个 ROC 曲线的 AUC 估计值及其标准误差和（边际）置信区间首先出现。BSI 的 AUC 为 0.7795，与 SUV 的 0.6773 相比相当有利。然而标准误差相当大：BSI 为 0.0771，SUV 为 0.0836，导致置信区间很宽。BSI 的 AUC 的 95% 置信区间范围从 0.6284 到 0.9307。SUV 的置信区间在 0.5134 到 0.8412 之间。似乎 SUV 几乎无法与随机预测器区分开来（因为下限置信限仅略高于 0.50）。

## Output 4.1

```
The ROC Macro

    ROC Curve Areas and 95%
          Confidence Intervals
    ROC Area Std Error Confidence Limits

BSI    0.7795    0.0771    0.6284    0.9307
suv1   0.6773    0.0836    0.5134    0.8412

    Contrast Coefficients
            BSI       suv1

Row1       1        -1

        Tests and 95% Confidence Intervals for Contrast Rows
    Estimate  Std Error Confidence Limits     Chi-square Pr > ChiSq

Row1   0.1023    0.0954   -0.0847     0.2893      1.1491   0.2837

        Contrast Test Results
Chi-Square   DF      Pr > ChiSq

    1.1491    1      0.2837
```

接下来出现对比信息。这可用于验证结果是否针对预期比较，并推断差异的方向。在此示例中，AUC 曲线的正差异指向 BSI 是更好的检验。下一部分输出比较了两个 ROC 曲线。两个 AUC 的差异为 0.1023，如 Estimate 列下所示。这可以直接通过 0.7795 和 0.6773 之间的差值获得，即输出第一部分中报告的 BSI 和 SUV 的 AUC。然而，你不能根据单个面积的信息计算差异的标准误差，因为数据是配对的。基于此标准误差，两个 AUC 之间的差异不显著（p=0.2837），置信区间为（-0.0847，0.2893）跨越 0。

似乎检验统计量和 p 值被报告了两次。如果对比涉及多于一行，则对比行上的输出将为对比中的每一行包含一行。标记为“Contrast Test Results”的部分仍将报告一个具有 r−1 自由度的单一 p 值，其中 r 为行数。因此，只有在对比只有一行时，“Contrast Rows”部分和“Contrast Test”部分的信息才一致。

如本文所述，必须呈现 ROC 曲线的可视化显示以及 AUC 的比较。图 4.1 由第 3 章中介绍的 %PLOTROC 宏生成。语法修改与 %ROC 宏中的类似（即多个标记在 VAR 宏变量中指定，用空格分隔）。

```
%plotroc (bone bsi,suv,gold,anno=4,tlist1=1.08 2.29, tlist2=4.8 5.8);
```

## Figure 4.1 ROC Curves for BSI (Solid) and SUV (Dashed) Overlaid

![](img/ac26020abd4f22abfaafc0607939c6e4_51_0.png)

图 4.1 显示 BSI 在特异性的大多数取值下比 SUV 具有更高的敏感性。在敏感性为 50% 至 80% 的范围内，BSI 的敏感性明显高于 SUV。BSI 的阈值为 2.29 可实现超过 80% 的敏感性和超过 70% 的特异性。SUV 的任何操作点都无法提供可比的准确性。尽管如此，请记住，显示的 AUC 估计具有较高的变异性，并且差异没有统计学意义。

## 4.4.2 Comparisons with Unpaired Data

从理论上讲，%ROC 宏只能在数据为配对时比较两个预测因子的 AUC。由于在 ROC 曲线领域中非配对设计非常罕见，没有宏执行所需比较。幸运的是，非配对比较可以通过手工或几行 DATA 步代码轻松完成：如果 AUC₁ 和 AUC₂ 是两个 AUC 估计值，其相关标准误差为 s₁ 和 s₂，则非配对比较的检验统计量如下：

T = (AUC₁ - AUC₂)² / (s₁² + s₂²)

在大样本中，在零假设下（即两个 AUC 确实相等时），T 将具有一个自由度的卡方分布。

请注意，AUC₁ 和 s₁ 是在仅对第一个预测因子执行 %ROC 宏时报告的，如第 3 章中所述。同样，AUC₂ 和 s₂ 是从第二次 %ROC 调用估计的。一旦计算出 T，两侧 p 值可使用 DATA 步中的以下行找到：

```
p_value=1-probchi(t_unpaired,1);
```

如果前列癌研究仅提供 PET 或 BSI（但不是两者），使得 resulting 数据结构为非配对，并且如果 AUC 估计值及其标准误差与之前所述相同（AUC 为 0.7795 和 0.6773，标准误差为 0.0771 和 0.0836），则 T 将为 0.8095，p 值为 0.3683。使用非配对设计 p 值增加可通过配对设计的有效性增加来解释，如第 4.3 节中简要讨论的那样。

## 4.5 Comparing the Binormal ROC Curves

第 3 章表明，你可以通过假设标记在患病组和未患病组的分布具有正态分布且参数可能不同来获得 ROC 曲线的模型化估计。具体而言，如果患病患者的标记值服从均值为 μ₁ 和方差为 σ₁² 的正态分布，并且未患病患者的标记值服从均值为 μ₀ 和方差为 σ₀² 的正态分布，则 ROC 曲线可函数写为

Φ(a + bΦ⁻¹(x))

AUC 给出为

Φ( a / √(1+b²) )

其中双正态参数 a 和 b 给出为

a = (μ₁ - μ₀)/σ₁,   b = σ₀/σ₁

接下来，我们详细讨论配对数据；关于非配对数据的评论出现在本节末尾。

### 4.5.1 Comparisons Based on the Binormal Model with Paired Data

数据的配对性质需要在继续之前对模型进行一些调整。设 Xᵢⱼ 表示第 i 个患者和第 j 个标记的标记值。然后 Xᵢⱼ（或其某种变换）假定在患者有阳性结果时具有以下正态分布：

Xᵢⱼ ~ N( μ₁ⱼ + uᵢ, σ₁ⱼ² )

如果患者有阴性结果，则 $X_{ij}$ 的分布为

$X_{ij} \sim N\left(\mu_{0j}+u_i,\sigma_{0j}^2\right)$

$u_i$ 也服从正态分布：

$u_i \sim N\left(0,\sigma_r^2\right)$

关键假设是 $u_i$ 和 $u_{i'}$ 对于 $i\neq i'$ 是独立的。

该模型为每个标记有四个基本参数：$\mu_{1i}$ 和 $\mu_{0i}$ 是标记 $j$ 的结果阳性和结果阴性患者的均值，$\sigma_{1i}$ 和 $\sigma_{0i}$ 是两个标准差。在这方面，它是每个标记单独的标准双正态模型。为了考虑同一受试者上两个标记测量之间的相关性，引入了随机效应 $u$。因为 $u_i$ 是受试者特异性效应，在同一患者上测量的两个标记值将具有相同的 $u_i$。这种共同性在同一受试者上进行的两个测量（每个标记一个）上诱导出相关性。因为 $u_i$ 和 $u_{i'}$ 对于 $i\neq i'$ 是独立的，所以在不同受试者上测量的标记值之间没有相关性。在大多数设置中使用随机效应来诱导受试者选定对上的相关性是一种标准技术。

以下代码段首先运行 PROC TRANSREG 以确定正态性变换。请参阅第 3.7 节，了解此步骤在拟合双正态模型中的作用的详细解释。

```
proc transreg data=bone;
    model boxcox(suv1/parameter=0.1 geometricmean)=class(gold);
    output out=trsuv;
run;

proc transreg data=bone;
    model boxcox(bsi/parameter=0.1 geometricmean)=class(gold);
    output out=trbsi;
run;
```

Box-Cox 模型确定对数变换对 BSI 和平方根变换对 SUV 是实现正态性的最佳可能变换（未显示详细的 PROC TRANSREG 输出）。输出数据集 TRSUV 和 TRBSI 由 PROC TRANSREG 中的 OUTPUT 语句创建，并包含变换后的协变量 TBSI 和 TSUV。这两个数据集合并形成 Long，即长格式的数据集。然后 PROC NLMIXED 找到如前所述的随机效应双正态模型的极大似然估计。

```
data long;
    set trbsi(keep=_name_ gold tbsi rename=(tbsi=result _name_=subid)
        in=in1)
        trsuv(keep=_name_ gold tsuv rename=(tsuv=result _name_=subid)
        in=in2);
    if in1 then marker=0; else if in2 then marker=1;
run;
```
