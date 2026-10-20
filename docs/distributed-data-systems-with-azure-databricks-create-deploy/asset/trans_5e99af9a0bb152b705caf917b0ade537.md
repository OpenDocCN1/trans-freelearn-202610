

- 配置 Azure Databricks 使用自定义 DNS
- 配置网络安全组 (NSG) 规则
- 在现有的虚拟网络中部署 Azure Databricks 集群

这之所以成为可能，是因为 Azure Databricks 委托，它允许对子网进行联合控制。网络所有者允许在特定子网中部署资源，并通过指定此委托发生的条件以及添加或修改安全策略来行使控制。

## 虚拟网络要求

你打算部署 Azure Databricks 工作区的 VNet 应满足以下条件才能正常工作：

- 它应与我们正在创建的 Azure Databricks 资源具有相同的区域。
- VNet 必须属于与 Azure Databricks 资源相同的订阅。
- VNet 应为 Azure Databricks 提供专用私有和公共子网，以允许与控制平面通信。
- 地址空间：虚拟网络的 CIDR 块为 /16 – /24 之间，私有和公共子网的 CIDR 块最高为 /26。

虚拟网络允许我们在所有 Azure 资源中强制执行相同的策略。

## 部署到你自己的 VNet

我们将展示如何将集群部署到特定虚拟网络的基本方法。Azure Databricks 将在此网络内创建两个子网以及使用你提供的 CIDR 范围创建网络安全组，对入站和出站流量进行白名单，并在虚拟网络更新后进行部署。

## 部署要求

要部署 Azure Databricks，你首先必须按以下方式配置你的虚拟网络：

- 虚拟网络可以是现有的，也可以是新创建的，但正如前面提到的，区域和订阅必须与你计划创建的 Azure Databricks 工作区相同。
- 虚拟网络需要 /16 – /24 之间的 CIDR 范围。

## 配置虚拟网络

在我们*创建 Azure Databricks 资源*时，我们被提示了一个名为 Networking 的选项卡。在此选项卡中，如果我们选择部署到我们自己的虚拟网络，我们将看到部署到我们自己的虚拟网络的选项：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_71_0.png)

图 2.17 – Azure Databricks 在自定义虚拟网络中

从这里，我们可以选择要使用的网络、对互联网公开的子网、私有子网以及 CIDR 范围。设置完成后，你可以点击 Review + create 并等待你的 Azure Databricks 在你的网络中部署。

我们可以使用 Azure 资源管理器 (ARM) 更详细地指定虚拟网络的配置。这些是我们可用于将服务部署到 Azure 的模板。有了这些模板，我们可以指定是否要使用现有子网、使用现有安全组等。在下一节中，我们将更深入地研究 Azure Databricks 的 ARM 模板。

## Azure 资源管理器模板

ARM 模板是基础设施即代码，允许我们以敏捷的方式自动部署资源。这些模板是 JSON 文件，以声明方式定义基础设施和配置，指定资源和属性。我们可以作为单个资源部署多个资源，并修改现有配置。就像代码一样，它可以存储在存储库中并进行版本控制，任何人都可以运行代码来部署类似的环境。

ARM 模板随后传递给 ARM API，后者部署指定的资源。这些可以包括虚拟网络、VM 或 Azure Databricks 工作区。

这些模板有两种操作模式，即 Complete 或 Incremental 模式。当我们在 Complete 模式下部署时，这会删除模板中未指定的任何对象以及正在部署到的资源组。增量部署会向现有资源添加额外资源。

这些模板的局限性在于它们不会连同资源一起部署代码。

还重要的是要提到，如果我们使用 ARM 模板部署资源，最好先在 Azure 的另一个测试订阅中对其进行测试。这可以帮助我们验证模板是否可以通过 Azure CLI 部署，并避免例如在 Complete 模式下部署然后因 Azure 订阅配额限制而无法创建新资源的情况。我们将在下一节中看到如何做到这一点。

## 使用 ARM 模板创建 Azure Databricks 工作区

我们将了解如何使用 ARM 模板创建 Azure Databricks 工作区，然后验证其是否已正确部署。为此，我们将以 Azure Quickstart Templates 中的 ARM 模板为例。

与往常一样，要创建 Azure Databricks 工作区，我们需要一个 Azure 订阅：

1. 转到 Azure 门户，并在搜索选项卡中查找 Deployment of Custom Template。从这里，我们可以选择构建自己的模板或使用 GitHub Quickstart Template 中的模板。我们将选择此选项并查找 101-databricks-workspace，但也有使用 VNet 注入和托管密钥的选项：

## 自定义部署

部署自定义模板

选择模板   基础   Review + create

使用 Azure 资源管理器模板以单一协调操作自动化部署资源。创建或选择下方模板开始。了解有关模板部署的更多信息

在编辑器中构建自己的模板

# 常用模板

- 创建 Linux 虚拟机
- 创建 Windows 虚拟机
- 创建 Web 应用
- 创建 SQL 数据库

# 加载 GitHub 快速入门模板

快速入门模板（免责声明）  101-databricks-workspace

部署 Azure Databricks 工作区。

作者：jeffpang
最后更新：2020-09-21
了解更多

选择模板   编辑模板

图 2.18 – 自定义部署

2. 选择模板后，你需要提供所需的值，然后选择 Review + create，最后选择 Create：

Home >

# 部署 Azure Databricks 工作区

Azure 快速入门模板

选择模板   基础   Review + create

模板
101-databricks-workspace
1 个资源
编辑模板   编辑参数

# 部署范围

选择用于管理部署资源和成本的订阅。使用资源组像文件夹一样组织和管理所有资源。

订阅 *   Pay-As-You-Go
资源组 *   创建新

# 参数

区域 *   East US
工作区名称 *
定价层   premium
位置   [resourceGroup().location]

Review + create   < 上一步   下一步：Review + create >

图 2.19 – 从 ARM 模板创建 Azure Databricks

正如我们之前可能经历过的那样，工作区的创建需要一些时间。如果部署失败，工作区将处于失败状态，我们需要删除它并创建一个新的。当此工作区被删除时，托管资源组和任何其他已部署资源也将被删除。

## 查看已部署资源

你可以在 Azure 门户中检查 Azure Databricks 工作区是否已成功部署，或使用以下 Azure CLI 脚本来列出资源：

```
echo "Enter your Azure Databricks workspace name:" &&
read databricksWorkspaceName &&
echo "Enter the resource group where the Azure Databricks workspace exists:" &&
read resourcegroupName &&
az databricks workspace show -g $resourcegroupName -n $databricksWorkspaceName
```

## 清理资源

如果你正在按照本教程进行操作并且不打算使用之前创建的资源，可以使用下一个 Azure CLI 脚本来删除它们：

```
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

让我们谈谈 Azure CLI。它是我们可用于与 Azure Databricks 中的资源交互的工具。在下一节中，我们将更详细地描述如何使用它来管理我们的工作区资产。

## 设置 Azure Databricks CLI

Azure Databricks 附带一个 CLI 工具，允许我们管理资源。它建立在 Azure Databricks API 之上，允许你访问工作区、作业、集群、库等。这是一个托管在 GitHub 上的开源项目。

