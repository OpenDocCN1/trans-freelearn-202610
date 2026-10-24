

在 Azure Pipelines 中，可以选择经典编辑器模式，该模式允许我们通过图形界面配置管道，或者 YAML 管道模式，该模式涉及使用 **YAML Ain't Markup Language** (YAML) 文件来描述管道的配置。

在此实验中，我们将使用经典编辑器模式，以便可以可视化不同的选项和配置步骤。

3. 管道的第一个配置步骤是选择包含应用程序源代码的仓库。

今天，Azure Pipelines 支持多种 Git 系统，例如 Azure Repos、GitHub、Bitbucket 和 **Subversion** (SVN)。因此，我们将在包含导入源代码的仓库中选择 **Azure Repos Git**，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_237_0.png)

图 7.24 – Azure Pipelines：选择源代码仓库

4. Azure Pipelines 提议选择一个包含所有预配置构建步骤的构建模板；也可以从空模板开始。

由于我们的项目是 ASP.NET Core 应用程序，我们将选择 **ASP.NET Core** 模板，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_238_0.png)

图 7.25 – Azure Pipelines：选择模板

选择模板后，我们将到达构建定义的配置页面。

构建定义的配置包含四个主要部分，概述如下：

- Variables
- Steps
- Triggers
- Options

5. 我们配置 **Variables** 部分，该部分允许我们以键值形式填写变量列表，创建可在步骤中使用的值。

下图显示了我们构建定义的 **Variables** 选项卡：

![](img/d3344b39c888aab0b832625465f837c9_239_0.png)

当我们导航到 **Variables** 选项卡时，可以看到模板已预填的 `BuildConfiguration` 和 `BuildPlatform` 变量，以及 **+ Add** 按钮，该按钮允许我们在需要时添加其他变量。

> **Note**
> 变量文档可在此处查阅：https://docs.microsoft.com/en-us/azure/devops/pipelines/process/variables?view=azure-devops&tabs=classic%2Cbatch。

6. 我们配置 **Tasks** 选项卡，该选项卡包含构建中要执行的所有步骤的配置。

下图是该选项卡的截图：

![](img/d3344b39c888aab0b832625465f837c9_240_0.png)

图 7.27 – Azure Pipelines：任务列表

在上述截图中，第一部分是 **Pipeline**，它允许我们配置构建定义的名称以及要使用的代理。

确实，在 Azure DevOps 中，管道在安装在 VM 或容器上的代理上执行。

Azure DevOps 提供来自多个 **operating systems (OSes)** 的免费代理，称为托管代理，但也可以安装自己的代理，称为 **self-hosted** 代理。

> Note
> 要了解有关托管代理和自托管代理的更多信息，请参阅以下文档：https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/agents?view=azure-devops。

下图显示了 **Pipeline** 部分的配置：

![](img/d3344b39c888aab0b832625465f837c9_241_0.png)

图 7.28 – Azure Pipelines：代理池配置

7. 然后，我们有 **Get sources** 阶段，它包含我们在开始时进行的源代码配置；不过，可以在此处对其进行修改，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_241_1.png)

图 7.29 – Azure Pipelines：源代码配置

8. 我们有 **Agent job** 部分，它包含要在管道中执行的任务的有序列表。这些任务中的每一个都在右侧面板中配置。

我们可以单击 + 按钮添加任务，然后从 Azure Pipelines 目录中选择它们，如下所示：

![](img/d3344b39c888aab0b832625465f837c9_242_0.png)

图 7.30 – Azure Pipelines：添加任务

默认情况下，Azure Pipelines 包含非常丰富的任务目录；这些任务的列表可在此处查阅：https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/index?view=azure-devops。我们还可以安装在 Azure 市场中找到的任务：https://marketplace.visualstudio.com/search?target=AzureDevOps&category=Azure%20Pipelines&sortBy=Downloads。如果需要，也可以按照此处的文档创建适合您需求的任务：https://docs.microsoft.com/en-us/azure/devops/extend/get-started/node?view=azure-devops。

让我们定义 CI 管道中的五个任务，如下所示：

| Step/task | Description |
|---|---|
| Restore | Restores the packages referenced in the project |
| Build | Builds the project and generates binaries |
| Test | Runs unit tests |
| Publish | Creates a ZIP package that contains the binary files of the project |
| Publish build artifacts | Defines an artifact that is our ZIP package of the application, which we will publish in Azure DevOps, and which will be used in the deployment release, as seen in the previous section, using a package manager in the CI/CD process |

表 7.2 – Azure Pipelines 步骤列表

9. 我们 CI 管道的最后一个重要配置是在 **Triggers** 选项卡中配置构建触发器以启用 CI，如下图所示：

图 7.31 – Azure Pipelines：启用 CI

10. 我们的 CI 或构建管道配置已完成；我们通过单击 **Save & queue** 按钮来验证并首次测试其执行，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_244_0.png)

图 7.32 – Azure Pipelines：保存并排队管道

11. 构建执行结束时，我们有一些信息来帮助分析管道状态，如下所示：

- 下图显示执行日志。这显示了管道中定义的每个任务的执行详细信息：

![](img/d3344b39c888aab0b832625465f837c9_244_1.png)

图 7.33 – Azure Pipelines 运行结果

接下来显示单元测试的执行结果。下图显示了单元测试执行报告，其中包含一些重要指标，例如通过/失败的测试数量和测试执行时间：

![](img/d3344b39c888aab0b832625465f837c9_245_0.png)

图 7.34 – Azure Pipelines：测试执行摘要

下图显示已发布的构建产物。这提供了探索或下载管道中 **Publish Build Artifact** 任务定义的已发布构建产物的可能性。

![](img/d3344b39c888aab0b832625465f837c9_245_1.png)

图 7.35 – Azure Pipelines：浏览构建产物

现在我们知道我们的 CI 构建已配置并可运行，我们将为 CD 管道创建和配置部署发布。

## 创建 CD 管道 – 发布

在 Azure Pipelines 中，允许在不同阶段或环境中进行部署的元素称为 **release**。我们将创建一个发布定义，该定义将通过以下步骤将构建生成的构建产物部署到 Azure Web 应用：

1. 要创建发布定义，我们转到 **Releases** 菜单并单击 **New pipeline**，如下所示：

![](img/d3344b39c888aab0b832625465f837c9_246_0.png)

图 7.36 – Azure Pipelines：创建发布定义

2. 与构建一样，配置的第一步是选择一个已配置的模板。对于本实验，我们将选择 **Azure App Service deployment** 模板，如下图所示：

图 7.37 – Azure Pipelines：Azure 应用服务部署模板

3. 然后，在下一个窗口中，第一个阶段被命名，例如，CI 作为 CI 环境，如下图所示：

图 7.38 – Azure Pipelines：阶段名称

