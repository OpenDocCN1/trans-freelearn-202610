
## 二维示例（示例 5–8）

### 示例 5

在这里，我们计算 `Category` 和 `Ship Mode` 的组合，并在每个新的 `Category` 处重新开始计数。

在第五个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：-
- `Addressing`：`Category`、`Ship Mode`
- `At the level`：`Category`
- `Restarting every`：`None`

这将生成以下可视化结果：

![](Images/B18435_05_25.png)

图 5.25：示例 5

我们计算出现在 `Category` 层级的 `Category` 和 `Ship Mode` 组合。由于 `Category` 层级在层次结构中高于 `Ship Mode`，我们最终每个 `Category` 只计算一个值。

### 示例 6

在第六个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：-
- `Addressing`：`Ship Mode`、`Category`
- `At the level`：`Deepest`
- `Restarting every`：`None`

这将生成以下可视化结果：

![](Images/B18435_05_26.png)

图 5.26：示例 6

我们在 `Ship Mode` 层级计算 `Ship Mode` 和 `Category` 的组合；因此，我们先计算 `First Class` 及其与所有 `Category` 的组合，然后再计算 `Same Day` 及其与 `Category` 的组合。

### 示例 7

在第七个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：-
- `Addressing`：`Ship Mode`、`Category`
- `At the level`：`Deepest`
- `Restarting every`：`Ship Mode`

这将生成以下可视化结果：

![](Images/B18435_05_27.png)

图 5.27：示例 7

我们在最深层级计算 `Ship Mode` 和 `Category` 的组合，但在每个 `Ship Mode` 处重新开始计数；因此，我们计算 `First Class` 和 `Furniture`、`First Class` 和 `Office Supplies`，然后是 `First Class` 和 `Technology`。接着，我们转到 `Same Day` 及其组合，再次从 1 开始计数。

### 示例 8

在第八个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：-
- `Addressing`：`Ship Mode`、`Category`
- `At the level`：`Ship Mode`
- `Restarting every`：`None`

这将生成以下可视化结果：

![](Images/B18435_05_28.png)

图 5.28：示例 8

我们在 `Ship Mode` 层级计算 `Ship Mode` 和 `Category` 的组合。

## 示例 1、4 和 8 的分析

现在，让我们更详细地考虑一下此处提出的一些可能性。某些选项是相同的。事实上，在九个选项中，只有四个是唯一的。让我们来看看示例 1、4 和 8，它们都有相同的最终结果。这是否意味着它们完全相同？选项 1 和 4 是相同的。然而，选项 8 略有不同。要理解这一点，请注意选项 8 的表计算对话框中的说明：

![](Images/B18435_05_29.png)

图 5.29：索引

示例 8 中的说明显示为 `Results are computed along Ship Mode, Category`（请参阅 `Figure 5.29`）。示例 1 说明框中的文本与示例 4 相同：`Results are computed along Ship Mode for Category`，这可以翻译为分区 `Category` 和寻址 `Ship Mode`。这种翻译与示例 1 中实际完成的分区/寻址设置相同。因此，示例 1 和 4 是相同的。但示例 8 的细微差别是否意味着存在实际差异？没有！

## 示例 6 详细分析

示例 6 乍看之下可能令人困惑。为什么会出现这种奇怪的编号序列？因为寻址维度的顺序与 `Rows` 架上的维度顺序不同。寻址顺序是 `Ship Mode`、`Category`。`Rows` 架上的顺序是 `Category`、`Ship Mode`。只需交换 `Rows` 架上 `Category` 和 `Ship Mode` 的位置，并观察编号序列的变化，即可消除任何困惑：

![](Images/B18435_05_30.png)

图 5.30：切换维度

示例 6 有实际用途吗？有的。有时可能需要按与架上列出的顺序不同的顺序来寻址维度。但这通常不是这种情况。这引出了我们的下一条准则：

> 当为表计算寻址多个维度时，寻址顺序通常将反映 `Rows` 和/或 `Columns` 架上维度的顺序。

## 使用三个维度进行分区和寻址

让我们在练习环境中添加另一个维度，并稍微重新排序。将 `Category` 和 `Region` 放在 `Rows` 架上，将 `Ship Mode` 放在 `Columns` 架上。`Index` 应保留在 `Marks` 架上的 `Text` 中。此外，添加两个筛选器。将 `Region` 筛选为 `East`、`South` 和 `West`。将 `Ship Mode` 筛选为 `First Class`、`Second Class` 和 `Standard Class`。

当对三个维度进行分区和寻址时，可能的迭代次数跃升至 57 次；然而，其中只有 14 种是唯一的。以下列出了这些唯一的可能性。

请记住，寻址意味着在表计算设置中选择维度，而分区则是取消选择维度，如 `Figure 5.17` 所述。

### 示例 1

在第一个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：`Category`、`Region`
- `Addressing`：`Ship Mode`
- `At the level`：-
- `Restarting every`：-

这将生成以下可视化结果：

![](Images/B18435_05_31.png)

图 5.31：示例 1

在这里，我们计算每个 `Category` 和 `Region` 组合下不同的 `Ship Modes`。

### 示例 2

在第二个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：`Category`、`Ship Mode`
- `Addressing`：`Region`
- `At the level`：-
- `Restarting every`：-

这将生成以下可视化结果：

![](Images/B18435_05_32.png)

图 5.32：示例 2

在这里，我们计算每个 `Category` 和 `Ship Mode` 组合下不同的 `Regions`。

### 示例 3

在第三个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：`Region`、`Ship Mode`
- `Addressing`：`Category`
- `At the level`：-
- `Restarting every`：-

这将生成以下可视化结果：

![](Images/B18435_05_33.png)

图 5.33：示例 3

在这里，我们计算每个 `Region` 和 `Ship Mode` 组合下不同的 `Category` 选项。

### 示例 4

在第四个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：`Category`
- `Addressing`：`Region`、`Ship Mode`
- `At the level`：`Deepest`
- `Restarting every`：`None`

这将生成以下可视化结果：

![](Images/B18435_05_34.png)

图 5.34：示例 4

在这里，我们在最深层级计算每个 `Region` 和 `Ship Mode` 组合下不同的 `Category` 选项。

### 示例 5

在第五个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：`Category`
- `Addressing`：`Ship Mode`、`Region`
- `At the level`：`Deepest`
- `Restarting every`：`None`

这将生成以下可视化结果：

![](Images/B18435_05_35.png)

图 5.35：示例 5

在这里，我们在最深层级计算每个 `Ship Mode` 和 `Region` 组合下不同的 `Category` 选项。

### 示例 6

在第六个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：`Region`
- `Addressing`：`Category`、`Ship Mode`
- `At the level`：`Deepest`
- `Restarting every`：`None`

这将生成以下可视化结果：

![](Images/B18435_05_36.png)

图 5.36：示例 6

在这里，我们在最深层级计算每个 `Category` 和 `Ship Mode` 组合下不同的 `Regions`。

### 示例 7

在第七个示例中，按如下方式配置分区和寻址设置：

- `Partitioning`：`Ship Mode`
- `Addressing`：`Category`、`Region`
- `At the level`：`Deepest`
- `Restarting every`：`None`

这将生成以下可视化结果：

