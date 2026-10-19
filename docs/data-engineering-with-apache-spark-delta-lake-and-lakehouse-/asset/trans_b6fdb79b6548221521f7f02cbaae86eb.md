

## 构建流式摄取管道

现在我们将继续进行 **Electroniz** 的流式摄取管道。在发现会议期间，我们了解到 **Electroniz** 电子商务网站会持续向 **Azure Event Hubs** 发送新的销售交易。在前一节中，我们已成功创建了 **Electroniz** 流式源组件——一个 **Azure 事件网格**、一个 **Azure 事件中心** 和一个 **Azure 事件中心命名空间**：

+   1. 让我们开始向之前创建的 Azure 事件中心发送一些数据。请查看以下文件：https://github.com/PacktPublishing/Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/blob/main/project/prep/ecommerce/eventhub/ecomm_orders_1.txt。

这里有几笔交易，可以让我们使用 Azure Event Hubs Capture 测试事件向存储的交付。逐行复制以下每个命令，并将其粘贴到 Cloud Shell 窗口中。然后，按 Enter：

```
RESOURCEGROUPNAME="training_rg"
LOCATION="eastus"
TOPIC="esales"
EVENTHUB_NAMESPACE="esalesns"
EVENTHUB_NAME="esaleshub"
EVENT_SUBSCRIPTION="esalesevent"
sh Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/prep/ecommerce/eventhub/sendevent_1.sh -e $EVENTHUB_NAME -n $EVENTHUB_NAMESPACE -r $RESOURCEGROUPNAME -s $EVENT_SUBSCRIPTION -t $TOPIC
```

调用前述命令后，您的 Azure Cloud Shell 应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_175_0.png)

图 5.64 – 确认已向 Azure Event Hubs 发送的示例事件

+   2. 为确保我们收到了示例事件，请导航到 **Home | All Resources | esalesns。**

使用左侧窗格，单击 **Event Hubs**。在右侧窗格上，单击 **esaleshub。**

为确保 Azure Event Hubs 正在接收事件，请单击 **Process data**。使用 Azure Event Hubs 进程生成的数据，您可以预览流中的数据：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_176_0.png)

图 5.65 – 显示进程数据和捕获事件的 Azure Event Hubs 屏幕

+   3. 单击 **Explore**。从 **Query** 窗口，注意有关权限的消息。单击 **Create：**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_176_1.png)

图 5.66 – Azure Event Hubs 进程数据权限屏幕

窗口现在应自行刷新。或者，您也可以选择 Test query：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_177_0.png)

图 5.67 – Azure Event Hubs 进程数据权限屏幕

4. 关闭 Query 窗口。使用左侧窗格，单击 Capture。输入以下内容：

+   - Capture: On
+   - Time window: 2 minutes
+   - Size Window: 10 MB

在捕获时间窗口内未发生事件时不要发出空文件：已勾选

单击 Select Container。导航到 traininglakehouse | bronze。然后，单击 Select。

最后，单击 Save changes：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_178_0.png)

图 5.68 – Azure Event Hubs 捕获数据屏幕

+   5. 要检查事件是否成功捕获到 Azure Data Lake Storage，请导航到 Home | All Resources | traininglakehouse。

然后，单击 Containers。最后，单击 bronze。几分钟后，您应该会在此容器中看到捕获的事件。浏览目录将带您进入以 Avro 格式存储的事件：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_178_1.png)

图 5.69 – 保存为 Avro 文件的示例电子商务交易

**Important**
Avro 格式使用 JSON 来定义模式并以紧凑的二进制行格式序列化数据。Avro 以无缝的模式演进而闻名——随着时间的推移适应模式更改。

如果您看到前述 Avro 文件，这意味着您已成功测试了批处理和流式管道。从之前的测试来看，数据管道似乎对现有数据反应良好，但这还不是全部。

6. 最后，让我们推送一些更多电子商务交易。逐行复制以下每个命令，并将其粘贴到 Cloud Shell 窗口中。然后，按 Enter：

```
RESOURCEGROUPNAME="training_rg"
LOCATION="eastus"
TOPIC="esales"
EVENTHUB_NAMESPACE="esalesns"
EVENTHUB_NAME="esaleshub"
EVENT_SUBSCRIPTION="esalesevent"
sh Data-Engineering-with-Apache-Spark-Delta-Lake-and-Lakehouse/project/prep/ecommerce/eventhub/sendevent_2.sh -e $EVENTHUB_NAME -n $EVENTHUB_NAMESPACE -r $RESOURCEGROUPNAME -s $EVENT_SUBSCRIPTION -t $TOPIC
```

根据前述测试用例的结果，我们可以安全地推断 Electroniz 的批处理和流式摄取管道按预期工作。我们现在已在 bronze 层中拥有原始数据，这意味着我们已准备好将重点转移到 silver 层。

下一章因多种原因非常重要。在开始创建 silver 层之前，我们需要了解湖屋架构所建立的基础框架——Delta Lake。

**Cost-saving tip**
尽管 Azure 资源的成本应由您的免费套餐覆盖，但最好还是删除任何非必需资源。此时，请随意删除除存储帐户之外的所有资源。检查当前正在运行的资源的一种快速方法是转到 Home | Resource groups | training_rg。选择您想要删除的资源，然后按菜单中的 Delete 按钮。再次强调，不要删除存储帐户，因为我们将在下一章中使用存储在其中的数据。

## Summary

在本章中，我们学习了如何通过以云架构图和管道设计文档的形式反思用户需求来启动数据管道开发。在管道设计文档中，我们决定创建摄取、整理和聚合管道，以构建湖屋的 bronze、silver 和 gold 层。

我们还更详细地了解了 bronze 层以及它与整体湖屋架构的关系。我们通过实际开发 **Electroniz** 批处理和流式摄取管道来跟进对 bronze 层的理解。现在我们已在 bronze 层中拥有来自各种数据源的原始数据，我们将继续进行 *Chapter 6, Understanding Delta Lake*，并了解 **Delta Lake**。了解 Delta Lake 的工作原理对于构建湖屋的 silver 层至关重要。

## 6
理解 Delta Lake

在上一章中，我们创建了湖屋的 bronze 层。bronze 层以从数据源收集的原生形式存储原始数据。问题是原始数据并不处于可直接用于分析操作的形状。

作为一名数据工程师，您有责任将原始数据转换为可供分析工作负载使用的形状和形式。在本章中，我们将进一步学习如何清理原始数据。清理数据的过程涉及应用清理和标准化数据的逻辑，然后将其写入湖屋的 silver 层。

但这还不是全部——silver 层应以支持 **ACID（原子性、一致性、隔离性和持久性）** 事务的开放格式存储数据。这是通过使用 **Delta Lake** 引擎完成的。在开始构建 silver 层之前，我们需要完全了解 Delta Lake 的一些关键功能以及它们与湖屋架构的关系。

在本章中，我们将涵盖以下主题：

+   - 理解 Delta Lake 如何实现湖屋
+   - 理解 Delta Lake
+   - 创建 Delta Lake 表
+   - 执行数据的 upsert
+   - 理解隔离级别
+   - 理解并发控制

## 理解 Delta Lake 如何实现湖屋

在 *Chapter 2, Discovering Storage and Compute Data Lake Architectures* 中，我们谈到了传统仓库系统与数据湖之间的取舍斗争。在过去几年中，许多组织通过远离传统数据仓库转向数据湖来现代化其数据工程和分析平台。转向数据湖无疑赋予了他们以大规模存储和计算任何格式数据的灵活性。然而，这些优势并非没有付出一些代价。最大的代价是数据的可靠性。与数据仓库一样，数据湖中没有可用的事务保证。这种需求导致了名为 Delta Lake 的开源存储层的推出。

推出后，专家们提出了将弹性数据仓库的强大功能与数据湖的灵活性相结合的想法，并将其称为 **lakehouse**。数据湖和数据仓库的结合提供了独特而强大的功能组合，如下所示：

+   - 数据可以以多种格式进行分析——结构化、半结构化或非结构化。
+   - 保留按需低成本存储和计算的理念。
+   - 支持包括分析、数据科学、机器学习和人工智能在内的各种用例。
+   - 通过整理、标准化和去重等有效技术保持数据的完整性。
+   - 提供安全存储和共享数据的方式，同时保持合规性。

