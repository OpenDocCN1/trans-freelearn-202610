


因此，当代码被推送到仓库时，我们的 CI 流水线会被触发，现在让我们来看看如何查看其执行详情。

## 访问 CI 流水线执行详情

要访问已执行 CI 流水线的执行详情，请执行以下步骤：

1. 在 GitLab CI 菜单中，进入 **CI / CD | 流水线**，你将看到流水线执行列表，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_268_0.png)

图 7.64 – GitLab 流水线

2. 要显示流水线的详情，我们点击所需的流水线执行，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_268_1.png)

图 7.65 – GitLab 流水线执行

3. 我们可以看到执行状态，以及在流水线 YAML 文件中定义的两个阶段。要查看某个阶段的执行日志详情，我们点击该阶段，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_269_0.png)

图 7.66 – GitLab 执行日志详情

我们可以看到流水线 YAML 文件中编写的脚本的执行情况。

在本节中，我们了解了在 GitLab CI 中实现 CI 流水线的过程，包括初始化远程仓库、创建用于配置流水线的 YAML 文件以及执行流水线。

## 摘要

在本章中，我们探讨了 DevOps 中最重要的主题之一：CI/CD 流程。我们首先介绍了 CI 和 CD 的原则。然后，我们重点介绍了包管理器，涵盖了 NuGet、npm、Nexus 和 Azure Artifacts。

最后，我们了解了如何使用三种不同的工具（Jenkins、Azure Pipelines 和 GitLab CI）实现和执行端到端 CI/CD 流水线。对于每种工具，我们都介绍了应用程序源代码的归档，以及流水线的创建和执行。

阅读本章后，我们应该能够以源代码管理作为源来创建 CI 和 CD 流水线。此外，我们将能够选择和使用包管理器来集中和分发我们的包。

在下一章中，我们将学习如何使用 Azure DevOps 为 IAC 项目创建 CI/CD 流水线，目标是执行 Packer、Terraform 和 Ansible 代码。

## 问题

1. 实现 CI 流水线的先决条件是什么？
2. CI 流水线何时会被触发？
3. 包管理器的用途是什么？
4. NuGet 包管理器中存储哪些类型的包？
5. Azure Artifacts 集成到哪个平台中？
6. Jenkins 是云服务吗？
7. 在 Azure DevOps 中，允许管理 CI/CD 流水线的服务名称是什么？
8. GitLab 提供哪三种服务？
9. 在 GitLab CI 中，哪个元素允许你构建 CI 流水线？

## 延伸阅读

如果你想了解更多关于 CI/CD 流水线的信息，可以参考以下资源：

- Hands-On Continuous Integration and Delivery: https://www.packtpub.com/virtualization-and-cloud/hands-continuous-integration-and-delivery
- Continuous Integration, Delivery, and Deployment: https://www.packtpub.com/application-development/continuous-integration-delivery-and-deployment
- Mastering Jenkins: https://www.packtpub.com/application-development/mastering-jenkins
- Azure DevOps Server 2019 Cookbook: https://www.packtpub.com/networking-and-servers/azure-devops-server-2019-cookbook-second-edition
- Mastering GitLab 12: https://www.packtpub.com/cloud-networking/mastering-gitlab-12

# 8 使用 CI/CD 流水线部署基础设施即代码

在本书的第一部分，我们学习了大量关于 **基础设施即代码 (IaC)**、其优势以及 Terraform、Packer 和 Ansible 等工具及其命令行的知识。然后，在第二部分中，我们讨论了 Git，并专门用一章来介绍应用程序的持续集成和持续部署。

但是，我们也不应忽视将 CI/CD 作为 IaC 付诸实践，这将使我们能够编排和自动化整个基础设施配置过程。

正是在这本书的第二版中，我想添加这一章，以重复利用我们在前面章节中学到的所有内容，并展示如何使用 CI/CD 流水线实现 IaC，该流水线将由 Packer、Terraform 和 Ansible 组成。

将要使用的流水线工具是 Azure Pipelines，我们已经在 _第 7 章，持续集成和持续交付_ 中详细介绍过它。因此，我们将学习如何编写 Azure 流水线的 YAML 文件以使用 Packer 生成镜像。然后，我们将看到如何编写 YAML 流水线以使用 Terraform 配置 **虚拟机 (VM)**，并通过执行 Ansible 在该 VM 上安装 nginx 来完成。

本章涵盖以下主题：

- 在 Azure Pipelines 中运行 Packer
- 在 Azure Pipelines 中运行 Terraform 和 Ansible

## 技术要求

本章需要以下内容：

- 用于创建 Packer 镜像和配置 VM 的 Azure 订阅；你可以在此处创建免费帐户：https://azure.microsoft.com/en-us/free/search/。
- 用于创建 YAML 流水线定义的 Azure DevOps 帐户；你可以在此处创建免费帐户：https://azure.microsoft.com/en-us/pricing/details/devops/azure-devops-services/。

本章的源代码可在此处查看：https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP08。

查看以下视频以观看代码实际运行情况：https://bit.ly/3BDT9ne

## 在 Azure Pipelines 中运行 Packer

要启动我们的 IaC 流水线，我们将自动使用 Packer 创建 Azure VM 镜像。要执行此操作，我们将使用带有 YAML 格式流水线的 Azure Pipelines。该流水线的目标是自动执行 Packer 命令。

> **重要说明**
> 有关如何在 Azure DevOps 中创建 YAML 流水线的更多详细信息，请阅读 _第 7 章，持续集成和持续交付_。

我们将在本节的实验中使用的 Packer 模板是我们在 _第 4 章，使用 Packer 优化基础设施部署_ 中学习的代码。此源代码可在此处查看：https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/blob/main/CHAP08/packer/.pkr.hcl。

在开始使用 Packer 之前，我们需要先创建一个 Azure 资源组，用于存储流水线创建的 VM 镜像。在我们的实验中，我们将此 Azure 资源组命名为 rg_images。下图显示了 Azure 资源组：

![](img/d3344b39c888aab0b832625465f837c9_273_0.png)

图 8.1 – Packer 镜像的 Azure 资源组

现在，我们可以编写流水线代码，该代码将在 Packer 模板上执行 Packer 命令行。

为此，在与 Packer 模板相同的文件夹中，创建一个新的 YAML 文件来描述流水线的步骤，然后编写三个代码块，如下所示：

1. 第一步是执行 `packer init` 命令：

```yaml
- script: packer init $(Build.SourcesDirectory)/CHAP08/packer/.pkr.hcl
  displayName: Packer init
```

2. 第二步是通过运行 `packer validate` 命令来验证 Packer 模板：

```yaml
- script: packer validate $(Build.SourcesDirectory)/CHAP08/packer/.pkr.hcl
  displayName: Packer validate template
```

3. 最后一步是通过运行 `packer build` 命令使用 Packer 构建镜像：

```yaml
- script: packer build $(Build.SourcesDirectory)/CHAP08/packer/.pkr.hcl
  displayName: Packer build template
  env:
    PKR_VAR_clientid: $(PKR_VAR_clientid)
    PKR_VAR_clientsecret: $(PKR_VAR_clientsecret)
    PKR_VAR_subscriptionid: $(PKR_VAR_subscriptionid)
    PKR_VAR_tenantid: $(PKR_VAR_tenantid)
```


