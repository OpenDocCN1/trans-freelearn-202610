## Azure 上的容器与持续部署

容器是开发人员构建、测试、部署、更新和重新部署应用程序到各种环境的一种简单方式，这些环境涵盖开发人员的本地计算机、本地数据中心，甚至跨多个云厂商的云端。

Azure 提供多种服务来部署容器。容器使持续构建和部署应用变得容易。通过使用 Azure Kubernetes Service 在 Azure 中进行 Kubernetes 编排，我们的容器集群是可复制的，并且易于管理。

在本章中，我们将介绍 Azure 上容器的**持续集成和持续交付（CI/CD）**。我们将设置持续部署以生成我们的容器镜像和编排。

在本章中，我们将涵盖以下主要主题：

- 使用 Azure DevOps 和 Azure Container Registry 为 Docker 设置持续部署
- 使用 Azure DevOps 对 Windows 容器进行持续部署
- 将 Docker Hub 集成到 CI/CD 管道中

## 使用 Azure DevOps 和 Azure Container Registry 为 Docker 设置持续部署

在本节中，我们将为构建容器化应用程序设置 CI 管道。我们将使用 Azure DevOps 构建 Docker 镜像并将其发布到 Azure Container Registry，以提高部署的可靠性和速度。





我们将在 GitHub 上使用我们的解决方案。你可以从此仓库 fork 或 clone：https://github.com/didoureba/healthcaresolution.git。我们将部署之前的医疗保健解决方案，其中包括一个 ASP.NET API 和两个使用 ASP.NET MVC 的应用程序，并按照下图中所示的步骤将应用程序部署到 Azure Container Registry。我们将使用 Azure DevOps 来确保 CI/CD：

![](img/66aceec30d3e7dead2f71bb68a887688_260_0.png)

请注意，我们可以使用 Azure Repos 代替 GitHub 仓库。Container Registry 资源已经创建。

## 创建管道

在本节中，我们将创建一个管道，因为我们需要一个 Azure DevOps 组织和一个项目已经创建。为此，请按照以下步骤操作：

- 1. 登录到你的 Azure DevOps 组织并导航到你的项目：**healthcaresolution**。
- 2. 选择 **Pipelines** 然后 **Pipelines**，选择 **Create Pipeline**，然后选择 **New pipeline**，它将显示在底部，以创建新的管道，如下图所示：
- 4. 添加 GitHub 凭据后，从列表中选择你的仓库。
- 5. 在 Configure 选项卡中，选择 Docker: Build and push an image to Azure Container Registry 任务。
- 6. 选择订阅，然后点击 Continue。
- 7. 你将被要求连接到你的 Azure 账户。身份验证后，选择你的容器注册表。输入镜像名称，然后选择 Validate and configure，如下图所示：

![](img/66aceec30d3e7dead2f71bb68a887688_262_0.png)

图 11.4 – 配置构建和部署镜像到 Azure Container Registry

当 Azure Pipelines 构建你的管道时，它将创建到 Docker Registry 服务的连接，以便你的管道能够将镜像推送到 Azure Container Registry。最终它会生成一个名为 azure-pipelines.yml 的 YAML 文件来定义你的管道。

- 8. 查看生成的 YAML 管道并选择 Save and run：

![](img/66aceec30d3e7dead2f71bb68a887688_263_0.png)

图 11.5 – azure-pipelines.yml 文件

在我们的 YAML 文件中，我们有以下内容：

- - 变量，包括 Container Registry 连接，例如镜像仓库、容器注册表和标签
- - VM 镜像名称，即操作系统环境
- - 阶段，包括构建作业和不同的设置

在下图中，我们可以看到 azure-pipeline.yml 文件和阶段的不同设置：

![](img/66aceec30d3e7dead2f71bb68a887688_263_1.png)

图 11.6 – YAML 文件中阶段的设置

在下图中，我们展示了如何使用 .NET Framework 构建和推送应用程序到 Azure Container Registry 和 App Service：

![](img/66aceec30d3e7dead2f71bb68a887688_265_0.png)

图 11.8 – 使用 .NET Framework 构建并推送应用程序到 Azure Container Registry 和 App Service

## 部署到 Azure Container Registry

我们可以按照之前介绍的相同步骤构建并推送应用程序到 Azure Container Registry。

我们可以使用另一种方法通过以下步骤在 Azure DevOps 中的项目与 Azure Container Registry 之间创建服务连接：

- 1. 在 Azure DevOps 中打开你的项目，并在页面左侧底部选择 **project settings**。在 **Pipelines** 下，选择 **New Service connection** 以使用服务主体定义并保护 Azure 订阅与 Azure DevOps 项目之间的连接。点击 **Create service connection** 按钮：

图 11.9 – 在 Azure DevOps 与 Azure 服务之间创建服务连接

2. 将显示一个新对话框页面，要求我们从列表中选择一个服务连接以将 Azure 账户链接到 Azure DevOps 账户。选择 Azure Resource Manager 并点击 Next 按钮继续配置：

![](img/66aceec30d3e7dead2f71bb68a887688_267_0.png)

图 11.10 – 配置新的服务连接

- 3. 在下一个对话框页面中，选择 **Service principal (automatic)** 并点击 **Next**。选择订阅和资源组，然后输入服务连接。选择 **Save** 完成配置。

> **重要说明**
> 
> 如果我们只需要将 Azure Container Registry 与 DevOps 账户连接，我们可以选择 **New Service connection** 添加新的连接链接。然后，选择 **Docker Registry** 和 **Azure Container Registry**。填写所有参数并选择 **Save** 以确认连接。

5. 变量保存后，我们将更新 YAML 文件中的 vmImageName。将默认值 'ubuntu-latest' 替换为 'windows-latest'。我们还将在 containerRegistry 之后添加 buildContext，如以下屏幕截图所示，以确保所有必需的应用程序文件都被复制到镜像文件系统中。如果我们不添加这一行，我们将收到错误，提示在 Dockerfile 构建中找不到内部路径：

![](img/66aceec30d3e7dead2f71bb68a887688_269_0.png)

图 11.12 – Container Registry 的管道 YAML

6. 选择 Save and run 开始你的管道构建。

7. 一旦镜像推送到注册表，将你的容器推送到 App Service。你的 App Service 实例应该已创建。

在下一节中，我们将通过创建新的发布管道将应用程序部署到 Azure App Service。

## 部署到 Azure App Service

要将我们的应用程序部署到 Azure App Service，我们将创建一个新的发布管道。

要创建新的发布管道，请按照以下步骤操作：

- 1. 从仪表板选择 Pipelines 然后 Releases。
- 2. 选择 New Pipeline，然后选择 Empty job，如下图所示：
- 4. 选择订阅和应用程序名称并指定完全限定的容器镜像名称。如果你有多个容器镜像，在 Image name 输入中提供它们全部的名称。如果你使用 docker-compose 文件，在 Configuration File 输入中添加路径并选择 Save：

![](img/66aceec30d3e7dead2f71bb68a887688_271_0.png)

图 11.15 – 配置 Azure Web App on Container Deploy

## 重要说明

你需要为你的 Container Registry 启用管理员用户。选择 Container Registry 的 Settings 下的 Access keys，然后为 Admin user 选择 Enabled。此选项允许使用注册表名称作为用户名和管理员用户访问密钥作为密码，以便 Docker 登录到你的 Container Registry。

![](img/66aceec30d3e7dead2f71bb68a887688_272_0.png)

图 11.16 – 在 Azure Container Registry 中启用 Admin user

5. 在 Pipeline 选项卡上，选择 Add an artifact：

图 11.17 – 添加构件

6. 在 Add an artifact 中，选择 Build 作为 Source type，然后选择项目、源、默认版本并选择 Add：

