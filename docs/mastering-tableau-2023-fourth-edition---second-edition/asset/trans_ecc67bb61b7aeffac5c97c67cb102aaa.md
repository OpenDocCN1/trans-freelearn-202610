

本章的假设前提是：你熟悉条形图、折线图、树状图、饼图和面积图等基本图表类型，并且已经使用 Tableau 构建过多个图表。本章旨在重点介绍如何改进你可能经常使用的可视化类型，同时介绍一些你可能不熟悉但非常实用的图表类型。随后，我将向你介绍 Tableau Exchange，这是一个包含扩展程序、加速器和连接器的平台，能提供一些更独特的图表类型。

也许本章最有用的部分根本不在书中，而是在与本章关联的工作簿中。请务必下载该工作簿（链接将在下一节提供），以查看各种可视化类型。

本章将探讨以下可视化类型和主题：

*   改进常用可视化
*   自定义背景图片
*   Tableau Exchange
    *   扩展程序
    *   加速器
    *   连接器

请记住，仪表板的**内容**最重要，但如果你能用更漂亮的设计呈现相同的内容，就选择更漂亮的设计。确实存在**美感偏见**和**设计驱动消费**这种现象。你想推销你的产品——就让它看起来好看！希望接下来的章节能帮你找到方向，最终让你成为一名更优秀的仪表板设计师。

# 改进常用可视化

大多数常用可视化之所以流行，自有其道理。基本的条形图和折线图为人熟知、直观且灵活，因此在数据可视化中被广泛使用。其他不太基础的可视化，如子弹图和帕累托图，可能不是你每天都用，但却是数据分析师工具箱中有用的补充。虽然这些都很好，但它们还能做得更多。在本节中，我们将探讨如何调整、扩展甚至彻底改造一些流行的图表类型。

## 子弹图

