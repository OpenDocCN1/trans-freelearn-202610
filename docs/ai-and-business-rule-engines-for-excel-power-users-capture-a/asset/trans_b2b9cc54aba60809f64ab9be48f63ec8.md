

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_167_0.png)

图6.12 – 筛选器上下文菜单

- 4. 在筛选器框中，设置__url值（包括下划线）。单击“确定”后，你会看到数据列表现在短多了，并且**应用的步骤**列表中添加了一个额外的步骤。
- 5. 通过从菜单中选择**转换**，然后再选择**转置**，来交换行和列。
- 6. 同样在**转换**菜单中，选择**将第一行用作标题**。
- 7. 虽然我们的数据比屏幕宽，但比之前整洁多了。它看起来与*图6.13*颇为相似：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_167_1.png)

图6.13 – 筛选器上下文菜单

- 8. 数据现在可以返回Excel了。从**开始**菜单中，选择**关闭并加载**。
- 9. 在出现的对话框中，选择新工作表作为工作簿中的目标位置。最好使用新工作表而不是现有工作表，因为如果外部来源的数据重叠，Excel会发出警告。一切顺利的话，Excel会将这些信息插入为表格，如*图6.14*所示：

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_167_2.png)

图6.14 – 数据以表格形式加载到Excel中

# 1M-END：一种用于端到端图像压缩的两级深度神经网络统一架构

作者：Di Ma，Zekun Li，Shuyuan Zhu，Shiyin Qin，Bing Zeng

摘要——近年来，许多工作聚焦于使用深度神经网络的端到端图像压缩。然而，这些工作大多使用超先验...

索引词——端到端图像压缩，深度神经网络，学习型分析变换，学习型综合变换，量子...

## 1 引言

近年来，深度神经网络（DNNs）引起了越来越多的关注。在图像压缩领域，基于DNNs的端到端图像压缩方法取得了显著的性能...

在本文中，我们提出了一种用于端到端图像压缩的两级深度神经网络架构。第一级是基于非局部注意力的卷积神经网络（NLA-CNN），第二级是...

## 2 相关工作

传统的图像压缩方法，例如JPEG和HEVC，已被广泛使用...

## 3 提出的方法

本节将详细介绍所提出的架构。

### 3.1 第一级：NLA-CNN

NLA-CNN由编码器网络和解码器网络组成。编码器网络由分析变换...

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_168_0.png)

图1：所提出的基于NLA-CNN的端到端图像压缩架构的3D视图。

```
```
Algorithm 1: Training Procedure
Input: Training images x;
Output: Reconstructed image x_hat;
1: Initialize the parameters...
2: for each epoch do
3:   ...
```
```

![](img/48815d6f06dc8c7aeaa3963d289fb1e7_170_0.png)

图6.17 – 创建我们的表以供Power Query使用

- 5. Power Query将创建一个以此名称命名的表，并打开Power Query编辑器。查询的名称将是在*步骤4*的对话框中给定的名称。

现在我们已经将该表命名并加载到Power Query中，可以在Power Query M公式中引用它。你应该已经从上一步打开了Power Query编辑器。让我们创建一个查询来读取此表中的信息：

- 1. 在编辑器左侧的**查询**窗格中，右键单击并选择**新查询**、**其他源**，然后选择**空白查询**。
- 2. 创建的新查询将被称为`Query1`或类似名称。通过*右键单击*它来重命名——在可下载的示例中，我们将其命名为`RestUrl-onestep`。
- 3. 通过单击**开始**菜单上的**高级编辑器**打开高级编辑器。
- 4. 出现的屏幕只是我们之前看到的公式栏的放大版本。替换Excel建议的任何文本。将其替换为以下查询：

```powerquery
let
    // "SourceUrl" with quotes needs to match the named
    // range on our Excel sheet. You may need to change {1} to
    // {0} depending on when your first line begins
        pSourceUrl = Excel.CurrentWorkbook()
    {[Name="SourceUrl"]}[Content]{1}[Column1]
in
    pSourceUrl
```

