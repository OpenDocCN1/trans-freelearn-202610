

启用工作区访问控制后，存在以下权限：

- 只有管理员可以在 **Workspace** 文件夹中创建项目，但用户可以管理现有项目。
- 应用于文件夹的权限将应用于其包含的项目。
- 用户对其主目录仍保留 **Manage** 权限。

了解这些权限有助于我们提前了解这些策略的可能变更如何影响用户与组织数据的交互方式。

## Notebook 权限

Notebook 具有与文件夹相同的五个权限级别：**No Permissions**、**Read**、**Run**、**Edit** 和 **Manage**。

## 配置 notebook 和文件夹权限

用户可以通过点击 notebook 上下文栏中的 **Permissions** 按钮来配置 notebook 权限。选择文件夹，然后从下拉菜单中点击 **Permissions**：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_52_0.png)

从那里，您可以授予用户或组的权限以及编辑现有权限：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_52_1.png)

通过从下拉菜单中选择其中一个选项，可以轻松地以这种方式对 notebook 应用访问控制。

## MLflow Model 权限

您可以为在 **MLflow Model Registry** 中注册的 **MLflow Models** 分配六个权限级别：**No Permissions**、**Read**、**Edit**、**Manage Staging Versions**、**Manage Production Versions** 和 **Manage**。

## 默认 MLflow Model 权限

除了当前的工作区访问控制外，还维护以下权限：

- 注册表中的模型可以由任何人创建。
- 管理员可以管理注册表中的任何模型。

当没有工作区访问控制时，用户可以管理注册表中的任何模型。

启用工作区访问控制后，存在以下权限：

- 用户只能管理他们创建的模型。
- 只有管理员才能管理由其他用户创建的模型。

这些选项适用于在 Azure Databricks 中创建的 MLflow Models。

## 配置 MLflow Model 权限

需要注意的一点是，只有管理员属于具有 **Manage** 权限的 admins 组，而其余用户属于 **all users** 组。

可以通过点击侧边栏中模型的图标、选择模型名称、点击模型名称右侧的下拉图标，最后选择 **Permissions** 来修改 MLflow Model 权限。这将显示一个对话框，我们可以从中选择特定用户或组并添加特定权限：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_53_0.png)

图 1.42 – MLflow 权限

您可以通过从 **Permission** 下拉菜单中选择新权限来更新用户或组的权限：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_54_0.png)

图 1.43 – MLflow 访问管理

通过选择其中一个选项，我们可以控制 MLflow 实验如何与我们的数据交互，以及哪些用户可以创建与其一起工作的模型。

## 总结

在本章中，我们尝试涵盖了 Azure Databricks 的所有主要方面。我们发现的一些内容包括如何创建 notebook 来执行代码，如何导入数据以使用，如何创建和管理集群等等。这很重要，因为在组织内部在 Azure Databricks 中创建 ETL 和 ML 实验时，除了如何在 notebook 中编写 ETL 代码外，我们还需要知道如何管理所需的数据和计算资源，如何共享资产，以及如何管理其中每一个的权限。

在下一章中，我们将应用这些知识，更详细地探讨如何在 Azure Databricks 中创建和管理处理数据所需的资源，并了解有关自定义 VNet 的更多信息以及我们通过 Azure Databricks UI 或 CLI 工具与之交互的不同替代方案。

## 2
创建 Azure Databricks 工作区

在本章中，我们将应用我们在 *Chapter 1, Introduction to Azure Databricks* 中探索的所有概念。我们将使用 UI 创建第一个 Azure Databricks 工作区，然后通过 Azure CLI 探索不同的资源管理可能性，如何使用 ARM 模板部署这些资源，以及我们如何使用 VNet 注入将 Azure Databricks 集成到我们的虚拟网络中。

在本章中，我们将讨论以下主题：

- 使用 Azure 门户 UI
- 检查 Azure Databricks 身份验证
- 在 Azure Databricks 中使用 VNets
- Azure Resource Manager 模板
- 设置 Azure Databricks CLI

我们将首先从 Azure 门户 UI 创建工作区。

## 技术要求

本章最重要的先决条件是已经拥有资金和权限的 Azure 订阅。请记住，这是一项按使用付费的服务，但无论如何，您可以创建一个免费试用订阅；有关此选项的更多信息，请查看 Azure 门户（https://azure.microsoft.com/en-us/free/）。

## 使用 Azure 门户 UI

让我们开始通过 Azure 门户 UI 设置新的 Databricks 工作区：

- 1. 登录您的订阅的 Azure 门户并导航到 Azure 服务功能区。
- 2. 点击 Azure Databricks：
- 3. 这将带您进入 Azure Databricks 默认文件夹，您将在其中看到列出所有资源。点击 Create new resource 以创建 Azure Databricks 工作区环境：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_56_0.png)

图 2.1 – 创建 Azure Databricks 服务

![](img/7e45bd08916c2a17d4e75cbb898f41e8_56_1.png)

图 2.2 – 您的 Azure Databricks 部署资源

4. 一旦您点击 Create azure databricks service，您将需要填写一些关于您正在创建的工作区的详细信息。设置看起来会像这样：

- Workspace name
- Subscription
- Resource group
- Location
- Pricing Tier
- Deploy Azure Databricks workspace in your Virtual Network (Preview)

工作区的名称可以随意选择，但最好选择一个简单且能体现其用途的名称。例如，这些环境可以是验收或生产环境，每一个都是其自己的订阅类型。资源组可以在当时创建，并且必须逻辑上涵盖具有相同目的的所有资源。

部署资源的地理位置也很重要，因为它必须靠近服务点或必须与其一起工作的其他部署资源。

> **Note**
> 关于定价层：有三个选项，分别是 Standard、Premium 和 Trial。其中一个最大的区别是 Premium 允许您应用访问控制。这使其成为企业环境中的常见选择。

其中一个选项是您可以选择在自己的专用虚拟网络中部署。在此步骤中，我们将选择 No，但我们将在本章后面讨论 VNet 以及如何将其作为一种良好的安全实践，将所有 Azure 资源置于相同的网络安全策略下。

5. 最后，您可以创建资源，工作区将被自动配置和部署：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_58_0.png)

图 2.3 – 创建 Azure Databricks 工作区

现在我们已成功创建工作区，我们可以通过 Azure CLI 的 Workspace UI 访问它。

## 访问 Workspace UI

一旦您的 Azure Databricks 资源成功部署，您就可以访问工作区。您可以通过在 Azure 门户中点击资源中央的 Launch Workspace 按钮直接访问它，或者您可以通过使用资源中心左侧显示的 URL 访问它。此 URL 将带您进入您将使用 Azure AD 登录访问的工作区：

![](img/7e45bd08916c2a17d4e75cbb898f41e8_59_0.png)

图 2.4 – Azure Databricks 服务主面板

一旦您能够访问工作区，您将看到不同的选项来访问您的资产。您可以使用左侧选项功能区，或中心选项来打开 notebook、创建新集群、新作业等等。主左侧功能区中可访问的整个选项列表如下：

- Azure Databricks
- Home
- Workspace
- Recent assets
- Data
- Clusters

