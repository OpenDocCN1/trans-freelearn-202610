

- **关系**：这些与列表类似，但可以把多个值组合在一起，如图 4.17 所示。此关系中的每一行都包含有关我们产品的有用信息。而每一列本质上是一个列表，如果我们想要访问所有产品的列表，这很有用：

## Product Info (Relation)

| # | Product (string) | Weight in Grams (number) | Vegan Suitable (boolean) |
| --- | --- | --- | --- |
| 1 | "Peanut Crunch" | 75 | true |
| 2 | "Caramel Biscuit" | 125 | false |

Figure 4.17 – Relation Decision Node showing product information

## Relations are not Decision Tables

由于表格格式，关系看起来与决策表非常相似。尽管外观相似，但它们的工作方式截然不同。关系会返回其包含的所有信息。决策表会匹配最佳行并仅返回该信息。

关系非常适合定义一次信息并在多个决策模型中重复使用。实际上，此产品信息表将使用调用从外部系统源中提取。对于现在，将信息建模为一个地方的关系效果很好。随着系统的演进，这也使其更容易在以后进行更改。

## Play around with the nodes

本节中介绍的大多数节点一旦开始使用就很容易理解。我们鼓励你在 KIE Decision 编辑器中尝试使用它们，并使用 Runner 查看输出。例如，图 4.18 显示了我们运行 Product Info 关系节点时的输出：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_115_0.png)

Figure 4.18 – Output from a Relation Decision Node showing product information

我们将在示例中使用一些这些表达式和节点来帮助你更好地理解它们。由于所有节点都是 **Object Model Group (OMG)** 决策模型标准的一部分，网上有很多关于每个节点的信息和示例。开始的好地方是 learn DMN 网站（是 KIE 的一部分）：https://learn-dmn-in-15-minutes.com/learn/decision-logic。Drools 网站（也是 KIE 的一部分）上有更详细的信息：https://www.drools.org/learn/dmn.html。

## Adding nodes to support our Decision Table

我们之前的决策表示例很简单：一个数据输入节点和一个生成输出的表。由于决策模型的强大之处在于将许多节点组合在一起，让我们开始构建一个链接多个节点的示例。

此示例将允许我们根据客户出生所在的星期几推荐不同的巧克力棒。这来自真实的例子，可能并不比你被要求建模的一些业务规则更离谱。

我们将基于之前决策表示例开始工作，可从本书的 GitHub 站点下载为 `04_First_Decision_Table.dmn`：

- 1. 打开 Decision Model 编辑器，并添加 **DayCalc** 决策节点。
- 2. 创建一条箭头连接现有的 **Customer Data Note** 以将数据馈送到这个新的 **DayCalc** 节点。
- 3. 创建另一条箭头连接 **DayCalc** 节点到现有的 **Recommended Product** 表。
- 4. 打开 **DayCalc** 节点的 **Properties editor** 并将输出设置为 **string**。它应如下 *Figure 4.19* 所示：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_116_0.png)

Figure 4.19 – Creating a Function Decision Node

- 5. 接下来，通过单击节点旁边的钢笔图标编辑 **DayCalc** 节点。
- 6. 在出现的屏幕上，将 **Expression type** 设置为 **Function**。
- 7. 通过单击 **Edit Parameters** 框编辑此函数的参数。在弹出菜单中，单击 **Add parameter**，如 *Figure 4.20* 所示：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_116_1.png)

Figure 4.20 – Adding parameters to the function

- 8. 输入一个名为 DOB 的参数，并将其数据类型设为 **Date**。单击任意位置退出空白菜单。

9. 现在你回到节点编辑器，在表达式框中输入 day of week (DOB)。你猜对了——这是一个 FEEL 表达式，返回与此日期关联的星期几（例如 Monday 或 Tuesday）。通过此编辑，你的节点应类似于图 4.21：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_117_0.png)

Figure 4.21 – Entering the function

如果我们正确输入了公式（没有拼写错误），你应该在 Run 面板中看到函数 DayCalc( DOB ) - Evaluated with success 的输出。

虽然知道 KIE 能理解我们的函数很有用，但我们还没有通过传入日期调用它并收到响应。

## Running our function

