
## 处理表格

重要的是，条件应该是一个返回单个逻辑值的表达式。条件后面的代码段将在该值为 `TRUE` 时执行，如果该值为 `FALSE`，则被忽略。例如，如果 `x` 不大于 2，由于 `print("x is large!")` 表达式不会被执行，因此什么也不会发生：

```r
> x = 0
> if(x > 2) {print("x is large!")}
```

屏幕上不会打印任何内容。

> 与函数定义（参见前一章）和 for 循环（参见下一节）一样，只包含一个表达式的代码不必用花括号 `{` 括起来。

另外，我们可以使用 `else` 运算符来添加另一段代码。当 `if` 中的条件为 `FALSE` 时，`else` 运算符之后的代码段将被执行，如下所示：

```r
> x = 3
> if(x > 2) {print("x is large!")} else {print("x is small!")}
[1] "x is large!"
> x = 1
> if(x > 2) {print("x is large!")} else {print("x is small!")}
[1] "x is small!"
```

还有一个专门用于逐元素处理向量的条件运算符，称为 `ifelse`。使用 `ifelse` 时，我们需要提供三个参数：一个逻辑向量、一个用于 TRUE 的值（yes 参数）以及一个用于 FALSE 的值（no 参数）。我们得到的将是一个与输入逻辑向量等长的新向量，其中 TRUE 和 FALSE 值已被我们提供的替代值所替换。

关于 TRUE 和 FALSE 的替代值，最常用的操作模式有两种：一种是将它们设为长度为 1 的向量（然后它们会被循环使用以填充整个逻辑向量的长度），另一种是将它们设为与逻辑向量等长的向量（然后逻辑向量的元素会被替换为分别来自 yes 或 no 向量的对应元素）。

例如，当我们想根据某个条件将给定向量的值分成两类时，第一种模式很有用：

```r
> dat$mmxt[1:7]
[1]  5.6  9.8  7.2 12.2 11.8 19.6 24.1
> ifelse(dat$mmxt[1:7] < 10, "cold", "warm")
[1] "cold" "cold" "cold" "warm" "warm" "warm" "warm"
```

## 对向量的不同部分应用函数

`tapply` 函数用于对向量的不同部分应用函数，然后将结果合并为单个对象。为此，我们需要为以下三个参数提供对应的值：

-   向量 A，函数将对其进行操作（x）
-   向量 B，用于定义向量 A 的子集（INDEX）
-   将应用于向量 A 子集的函数（FUN）

作为示例，我们将使用一个小型表格，它是 iris 数据集（在 R 中键入 iris 即可获得）中随机抽取的六行子集（原始数据共 150 行）。这些数据是三种不同鸢尾花物种（第五列，Species）的不同植株（行）的四个花部性状（前四列）的测量值。你可以用 iris=iris[sample(1:nrow(iris),6),] 创建类似下面示例的 data.frame 对象（注意，由于是随机抽样，每次得到的具体数值都会不同）。示例中使用的确切表格可在本书网站（iris2.csv）上找到。下面是我们将要使用的 iris 数据集子集：

```
> iris
      Sepal.Length Sepal.Width Petal.Length Petal.Width Species
100          5.7        2.8         4.1        1.3 versicolor
45           5.1        3.8         1.9        0.4 setosa
90           5.5        2.5         4.0        1.3 versicolor
34           5.5        4.2         1.4        0.2 setosa
38           4.9        3.6         1.4        0.1 setosa
101          6.3        3.3         6.0        2.5 virginica
```

使用 tapply，我们可以快速求出每个物种的平均花瓣宽度，如下所示：

```
> x = tapply(iris$Petal.Width, iris$Species, mean)
> x
      setosa versicolor virginica
0.2333333 1.3000000 2.5000000
```

第一个参数 iris$Petal.Width 是我们应用函数的向量。第二个参数 iris$Species 是定义 iris$Petal.Width 子集的向量。基本上，iris$Petal.Width 中与 iris$Species 的每一个唯一值对应的位置上的所有元素都被视为一组。最后一个参数是我们应用于 iris$Petal.Width 子集的函数；在本例中是 mean 函数。因此，iris$Petal.Width 向量被分成三个子集，对每个子集计算平均值，然后将结果再次合并。