图 11.18 – 构建源类型

7. 启用 Continuous deployment trigger 并保存所有修改：

图 11.19 – 启用持续部署触发器

8. 选择 **Create release**，添加阶段并为此版本的构件源更新版本。然后，点击 **Create** 按钮：

![](img/66aceec30d3e7dead2f71bb68a887688_274_0.png)

图 11.20 – 配置发布

我们准备再次将本地代码推送到我们的仓库以启动管道。我们可以在更新 App Service 的最新版本之前添加审批请求。

我们将在下一节中了解如何将 Docker Hub 与 CI/CD 管道集成。

## 将 Docker Hub 与 CI/CD 管道集成

在本节中，我们将为构建容器化应用程序设置 CI 管道。我们将构建并发布 Docker 镜像到 Docker Hub：

![](img/66aceec30d3e7dead2f71bb68a887688_274_1.png)

图 11.21 – 场景：使用 Azure DevOps pipelines 构建并推送 Docker 镜像到 Docker Hub





我们将通过向 Azure DevOps 项目添加服务连接开始，并遵循上一节中介绍的相同步骤。

在服务连接中，添加一个新的服务连接以将 Azure DevOps 与 Docker Hub 账户关联。选择 Docker Registry，并在注册表类型中勾选 Docker Hub。完成不同的设置，选择验证并保存：

![](img/66aceec30d3e7dead2f71bb68a887688_275_0.png)

图 11.22 – 将 Azure DevOps 与 Docker Hub 注册表关联

我们将按照之前相同的步骤创建构建管道。请遵循以下步骤：

+   1. 要继续配置你的管道，选择你的存储库并在配置选项卡中选择 Docker- 构建 Docker 镜像。然后，选择验证并配置。
+   2. 添加以下变量：
+      - imageRepository: your-image-name
+      - containerRegistry: 'your-docker-hub-registry-name'
+      - applicationName: your-app-name

图 11.23 – 构建到 Docker Hub 的管道

3. 向你的管道添加一个任务；选择 Docker，然后在容器注册表下选择连接服务，添加存储库的名称，并选择添加以保存任务：

图 11.24 – 使用 Docker Hub 连接服务更新 YAML 文件

4. 在更新你的 YAML 文件后选择保存并运行以启动你的构建管道。你可以在 Docker Hub 中验证你的存储库。

## 评估

## 第 1 章

+   1. 事件驱动架构的三个关键组件是什么？
+   事件驱动架构的关键组件如下：
+   - **事件生产者**：这些生成事件流
+   - **事件路由器**：这些在生产者和消费者之间管理事件传递
+   - **事件消费者**：这些监听事件
+   有关更多详细信息，请参阅 *理解事件驱动架构部分。*

+   2. Azure 的不同数据库服务有哪些？
+   Azure 的不同数据库服务如下：Azure SQL Database、Azure SQL Managed Instance、SQL Server on Azure Virtual Machines、Azure Database for PostgreSQL、Azure Database for MySQL、Azure Database for MariaDB、Azure Cache for Redis、Azure Database Migration Service 和 Azure Managed Instance for Apache Cassandra。
+   有关更多详细信息，请参阅 *探索云数据库部分。*

## 第 3 章

+   - 使用 .NET Core 应用程序从 Service Bus 队列读取消息需要添加哪些包？
+   `Azure.Messaging.ServiceBus.` 有关更多详细信息，请参阅 *练习 3 – 使用 .NET Core 应用程序将消息发布到 Service Bus 队列。*

## 第 4 章

+   1. 我们是否需要为 Durable Functions 添加更多包？
+   我们需要添加 `Microsoft.Azure.WebJobs.Extensions.DurableTask`

+   2. Azure 中的 durable functions 是什么？
+   Durable Functions 是 Azure Functions 的扩展，允许你在无服务器计算环境中编写有状态函数。有关更多详细信息，请参阅 *开发 durable functions 部分。*
