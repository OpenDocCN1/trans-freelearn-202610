

17. 现在，单击 Datasets。注意，数据共享尚未映射到接收方侧的存储帐户：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_367_0.png)

图 10.42 – 在消费者的订阅中检查接收到的共享

18. 单击 Map to target 并提供以下值：

- Target data type: Azure data lake storage Gen2
- Subscription: Free Tier or Paid Subscription
- Resource group: training_rg
- Storage accounts: <Consumers Storage Account>

消费者可以自由选择其订阅中的任何存储帐户来接收共享数据。

- Path: goldshared:

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_368_0.png)

图 10.43 – 在消费者的订阅中将数据集映射到目标

19. 单击 Map to target。确认屏幕应如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_368_1.png)

图 10.44 – 在消费者的订阅中确认数据映射

20. 现在，我们已准备好**触发**一次完整的快照，将共享数据从**提供者**帐户复制到**消费者**存储帐户。

在我们触发**完整快照**之前，我们需要为存储帐户授予一个角色。

使用新的浏览器标签页，导航到 Azure 门户。

单击 **Home > All Resources > <Consumers Storage Account>**。

然后，使用左侧菜单，单击 **Access Control (IAM)**。

在此处，单击 **Add role assignment**。

现在，选择 **Owner** 并单击 **Next**。

选择 **User, group, or service principal**，然后单击 **Select members**。

选择 Azure **principal**，其名称以用户登录 Azure 门户时使用的相同帐户名称开头。单击 **Select** 然后 **Next**：

# Add role assignment

Role
Members
Review + assign

**Selected role**
Owner

**Assign access to**
● User, group, or service principal
○ Managed identity

**Members**
+ Select members

| Name | Object ID | Type |
|------|-----------|------|
| [redacted] | 39ea52c6-0ad8-4736-b35a-da3222e579... | User |

图 10.45 – 为消费者的订阅添加角色分配

最后，单击 **Review + assign**。

21. 返回到 Data share 选项卡。单击 Trigger snapshot 并选择 Full copy：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_370_0.png)

图 10.46 – 在消费者的订阅中触发新的快照

22. 这将触发从提供者帐户到消费者帐户的完整快照复制。注意通知区域中的消息。此区域应显示快照复制的状态：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_370_1.png)

图 10.47 – 在消费者的订阅中检查快照创建的通知

23. 经过几分钟后，返回存储帐户。你应该在存储帐户中看到一个名为 goldshared 的新容器，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_371_0.png)

图 10.48 – 在消费者的订阅中检查新的快照

24. 现在，如果你单击 goldshared，你应该会看到以下文件夹：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_371_1.png)

图 10.49 – 在消费者的订阅中检查数据共享的内容

25. 现在，单击任意文件夹并检查文件是否已成功复制，如下所示：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_372_0.png)

图 10.50 – 在消费者的订阅中检查 blob

在这里，我们能够以简单、安全和可问责的方式在提供者和消费者之间成功共享数据。接下来，我们将研究组织在处理敏感数据时面临的另一个关键挑战。

## Data governance

我以“每字节数据都有一个故事可讲。真正的问题是这个故事是否被准确、安全、高效地讲述”作为本书的开篇。虽然组织忙于利用数据的真正力量，但数据治理和安全经常被忽视。但它不能被忽视太久。GDPR 等法规正在对未能满足与数据隐私、保留和可移植性相关的治理政策的组织施加法律问责和严格的处罚。

有效的 data governance 路径通常始于有效的数据发现、分类和跟踪血缘的方法。考虑到数据种类、量和速度的巨大差异，这可能是一项艰巨的任务。Azure Purview 是一种新的统一治理服务，可自动化诸如发现、分类和血缘等任务。

使用 Azure Purview，注册的数据源可以自动发现一次或定期按计划发现。发现的数据基于预定义或自定义规则进行分类。一旦数据被编目，用户可以通过提供描述、标签和访问规则手动增强元数据。让我们探索 Azure Purview 如何有效地处理此问题。

## Preparing the Azure resources

我们将开始准备 Azure Purview 帐户：

- 1. 在 Azure 门户搜索中，搜索 `Purview accounts` 并导航到它。
- 2. 在 **Purview accounts** 页面，单击 **Create Purview account** 并提供以下值：
  - **Subscription:** `Free Tier or Paid Subscription`
  - **Resource group:** `training_rg`
  - **Purview account name:** `trainingcatalog`
  - **Location:** `East US 2:`

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_373_0.png)

图 10.51 – 创建新的 Purview 帐户

单击 Review + Create：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_374_0.png)

图 10.52 – 指定新 Purview 帐户的参数

单击 Create。部署完成后，单击 Go to Resource：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_374_1.png)

图 10.53 – 新 Purview 帐户的主屏幕

现在我们有了 Azure Purview 帐户，我们已准备好创建数据目录。

## Creating a data catalog

数据目录是数据资产及其关系的集合。它通过使终端用户能够使用术语、分类和敏感性标签发现相关数据来促进有效的治理。按照以下步骤创建它：

- 1. 单击 **Open Purview Studio:**

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_375_0.png)

图 10.54 – trainingcatalog 的 Purview Studio 主屏幕

2. 要注册数据源，单击 Register Sources，然后单击 Register：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_376_0.png)

图 10.55 – 在 trainingcatalog 中选择 Azure Data Lake Storage Gen2

选择 Azure Data Lake Storage Gen2。

单击 Continue 并提供以下值：

- Name: trainingsource
- Subscription: Free Tier or Paid Subscription
- Storage account name: traininglakehouse
- Endpoint: 根据之前的选择自动填充

- Select a collection: None:

# Register sources (Azure Data Lake Storage Gen2)

Name *
trainingsource

Azure subscription
Azure subscription 1 (0b35426c-85f3-45e8-9f96-577a13b738d5)

Storage account name *
traininglakehouse

Endpoint
https://traininglakehouse.dfs.core.windows.net/

Select a collection
None

图 10.56 – 在 trainingcatalog 中注册新源

3. 单击 Register：

# Sources

Register   New collection   Refresh   Map view   Table view

Filter by keyword

Showing 0 collections, 1 source

trainingsource
Azure Data Lake Storage Gen2
View details

图 10.57 – 确认 trainingsource 的创建

4. 我们现在准备扫描我们创建的数据源。在继续之前，我们需要确保 trainingsource 具有扫描 traininglakehouse 的适当权限。

使用 Azure 门户，导航到 Home > All resources > traininglakehouse。

5. 使用左侧菜单，单击 Access Control (IAM)：

![](img/dae8a2243a3c3cbcd9c56ff3d2217672_378_0.png)

图 10.58 – traininglakehouse 的 Access Control (IAM)

单击 + Add，然后 Add role assignment。提供以下值：

- Role: Storage Blob Data Reader
- Assign access to: User, group, or service principal
- Select: trainingcatalog:

