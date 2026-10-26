

## 使用表格

![](img/ebfd6294770b225fdeceda9a7b8b560e_100_0.png)

> 为节省空间，从现在开始，`install.packages` 命令将不会作为代码段的一部分书写，也不会在每次使用包时都重复使用 `library` 加载包的命令。相反，每当代码段需要一个新贡献的包时，你应确保相应的包已加载。

请记住，下载一个包（使用 `install.packages` 函数）是一次性操作（除非包的新版本发布且我们希望重新安装）。另一方面，将包加载到内存中（使用 `library` 函数）是我们需要在每个新的 R 会话中执行的操作。

![](img/ebfd6294770b225fdeceda9a7b8b560e_100_1.png)

以下是关于包安装的更多提示：

- 包安装也可以通过 RGui 和 RStudio 中的菜单访问
- 当触发包安装时，用户会被提示选择一个 CRAN 镜像来下载包
- 当安装新版本的 R 时，所有包都需要重新安装

## 使用 melt 和 dcast 在长格式和宽格式之间转换

在本节和下一节中，你将学习几种有用的重塑数据的方法。重塑操作是数据分析过程中的必经步骤，因为在大部分时间里，我们处理的数据结构与使用特定函数或软件类型所需的结构不同。在数据重塑中，我们改变数据的形式（可能通过聚合减少其数量），但不改变数据本身。例如，Excel 中的 PivotTable 工具就是用于数据重塑的工具之一。

我们将在本节和后续章节中使用的函数属于贡献包 reshape2 和 plyr。 在 R 中执行所展示操作还有其他方法；其中一些方法仅使用基础包。然而，这里展示的方法被认为更直观、更灵活。由其作者 Hadley Wickham 对这两个包的介绍可在《Journal of Statistical Software》期刊中找到（见附录 B，参考文献）。请注意，其中一篇论文涉及 reshape 包（而不是后来开发的更高效的 reshape2 包），但 reshape 和 reshape2 的原理相同，因此该论文对 reshape2 用户也非常相关。Paul Murrell 在优秀著作《Introduction to Data Technologies》（2009）中提供了使用（主要是）基础包进行 R 数据操作的好介绍（该书也可在网上获取）。

你将要学习到的第一个操作是在宽格式和长格式之间转换。宽表格为每个测量变量包含一列。例如，我们的 dat 表就是宽格式，因为它有一个站点名称列、一个降水量列、一个最低温度列，等等。长表格是其中一列保存变量名，另一列保存测量值的表格。从宽格式切换到长格式时，我们通常对中间形式感兴趣，其中一些变量在列中，而其他变量在保存变量名的单列中标识。在 reshape2 术语中，前者称为标识符变量（id.vars），后者称为测量变量（measure.vars）。使用示例会更清楚。

我们之前看到的 iris 数据集也处于宽格式。要将其转换为长格式，我们可以使用 melt 函数，该函数用于将宽格式转换为长格式。使用 melt 时，我们需要指定要重塑的 data.frame 对象，以及 ID 和测量变量的身份（作为字符向量）。事实上，我们可以只指定 ID 或测量变量，函数会假设其余列属于另一类。例如，在这种特定情况下，最合理的方法是使用 species 作为 ID 变量（因为它描述了每个测量单元），并将花朵尺寸性状作为测量变量（因为它们是对每个测量单元进行的独立测量）。让我们看一下以下示例：

```
> library(reshape2)
> iris_melt = melt(iris, id.vars = "Species")
```

## 使用表格

结果表格的前几行打印如下：

```
```text
> head(iris_melt)
      Species     variable value
1 versicolor Sepal.Length   5.7
2     setosa Sepal.Length   5.1
3 versicolor Sepal.Length   5.5
4     setosa Sepal.Length   5.5
5     setosa Sepal.Length   4.9
6  virginica Sepal.Length   6.3
```
```

我们可以看到 ID 变量（本例中只有一个，Species）保留了其作为列之一的状态。其余列，即测量变量（本例中有四个），消失了；取而代之的是创建了两个新列（variable 和 value），分别保存测量变量名称和值。行数现在为 150（为原始行数的四倍，150），因为 ID 部分被复制了四次，每次对应一个测量变量。

