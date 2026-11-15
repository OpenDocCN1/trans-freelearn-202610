

5.  从 `Marks` 卡中移除 `AGG(Profit to Budget Profit Ratio)`，并注意到百分比标签不再被遮挡。

6.  点击 `Color` 货架并选择 `Edit Colors`。在弹出的对话框中，完成以下步骤：

    1.  双击 `Quota Met` 并将颜色设置为白色。

    2.  双击 `Quota Missed` 并将颜色设置为黑色。

7.  在点击每个对话框的 `OK` 并返回主屏幕后，再次点击 `Color` 货架，并为 `Border` 选择黑色。

8.  点击 `Size` 货架，通过向左拖动滑块来缩窄条形的宽度。

9.  右键点击 `Profit` 轴并选择 `Edit Reference Line`。然后将 `Value` 设置为 `90%,95%,100% of Average Budget Profit`：

![](Images/B18435_08_05.png)

Figure 8.5: Reference line

1.  在同一窗口中，向下移动到 `Formatting` 部分，将 `Fill` 颜色设置为 `Grey Light`。勾选 `Fill Below` 复选框和 `Reverse` 复选框。注意背景颜色现在更容易区分了：

![](Images/B18435_08_06.png)

Figure 8.6: Background colors

1.  右键点击标记为 `Greater of Profit or Budget Profit` 的轴并取消选择 `Show Header`。您可能希望进行一些额外的微调，但我们的结果如下所示：

![](Images/B18435_08_07.png)

Figure 8.7: Improved bullet graph

请注意，上述每个问题现在都已得到解决：

*   百分比数字不再被遮挡。
*   由于缩窄了条形，背景颜色更容易区分。
*   条形的颜色更加直观。此外，使用黑、白、灰三色规避了因色盲或灰度打印而导致的任何可读性问题。
*   图例中的 `False` 和 `True` 一词已被替换为更具描述性的术语 `Quota Met` 和 `Quota Missed`。

完成本节后，您将了解如何通过微调来改进可视化效果和所选图表类型。无论何时，只要您目前的选择还不能讲完整个故事，就可以这样做。此外，这也是一个卖点。用户喜欢精美的设计和清晰的仪表板。通过使用更高级的技术改进可视化效果，您将能够改进您的叙事和营销。

在下一节中，我们将继续为已知的 `visualizations` 增加复杂性。这次我们将创建饼图和甜甜圈图，并最终将两者结合。

## Pies and donuts

饼图在数据可视化领域通常不受青睐。它们有太多缺点。例如，饼图在矩形屏幕上无法很好地利用空间。矩形树图更合适。此外，饼图上合理的扇区数量相当有限，最好也就六到八个。矩形树图再次胜出，因为它们可以在更细的粒度级别上切片，同时保持实用。最后，使用饼图时，很难分辨两个大小相似的扇区哪个最大。在这方面矩形树图也好不到哪里去；然而，如果观察者理解矩形树图的排序是从左上角到右下角，就可以利用该知识来区分大小差异。当然，条形图完全规避了这个特定问题，因为眼睛可以轻松区分宽度和高度，但难以分辨角度（饼图）和体积（矩形树图）。

尽管存在这些缺点，但由于其普及度，饼图在未来几年内可能仍将在数据可视化中被广泛使用。人们喜欢它们，因为它们不同于老旧、枯燥的条形图。用户熟悉饼图，它们不需要任何解释，并且是展示比例的一种方式——尽管在我看来这不是最好的方式。对于悲观的 Tableau 制作者，最好的办法是忍气吞声。但对于那些愿意探索和突破边界的人来说，可以发掘出饼图的良好用途。以下练习就是对这种探索的一种贡献。

### Pies and donuts on maps

有时需要（或自以为需要）在地图上构建饼图。该过程并不困难（正如您将在以下练习中看到的），但存在一些无法轻易克服的缺点。我们将在练习后讨论这些缺点。

步骤如下：

1.  在与本章关联的工作簿中，导航到名为 `Pie Map` 的工作表并选择 `Superstore` 数据源。

2.  在 `Data` 窗格中，双击 `State` 以创建美国地图。

3.  将 `Sales` 放置在 `Color` 货架上。点击 `Color` 货架并将 `Palette` 更改为 `Grey`：

![](Images/B18435_08_08.png)

Figure 8.8: Edit Colors

1.  按住 Windows 下的 `Ctrl` 或 Mac 下的 `Command` 键，同时拖动 `Latitude (generated)` 的额外副本到 `Rows` 货架上，以创建两行，每行显示一张地图：

![](Images/B18435_08_09.png)

Figure 8.9: Latitude

1.  在 `Marks` 卡中，您会注意到现在有三个面板：`All`、`Latitude (generated)` 和 `Latitude (generated) (2)`。点击 `Latitude (generated) (2)` 并将标记类型设置为 `Pie`：

![](Images/B18435_08_10.png)

Figure 8.10: Marks card panes

1.  将 `Category` 放置在 `Color` 货架上，将 `Sales` 放置在 `Size` 货架上。

2.  要完成可视化，请右键点击 `Rows` 货架上第二个 `Latitude (generated)` 实例并选择 `Dual Axis`：

![](Images/B18435_08_11.png)

Figure 8.11: Pie map

您能在可视化中发现问题吗？应该立刻能看出两个问题：

*   首先，较小的饼图难以看清。点击 `Size` 图例的下拉菜单并选择 `Edit Sizes` 可以部分解决此问题，但 `Rhode Island` 等较小州份的饼图仍将存在问题。
*   其次，尽管销售额差异巨大，但许多州具有相同的浅灰色背景。

以下方法将解决这些问题，同时增加额外功能。

### Pies and donuts – beyond the basics

以下是在平铺网格地图上创建饼图和甜甜圈图所需的步骤。通过结合不同的方法，我们将能够一次性展示更多信息，而不会使视图过载：

1.  在与本章关联的工作簿中，导航到名为 `Altered Pie Map` 的工作表并选择 `Superstore` 数据源。

2.  创建以下计算字段：

    | **Name** | **Code** |
    | --- | --- |
    | `Category State Sales` | ``` {FIXED State, Category: SUM(Sales)} ``` |
    | `State Max` | ``` {FIXED State : MAX([Category State Sales])} ``` |
    | `Top Selling Category per State` | ``` MAX(If [State Max] = [Category State Sales] then Category END) ``` |

1.  我们需要前两个 `Level of Detail` (`LOD`) 计算和最后一个计算，以在显示每个州最畅销品类的同时，展示每个品类的销售额。

2.  在 `Marks` 卡中，将标记类型设置为 `Pie`。

3.  在 `Data` 窗格中，选择 `States` 数据源。

4.  点击 `Data` 窗格中 `State` 旁边的链接图标，以使用 `State` 作为混合字段：

![](Images/B18435_08_12.png)

Figure 8.12: Linking dimensions

1.  将 `Column` 拖至 `Columns` 货架，将 `Row` 拖至 `Rows` 货架。

2.  从 `Superstore` 数据源中，将 `Category` 放置在 `Color` 货架上，将 `Sales` 放置在 `Angle` 货架上。

3.  在 `Marks` 卡中将 `State` 放置在 `Detail` 上。

4.  点击 `Size` 货架并根据需要调整大小。