子弹图由 Stephen Few 发明，并于 2006 年通过其著作 *Information Dashboard Design: The Effective Visual Communication of Data* 公开发布。Stephen Few 通过其著作和博客 [www.perceptualedge.com](http://www.perceptualedge.com) 持续在数据可视化领域发出强有力的声音。子弹图能在保持美观和易读的同时，将大量信息压缩在一个小空间内，从而实现高效直观的沟通。不难理解，它们广受欢迎并被用于多种用途，网络搜索即可見一斑。以下两个练习将传授子弹图的基础知识以及如何在基础上进行改进。这并不意味着我在本章中改进了子弹图！初衷仅在于阐述如何在 Tableau 中更有效地使用这一重要可视化类型。让我们开始吧。

### 使用子弹图

以下步骤将教授你子弹图的基础知识：

1.  访问 [https://public.tableau.com/profile/marleen.meier](https://public.tableau.com/profile/marleen.meier) 以查找并下载与本章关联的工作簿。

2.  导航至名为 `Bullet Graph` 的工作表，并选择 `CoffeeChain` 数据源。

3.  将以下字段放置在各自的货架上：`Profit` 放在 `Columns`（列）上，`Market` 放在 `Rows`（行）上，`BudgetProfit` 放在 `Marks`（标记）卡中的 `Detail`（详细信息）上。

4.  右键单击 *x* 轴，选择 `Add Reference Line`（添加参考线）。

5.  在 `Edit Reference Line, Band or Box`（编辑参考线、参考带或参考框）对话框的左上角，选择 `Line`（线）。同时，将 `Scope`（范围）设为 `Per Cell`（每单元格），`Value`（值）设为 `SUM(Budget Profit)` 的 `Average`（平均值），`Label`（标签）设为 `None`（无）。单击 `OK`：

    ![](Images/B18435_08_01.png)

    图 8.1：参考线

6.  让我们再添加一条参考线。这一次，作为一种替代方法，点击 `Analytics`（分析）窗格，将 `Reference Line` 拖放到工作表上。（你显然也可以重复*步骤 4*中的方法。）

7.  在对话框中，选择 `Distribution`（分布），并将 `Scope` 设为 `Per Cell`。将 `Value` 设为 `Percentages`（百分位数），数值为 `90,95,100`，`Percent of` 设为 `SUM(Budget Profit)`。将 `Label` 设为 `None`。单击 `OK`：

    ![](Images/B18435_08_02.png)

    图 8.2：百分位数

8.  创建一个名为 `Profit to Budget Profit Ratio` 的计算字段，代码如下：

    ```
    SUM([Profit])/SUM([Budget Profit])
    ```

9.  创建另一个名为 `Quota Met?` 的计算字段，代码如下：

    ```
    SUM([Profit])>=SUM([Budget Profit])
    ```

10. 右键单击 `Profit to Budget Profit Ratio`，选择 `Default Properties`（默认属性）| `Number Format`（数字格式）| `Percentage`（百分比）。

11. 将 `Profit to Budget Profit Ratio` 放在 `Marks` 卡中的 `Label`（标签）货架上，将 `Quota Met?` 放在 `Marks` 卡中的 `Color`（颜色）货架上：

    ![](Images/B18435_08_03.png)

    图 8.3：初步子弹图

在浏览我们目前的结果时，你会注意到该可视化有一些重要方面。例如，参考线和彩色条形清晰地划分了何时达标、何时未达标。此外，百分位数传达了每个市场的实际利润与预算利润的接近程度。然而，也存在一些需要解决的问题：

*   **South**（南部）市场相关的百分比被部分遮挡。
*   参考分布代表的背景颜色被遮挡。
*   条形的颜色不直观。橙色被设为 **True**（真），在本例中表示达标的市场。然而从心理学角度来看，橙色是一种警示色，用于传达问题，因此更直观地关联那些未达标的市场。此外，这些颜色在灰度模式下不易区分。
*   图例中的 **False**（假）和 **True**（真）并不直观。

在即将进行的步骤中，我们将解决这些问题并展示可能的解决方案。

### 子弹图——进阶

为了解决上一节中图表存在的问题，请执行以下步骤：

1.  接续上一个练习，访问屏幕左侧的 `Data`（数据）窗格，右键单击 `Quota Met?`，并按如下方式调整计算：

    ```
    IF SUM([Profit])>=SUM([Budget Profit])
    THEN 'Quota Met'
    ELSE 'Quota Missed'
    END
    ```

2.  如果利润高于预算利润，此计算将生成字符串 `Quota Met`（达标）；如果利润不高于预算利润，则生成字符串 `Quota Missed`（未达标）。这两个字符串可用作图例，比之前的 **True** 和 **False** 更直观。

3.  创建一个名为 `Greater of Profit or Budget Profit` 的计算字段，代码如下：

    ```
    IF SUM(Profit)>SUM([Budget Profit])
    THEN SUM(Profit)
    ELSE SUM([Budget Profit])
    END
    ```

    如果利润多于预算金额，此计算将显示利润金额；如果利润较少，则显示预算金额。这将帮助我们始终显示两者中的较大金额。

4.  将 `Greater of Profit or Budget Profit` 放在 `Columns` 货架上 `Profit` 之后。同时，右键单击该药丸，选择 `Dual Axis`（双轴）。

5.  右键单击 `Greater of Profit or Budget Profit` 的轴，选择 `Synchronize Axis`（同步轴）。

6.  在 `Marks` 卡中，选择标记为 `All` 的面板，并将标记类型设为 `Bar`（条形）。

    ![](Images/B18435_08_04.png)

    图 8.4：标记卡

7.  从 `Color` 货架上移除 `Measure Names`（度量名称）。

8.  在 `Marks` 卡中，选择标记为 `AGG(Greater of Profit or Budget Profit)` 的面板。

9.  点击 `Color` 货架，将 `Opacity`（不透明度）设为 `0%`。

10. 在 `Marks` 卡中，选择标记为 `SUM(Profit)` 的面板。