类似地，我们可以使用 melt 函数将气候数据表 dat 转换为长格式，指定 tpcp、mmxt 和 mmnt 列包含测量变量。我们将其分配给名为 dat_melt 的不同对象：

```
```text
> dat_melt = melt(dat, measure.vars = c("tpcp","mmxt","mmnt"))
> head(dat_melt)
             station station_name elevation latitude longitude
1 GHCND:SP000060010       IZANA SP       2371  28.3089  -16.4992
2 GHCND:SP000060010       IZANA SP       2371  28.3089  -16.4992
3 GHCND:SP000060010       IZANA SP       2371  28.3089  -16.4992
4 GHCND:SP000060010       IZANA SP       2371  28.3089  -16.4992
5 GHCND:SP000060010       IZANA SP       2371  28.3089  -16.4992
6 GHCND:SP000060010       IZANA SP       2371  28.3089  -16.4992
          date month year variable value
1   1984-01-01     1 1984     tpcp  51.4
2   1984-02-01     2 1984     tpcp   0.0
3   1984-03-01     3 1984     tpcp  68.7
4   1984-04-01     4 1984     tpcp  13.6
5   1984-05-01     5 1984     tpcp  21.4
6   1984-06-01     6 1984     tpcp   6.5
```
```

我们可以检查并看到熔化后的表格 dat_melt 的行数正好是原始表格 dat 的三倍。

## 使用表格

或者，我们可以让月份形成新的列，如下所示：

```
> dat2 = dcast(dat_melt, station+station_name+variable+year~month)
> head(dat2)
        station      station_name variable year    1    2    3
1 GHCND:SP000003195 MADRID RETIRO SP     tpcp 1984 25.3 37.2 58.0
2 GHCND:SP000003195 MADRID RETIRO SP     tpcp 1985 67.8 45.1  6.0
3 GHCND:SP000003195 MADRID RETIRO SP     tpcp 1986 10.6 57.0 22.2
4 GHCND:SP000003195 MADRID RETIRO SP     tpcp 1987 93.3 42.9  6.7
5 GHCND:SP000003195 MADRID RETIRO SP     tpcp 1988 60.0 20.9  1.1
6 GHCND:SP000003195 MADRID RETIRO SP     tpcp 1989  9.9 19.5 23.8

             4    5    6    7    8    9   10   11   12
1         39.2 82.4 35.6  0.0  7.1  7.1 30.1 161.0 11.3
2         36.8 29.9 24.1  5.4  0.0  0.0  0.0  39.9 83.8
3         57.3 12.0  1.0 37.1 16.4 47.5 93.8  13.6 19.0
4         63.1 58.4  7.8 43.9 14.5 11.9 58.5  64.8 79.1
5         96.6 46.5 49.8  9.5  0.0  0.0 79.2  50.1  0.2
6         52.2 97.7 12.7 13.3  3.2 37.5  5.9 146.4 138.6
```

> 请注意，这次我们省略了一些变量从结果表中（elevation、latitude 等），并使月份的水平（共有 12 个）出现在单独的列中，各列的单元格中包含每个月的相应气候变量值。这种形式非常适合回答诸如每个站点哪个月最暖和这样的问题（例如使用 apply）。

我们迄今为止执行的转换操作都涉及在结果表中保留所有原始数据。当我们指示创建无法包含所有原始数据的表时会发生什么？在这种情况下，会进行聚合，我们需要指定用于聚合的函数（否则，默认函数 length 将被使用）。例如，我们可以按年份计算西班牙的平均气候条件，如下所示：

```
> dat2 = dcast(dat_melt, year ~ variable, mean, na.rm = TRUE)
> head(dat2)
   year       tpcp      tmxt      mmnt
1  1984 54.34180 19.38194  9.381115
2  1985 45.13103 20.31096  9.793890
3  1986 47.55329 19.75327  9.681250
4  1987 57.09826 20.31684 10.350206
5  1988 47.78863 20.07662  9.934514
6  1989 54.82944 20.97615 10.654617
```

