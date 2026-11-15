

## 对比联接与混合

1. 在 `Shirts` 工作表中，选择 `Shirts` 数据源，将 `Sales Associate` 放入 `Rows`（行）功能区，将 `Shirt Amount` 放入 `Text`（文本）功能区：

![](Images/B18435_04_18.png)

图 4.18：`Shirts`

2. 在 `Join` 工作表中，选择 `Join` 数据源，将 `Salesperson` 放入 `Rows`（行）功能区。接着，双击 `Pants Amount` 和 `Shirt Amount` 将两者都放入视图：

![](Images/B18435_04_19.png)

图 4.19：`Join`

3. 在 `Blend – Pants Primary` 工作表中，选择 `Pants` 数据源，将 `Salesperson` 放入 `Rows`（行）功能区，将 `Pants Amount` 放入 `Text`（文本）功能区：

![](Images/B18435_04_20.png)

图 4.20：`Blend – Pants Primary`

4. 保持在同一工作表，从左侧数据源面板中选择 `Shirts` 数据源，然后双击 `Shirt Amount`。如果弹出错误消息，请点击 `OK`。

5. 从菜单栏中选择 `Data`（数据），然后选择 `Edit Blend Relationships…`（编辑混合关系……）：

![](Images/B18435_04_21.png)

图 4.21：`Edit Blend Relationships...`

6. 在弹出的对话框中，将 `Pants` 选为 `Primary data source`（主数据源），将 `Shirts` 选为 `Secondary data source`（辅助数据源），然后点击 `Custom`（自定义）单选按钮，如图 4.22 所示，接着点击 `Add…`（添加……）。

7. 在左侧列中选择 `Salesperson`，在右侧列中选择 `Sales Associate`。左侧列代表来自 `Primary data source`（主数据源）的数据，右侧列代表 `Secondary data source`（辅助数据源）中所有可用的数据。

8. 如果有其他链接，请全部移除，然后点击 `OK`。对话框中的结果应与下面截图中显示的内容一致：

![](Images/B18435_04_22.png)

图 4.22：`Blend Relationships`

简要回顾一下我们迄今为止所做的工作：我们正在使用三个数据源：`Pants`、`Shirts` 和 `Join`，其中 `Join` 由 `Pants` 和 `Shirts` 组成。我们还创建了一个以 `Pants` 为主数据源的混合，并使用 `Salesperson` 和 `Sales Associate` 作为键将它们连接起来。

不要被名称 `Blend Relationships`（混合关系）搞混。这与逻辑层关系无关。它只是弹出窗口的名称。

回到练习，继续以下步骤：

9. 在 `Blend – Shirts Primary` 工作表中，选择 `Shirts` 数据源，将 `Sales Associate` 放入 `Rows`（行）功能区，将 `Shirt Amount` 放入 `Text`（文本）功能区：

![](Images/B18435_04_23.png)

图 4.23：`Blend – Shirts Primary`

10. 在同一工作簿中，选择 `Pants` 数据源，确保 `Salesperson` 旁边的链接图标处于连接/红色状态，然后双击 `Pants Amount` 将其添加到视图中：

![](Images/B18435_04_24.png)

图 4.24：`Finalizing the worksheet`

11. 将所有五个工作表放入一个仪表板中。根据需要进行格式化和排列。现在让我们在下面的截图中对比这五个工作表的结果：

![](Images/B18435_04_25.png)

图 4.25：`Comparison`

在上面的截图中，`Join` 工作表的结果是预期值的两倍。为什么？因为联接首先匹配公共键（在本例中为 `Salesperson`/`Sales Associate`），然后聚合结果。在公共键上找到的匹配项越多，问题就越严重。如果在公共键上找到多个匹配项，结果将呈指数级增长。两个匹配项会导致平方结果，三个会导致立方结果，以此类推。这种指数效应在下面的截图中以图形方式表示：

![](Images/B18435_04_26.png)

图 4.26：`Join`

另一方面，混合功能运行得更高效，但在混合能正常工作之前，我们必须编辑数据关系，以便 Tableau 能够使用 `Salesperson` 和 `Sales Associate` 字段连接这两个数据源。如果这两个字段名称相同（例如均为 `Salesperson`），Tableau 会自动提供一个选项，使用这些字段在数据源之间进行混合。

`Blend – Pants Primary` 和 `Blend – Shirts Primary` 工作表的结果是正确的。不存在指数效应。为什么？因为数据混合**先**聚合两个数据源的结果，**再**在公共维度上匹配结果。

在本例中，该公共维度即为 `Salesperson`/`Sales Associate`，如下截图所示：

![](Images/B18435_04_27.png)

图 4.27：`Blending results`

在本练习中我们看到，联接可能会改变数据结构，因此使用时要小心，且必须非常清楚哪些列适合作为键。此外，在联接数据前后都要进行检查，这可以简单到统计行数并检查是否为预期结果。

混合有优点也有缺点；例如，添加维度就不是那么简单直接。但在下一节中，我们将探讨更多关于辅助维度的细节。

## 添加辅助维度

数据混合虽然对于连接不同的数据源特别有用，但也有局限性。最重要的局限性是：**数据混合不支持来自辅助数据源的维度**。此限制有一个例外；也就是说，**有一种方法可以添加来自辅助数据源的维度**。让我们进一步探讨这一点。

除了 `Salesperson`/`Sales Associate` 和 `Shirt Amount`/`Pants Amount` 之外，数据源中还有其他字段。我们将在本练习中引用这些字段：

1. 在本章关联的工作簿中，选择 `Adding Secondary Dimensions` 工作表。

2. 选择 `Shirts` 数据源。

3. 为 `Material Type` 在 `Shirts` 和 `Pants` 数据源之间添加关系，执行以下步骤：

    1. 选择 `Data`（数据），然后选择 `Edit Relationships`（编辑关系）。
    2. 确保 `Shirts` 是主数据源，`Pants` 是辅助数据源。
    3. 选择 `Custom`（自定义）单选按钮。
    4. 点击 `Add…`（添加……）。
    5. 在左、右两列中均选择 `Material Type`：

![](Images/B18435_04_28.png)

图 4.28：`Custom` 混合关系

    6. 点击 `OK` 返回视图。

4. 将 `Material Type` 放入 `Rows`（行）功能区。

5. 选择 `Pants` 数据源，确保 `Data`（数据）面板中 `Material Type` 旁边的链接图标处于激活状态，而 `Salesperson` 旁边的链接图标处于非激活状态。如果图标是灰色的断开链接，则未激活；如果是橙色的连接链接，则已激活：

![](Images/B18435_04_29.png)

图 4.29：`Shirts` 和 `Pants`

6. 将 `Material Cat` 放在 `Material Type` 之前，将 `Fastener` 放在 `Material Type` 之后，均置于 `Rows`（行）功能区，如下所示：

![](Images/B18435_04_30.png)

图 4.30：`Secondary dimensions`

`Material Cat` 是来自辅助数据源的维度。数据混合不支持来自辅助数据源的维度。为什么在这种情况下它却生效了？有几个原因：

* `Material Cat` 和 `Material Type` 之间存在一对多关系；即 `Material Type` 维度的每个成员都**仅**与 `Material Cat` 维度的**一个**成员匹配。
* 视图是基于 `Material Type` 而非 `Material Cat` 进行混合的。这一点很重要，因为 `Material Type` 的粒度低于 `Material Cat`。尝试基于 `Material Cat` 混合视图将无法启用 `Material Type` 作为辅助维度。
* 主数据源中 `Material Type` 维度的每个成员在辅助数据源中也都存在。