`tapply` 返回的对象是一个 `array`，这是一个带有额外属性的向量，该属性说明了其维度的数量和大小。一维数组就是我们这里得到的结果，在用法上与向量完全相同。然而，`tapply` 返回的对象之所以是数组，是因为在某些情况下（这里我们不讨论），返回的对象会有多个维度，因此无法用向量表示（例如，当应用的函数返回多个值时，如 `range` 函数）。我们将在下一章进一步详细阐述二维（`matrix`）和三维（`array`）这类类似向量的对象。

请注意，该数组使用分组向量中的值来命名，因此我们可以使用其名称访问任意感兴趣的值，如下所示：

```
> x["setosa"]
  setosa
  0.2333333
```

此外，如果愿意，我们可以使用 `as.numeric` 将结果转换为向量，如下所示：

```
> as.numeric(x)
[1] 0.2333333 1.3000000 2.5000000
```

如前所述，`apply` 函数族在目的和概念上与循环类似，但语法更简单、更清晰。例如，上面的操作也可以用 `for` 循环来完成，尽管代码会更长（而且可以说更不清晰）：

```
> x = NULL
> for(i in unique(iris$Species)) {
+ x = c(x, mean(iris$Petal.Width[iris$Species == i]))
+ }
> names(x) = unique(iris$Species)
> x
versicolor  setosa  virginica
1.3000000 0.2333333 2.5000000
```

在这里，我们创建一个空对象（使用 `NULL`，这是 R 中表示空对象的特殊值），然后使用循环遍历 `iris$Species` 中的唯一值，每次将 `iris$Petal.Width` 的均值添加到 x 中对应的物种上。最后，我们使用 `names` 函数编辑结果向量的 names 属性，以添加唯一的物种名称。

![](img/ebfd6294770b225fdeceda9a7b8b560e_94_0.png)

让我们再看一个使用 tapply 处理气候数据的示例。假设我们想了解有多少个站点（以及哪些站点）在其各自的降水量时间序列中至少有一个缺失值。对于单个站点（例如名为 "IZANA SP" 的站点），我们可以按如下方式检查其 tpcp 列是否包含至少一个 NA 值：

```
> any(is.na(dat[dat$station_name == "IZANA SP", "tpcp"]))
[1] TRUE
```

返回值为 TRUE，表示答案是肯定的。请注意，该操作包含三个步骤。我们首先创建了 dat 的一个子集（包含站点名为 "IZANA SP" 且列名为 "tpcp" 的行）。由于该子集是从单个列创建的，因此会自动简化为向量。其次，我们使用 is.na 函数检查每个元素是否为 NA。最后，我们使用 any 函数检查结果逻辑向量中是否至少有一个元素为 TRUE。

要立即对所有站点执行此操作，我们可以使用 tapply：

```
> result = tapply(
+ dat$tpcp,
+ dat$station_name,
+ function(x) any(is.na(x)))
```

这次，我们在 tapply 函数中使用的值向量是 dat$tpcp（因为我们想在降水数据中查找缺失值），而定义子集的向量是 dat$station_name（因为我们想分别对每个站点的数据应用函数）。最后，我们应用的函数是一个用户自定义函数；为了简洁，其定义被包含在 tapply 函数调用中。该函数接受一个参数（x），并根据 x 是否至少包含一个 NA 值返回 TRUE 或 FALSE，与我们在上一段代码中的做法相同。

结果数组指示每个站点是否至少有一个降水量测量值缺失。以下是其前十个元素：

```
> result[1:10]
        A CORUNA ALVEDRO SP             A CORUNA SP
                    FALSE                     FALSE
    ALBACETE LOS LLANOS SP         ALBACETE OBS. SP
                    FALSE                     FALSE
      ALMERIA AEROPUERTO SP        ASTURIAS AVILES SP
                    FALSE                     FALSE
                AVILA SP BADAJOZ TALAVERA LA REAL SP
```