- 5. 确保文本与所示完全一致。单击屏幕右下角的**完成**以保存代码并退出屏幕。
- 6. Power Query将运行并返回您在电子表格中输入的参数。我们不打算展示截图，因为我们所做的只是将信息从Excel的一个部分转移到另一个部分——但这是一个关键步骤！

更重要的是，我们来分析一下刚才输入的公式中发生了什么。所有M公式都遵循标准的let ... in格式。let列出获取数据并转换数据的步骤。in将数据显示给用户。在本例中，let只有一行，但其中有四个关键操作：

- `Excel.CurrentWorkbook` 获取当前工作簿
- `{ [Name="SourceUrl"] }` 从该工作簿获取该名称的范围/表
- `[Content]` 返回该范围/表的内容
- 由于我们只想从整个内容中获取一个值，因此使用`{1}[Column1]`表达式来选择我们正在查找的行号和列名

如果你想在自己的工作簿中使用此示例，只需复制粘贴这些行即可。然后用你正在使用的范围或表的名称，以及你希望从该表获取的行/列来更新它即可。

**逐步操作日后会节省你的时间**
在可下载的示例中，我们将此示例保留为一个单独的查询，即使我们稍后会将其构建到更复杂的示例中。这使我们能够检查是否有任何问题——至少我们知道我们可以正确地从电子表格中读取值，并查看这些值是什么。

有关`Excel.CurrentWorkbook`和其他Power Query M公式的更多信息，请参阅Microsoft文档：https://learn.microsoft.com/en-us/powerquery-m/excel-workbook。

由于我们将不得不关闭Power Query编辑器以继续下一步，我们将其保存为**仅限连接**。这样可以让我们的查询在Excel中不显示结果。可以通过以下方式完成此操作：

- 在Power Query编辑器中选择**关闭并加载**
- 在出现的对话框中选择**仅限连接**

我们稍后将在本章中再次讨论参数。但首先，我们需要弄清楚如何在Power Query中发出POST请求，因为我们需要它来链接到我们的决策服务。

# 使用Power Query从Excel调用我们的决策服务

在本章前面，我们使用Power Query从Excel执行了一个简单的GET请求。但我们需要链接决策服务的API更复杂——它需要通过POST请求传入JSON数据。现在让我们基于已有的Power Query知识来构建这一功能，首先从一个不含参数的硬编码示例开始。

此示例可下载为06_power_query_decision_service.xlsx，我们将逐步完成：

- 1. 如果尚未打开编辑器，请打开它（**数据 | 获取数据 | 启动Power Query编辑器**）。
- 2. 创建一个新查询（在编辑器左侧的**查询**窗格中，右键单击并选择**新查询，其他源**，然后选择**空白查询**）。你还应将查询重命名为更有意义的名称。
- 3. 在**开始**菜单中打开**高级编辑器**，然后复制并粘贴以下文本：

```
let
    // the Rest URL we are calling - you will need to
    //update this with the value from your Swagger API
    RestUrl = "https://dmn-dev-sandbox.openshiftapps.com/
Customer Recommendations/Product Recommendation Service",

    // The JSON Values we will pass in to call the
    //decision Service
    Payload = "
    {""Customer": {
        ""Number"": 0,
        ""Name"": ""string"",
        ""Date of Birth"": ""2022-10-15"",
        ""Country of Residence"": ""UK"",
        ""Special Requests"": ""lactose intolerant"",
        ""Previous Orders"": [0]
    }}
    ",
    // Do the JSON Call
    Response= Web.Contents(RestUrl, [Content=Text.
ToBinary(Payload),
        Headers=[ #"Content-Type" = "application/json",
#"Accept" = "application/json"]]),
    //Convert the response for display
    JsonResult = Json.Document(Response)

    // display the results
in
    JsonResult
```