虽然我们已将决策模型接线以使函数可用于决策表，但查看正在发生的事情会很有帮助。为此，我们将向决策模型添加另一个节点以允许我们可视化输出：

- 1. 打开我们之前的示例，添加另一个决策节点，并将其命名为 Customer Birth Day。
- 2. 从 DayCalc 函数链接新节点。你的模型应类似于图 4.22：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_117_1.png)

Figure 4.22 – Creating a Decision Node to test the function

- 3. 打开 **Customer Birth Day** 的属性，为其指定 **string** 数据类型。
- 4. 编辑 **Customer Birthday** 决策节点，并将 **Logic type** 设置为 **Context**。当然，调用函数的方式有很多，但这是一个练习上下文决策节点的有用借口。
- 5. 在你选择上下文类型后出现的屏幕上，按如下方式添加条目，使其看起来像 *Figure 4.23*。你需要插入新行并从下拉菜单中选择 **string** 数据类型。`DayCalc(Customer.Date of Birth)` 和 `Day of Week` 是字面表达式：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_118_0.png)

- 6. 打开 **Run** 面板并从 **Date of Birth** 输入字段的日期选择器中选择一个日期——例如 **2000-01-01**。
- 7. 立即，KIE 将评估决策模型，包括我们刚刚创建的 **Customer Birth Day** 节点。我们将看到客户出生的星期几；在此例中，是星期六。

> **Tricks of the trade**
> 
> 添加输出节点以便我们可以看到正在发生的事情是调试的有用工具（尝试找出为什么某些东西没有按我们想要的方式工作）。由于没有人第一次就能做对，我们在 *Chapter 9* 中更详细地讨论了有用的测试技术。

我们使用的 **context** 节点非常简单：一个计算和一个输出行。如果需要，我们可以添加额外的行并让 KIE 逐步构建结果。同样，在我们继续构建更复杂的决策表之前，值得在 KIE 编辑器中练习此示例。

## A more sophisticated Decision Table

我们添加了一个函数节点来计算客户出生的星期几。但我们仍然没有使用该星期几来推荐合适的产品。让我们更新我们之前的示例（可下载为 04_Customer_Birth_Day.dmn）以实现这一点：

- 1. 打开 Decision Model，右键单击 **Recommended product** 决策节点，并选择编辑图标。
- 2. 添加 **Input Clauses**（淡蓝色），使你拥有图 4.24 所示的三个输入列。你可以通过右键单击现有输入列并选择 **Add right** 来实现。
- 3. 双击每个输入子句以设置数据的来源，如图 4.24 所示。`Customer.Country of Residence` 和 `Customer.Number` 直接来自我们的数据模型。`DayCalc(Customer.Date of Birth)` 调用我们创建的函数（一次）并使结果可用于与规则匹配。
- 4. 我们调整了一些列的大小并拖放以重新排序——但这不会改变规则的功能。
- 5. 在表的正文中，输入如图所示的值。请注意文本值所需的引号。
- 6. - 值表示不尝试匹配，而注释只是解释正在发生的事情的注释，否则会被忽略。

RecommendedProduct (Decision Table)

| F | Customer.Country of Residence (string) | DayCalc(Customer.Date of Birth) (string) | Customer.Number (number) | RecommendedProduct (string) | annotation-1 |
|---|----------------------------------------|------------------------------------------|--------------------------|-----------------------------|--------------|
| 1 | - | - | <10000 | "Silk Tray" | Only first 10000 custom |
| 2 | "SA" | - | - | "Lumpy Fruit and Nut Bar" | |
| 3 | "UK" | "Friday" | - | "Crunch Bar" | |
| 4 | "US", "UK" | - | - | "Peanut Candy" | |
| 5 | - | - | - | "Milk Chocolate" | |

Figure 4.24 – A more sophisticated decision table

7. 打开 **Run** 面板以评估我们的模型。据预期，**US** 客户会被推荐 **Peanut Candy**。**UK** 客户会被推荐 **Crunch Bar**——但仅当他们出生于 **Friday** 时。否则，客户将收到默认建议的 **Milk Chocolate** 棒。并且任何在前 10,000 名客户中的人都会被推荐我们的特别 **Silk Tray** 优惠。

