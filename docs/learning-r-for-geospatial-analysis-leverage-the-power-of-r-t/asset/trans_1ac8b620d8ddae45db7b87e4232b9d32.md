

```
+ v_mod =
+ autofitVariogram(
+ formula = form,
+ input_data = dat)$var_model}
+ # (3) Create gstat object
+ g = gstat(formula = form, data = dat, model = v_mod)
+ # (4) Perform cross-validation
+ cv = gstat.cv(g)
+ # (5) Calculate RMSE and assign to cv_results
+ cv_results$rmse[i] = rmse(cv)
+ }
```

注意，在步骤 2 中，使用了嵌套的条件语句，以便根据所使用的插值方法确定 form 和 v_mod。我们可以将此特定代码段执行的流程写成决策树的形式：

+   - 如果方法为 "IDW"，则 form 为 value~1 且 v_mod 为 NULL
+   - 否则：
+     - 如果方法为 "OK"，则 form 为 value~1
+     - 如果方法为 "UK"，则 form 为 value~elevation
+     - v_mod 根据 form 计算得出

一旦循环执行完成（这可能需要几分钟），cv_results 的 rmse 列将用得到的 RMSE 值填充，而不是 NA 值：

```
> head(cv_results)
     variable year method    rmse
1      mmnt 2001    IDW 1.942872
2      mmxt 2001    IDW 1.594996
3      mmnt 2002    IDW 1.880490
4      mmxt 2002    IDW 1.570574
5      mmnt 2003    IDW 1.912887
6      mmxt 2003    IDW 1.605938
```

注意，例如，第三行表格中的 RMSE 值（1.880490），它对应于 "mmnt" 在 2002 年的 IDW 插值，正如预期的那样，与我们之前手动得到的结果相同。利用此 RMSE 表可以进行多种有趣的比较。如前所述，我们主要感兴趣的是在 10 年和两个变量上，平均而言哪种插值方法最准确。要找出答案，我们可以使用 tapply：

```
> tapply(cv_results$rmse, cv_results$method, mean)
        IDW        OK        UK
1.773243 1.908957 1.458997
```

结果显示，平均而言，UK 在三种方法中预测误差最低。有趣的是，OK 的平均准确性低于 IDW，尽管在我们之前看到的特定例子中情况正好相反。基于此结果，UK 将成为我们制作西班牙年平均气温图的首选方法。

> 需要注意的是，比较交叉验证 RMSE 只有在对同一数据的不同空间预测模型之间才有意义。否则，RMSE 可能更高或更低，不仅因为预测误差不同，还因为预测值本身处于不同的尺度。例如，在我们的案例中，降水量（"tpcp"）预测的 RMSE 会远高于温度预测的 RMSE（你可以尝试看看），仅仅因为西班牙通常观测到的年降水量（以 mm 计）比温度（以摄氏度计）高出一个数量级。然而，这并不意味着相对于测量变量本身的尺度，降水的预测模型就不如准确。

我们准备继续，使用另一个循环来计算预测值（而不是我们刚刚完成的交叉验证）。同样，做此操作的一种井然有序的方式是构建一个描述我们每个空间插值所需参数集的表格，然后遍历该表格并依次执行所需的操作。我们将再次使用 expand.grid 函数，这次仅产生两个变量的所有组合：year（2006-2010 的 5 年）和 variable（"mmnt" 和 "mmxt"）。方法将保持未指定，因为我们已经决定它应始终为 UK。

```
> spainT_tab = expand.grid(
+ year = 2006:2010,
+ variable = c("mmnt", "mmxt"))
```

由此产生的表格，命名为 `spainT_tab`，如下所示：

```
> spainT_tab
  year variable
1  2006      mmnt
2  2007      mmnt
3  2008      mmnt
4  2009      mmnt
5  2010      mmnt
6  2006      mmxt
7  2007      mmxt
8  2008      mmxt
9  2009      mmxt
10 2010      mmxt
```

我们准备创建第二个循环，该循环将遍历 `spainT_tab` 的行，并对相应的温度数据进行空间插值。由于我们的结果将是栅格（而不是交叉验证循环中那样为数值），因此它们不能附加到表格本身。相反，我们将把结果堆叠成一个多波段栅格，其中每个图层对应 `spainT_tab` 中的一行。换句话说，`spainT_tab` 将作为结果多波段栅格的补充表格发挥作用（正如 `dates` 对栅格 `r` 所起的作用；参见第 4 章，Working with Rasters）。为了构建栅格，我们将首先创建一个空的 `RasterStack`，在循环执行期间插值结果将被附加到其中：

```
> spainT = stack()
```

循环将遍历 `spainT_tab` 的行，每次执行以下操作集：

+   1. 根据 `spainT_tab` 的当前行创建名为 `dat` 的点图层，并设置变量和年份。
+   2. 拟合变异函数模型（`v`）。
+   3. 根据 `dat` 和 `v` 创建 `gstat` 对象（`g`）。
+   4. 基于 `g` 和 `dem_spain` 使用 `interpolate` 计算预测值栅格（`z`）。
+   5. 根据 `dem_spain` 对 `z` 进行掩膜处理并将结果附加到 `spainT`。

当我们只对年际差异感兴趣时，最合理的做法是找出特征年平均气温状态，找出每个年份状态与特征状态的偏差，然后比较偏差图像而不是原始图像。从总体趋势中计算偏差是突出我们数据中感兴趣模式的非常常见做法，并且有许多方法可以找到总体趋势以及每个观测值与其的偏差。在这里，我们将采用最简单的方法——每个像素中的特征温度将定义为该像素中观测到的 5 年平均值，而偏差将等于每个观测值与平均值的算术差。

由于我们需要分别处理最低和最高温度数据，我们将首先创建两个向量来标识每种情况下相关的 `spainT` 图层：

```
```r
> mmnt_layers = which(spainT_tab$variable == "mmnt")
> mmnt_layers
[1] 1 2 3 4 5
> mmxt_layers = which(spainT_tab$variable == "mmxt")
> mmxt_layers
[1]  6  7  8  9 10
```
```

我们还将创建一个新的双波段 RasterStack，命名为 means，用于保存其第一和第二图层中的最低和最高平均温度模式。两个图层也将相应命名为（"mmnt" 和 "mmxt"）：

```
```r
> means = stack(mean(spainT[[mmnt_layers]]),
+ mean(spainT[[mmxt_layers]]))
> names(means) = c("mmnt", "mmxt")
```
```

现在我们有了均值，唯一需要做的就是从 spainT 中的每一组图层中减去适当的均值（从图层 1-5 中减去最低温度的均值，从图层 6-10 中减去最高温度的均值）。有许多方法可以做到这一点。例如，我们可以将 spainT 栅格分为两部分，从每个子堆栈中减去相应的均值，然后再次合并它们。然而，更优雅且可扩展到任意数量类别的方法是，通过复制创建具有与 spainT 相同图层数目的新 means 栅格，其中相关均值占据其图层。换句话说，我们需要一个栅格，其中 means[[1]] 被复制五次（占据图层 1-5），并且 means[[2]] 被复制五次（占据图层 6-10）。为此，我们可以利用栅格图层也可以按名称选择这一事实。因此，以下表达式取 "mmnt" 和 "mmxt" 图层并根据 spainT_tab$variable 列进行复制：

```
```r
> means = means[[spainT_tab$variable]]
```
```

