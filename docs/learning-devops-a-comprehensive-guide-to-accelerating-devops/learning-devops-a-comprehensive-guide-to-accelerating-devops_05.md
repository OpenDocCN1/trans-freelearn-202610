## 前言

今天，随着技术的发展和竞争的日益加剧，公司面临着设计和更快速交付产品的真正挑战——同时保持用户满意度。

应对这一挑战的解决方案之一是向公司引入一种不同团队（如开发和运维、测试人员以及安全团队）之间协作的文化。这种文化已经被证明并被称为 DevOps 文化，它可以确保团队和某些实践通过这种协作缩短公司的上市时间——通过更短的应用程序部署周期，并为公司的产品和应用程序带来真正的价值。

此外，随着公司向云端的重大转变，应用程序基础设施正在演变，DevOps 文化将使应用程序具有更好的可扩展性和性能，从而为公司带来财务收益。

如果您想了解更多关于 DevOps 文化的信息，并将其实践应用于您的项目中，本书将通过不同的工具和实验室向您介绍 DevOps 实践的基础知识。

在本书中，我们将讨论 DevOps 文化和实践的基础知识，然后我们将研究用于实施 DevOps 实践的不同实验室，例如 IaC、使用 Git 和 CI/CD 管道、测试自动化、代码分析以及 DevSecOps，以及向您的流程添加安全性。

本书的一部分也专门用于应用程序的容器化，涵盖 Docker 的简单使用以及在 Kubernetes 中管理容器。它包括部署期间减少停机时间的主题以及开源项目上的 DevOps 实践。

本书以一章专门介绍一些可以在项目生命周期中实施的良好 DevOps 实践而结束。

在第二版中，所有工具都已升级，我们将了解 HashiCorp 的 Vagrant 以及更多关于 Kubernetes 部署的内容。





本书旨在指导你使用不同的工具逐步实施 DevOps 实践，这些工具大多是开源的，或者是市场上的领先工具。

在撰写本书时，我的目标是与你分享我的日常经验；我希望它对你有用，并能应用到你的项目中。

## 本书适用人群

本书适用于任何想要开始实施 DevOps 实践的人。不需要特定的开发或系统运维知识。

## 本书涵盖内容

第 1 章，DevOps 文化与基础设施即代码实践，解释了 DevOps 文化的目标，并详细介绍了本书中将涉及的不同 DevOps 实践——IaC 和 CI/CD 管道。

第 2 章，使用 Terraform 配置云基础设施，详细介绍了使用 IaC 和 Terraform 配置云基础设施，包括其安装、命令行、生命周期、配置 Azure 基础设施示例的实用用法，以及使用远程后端保护 Terraform 状态文件。

第 3 章，使用 Ansible 配置 IaaS 基础设施，涉及使用 Ansible 配置虚拟机，包括 Ansible 的安装、命令行、为清单和剧本设置角色，其在 Azure 中配置虚拟机的使用，使用 Ansible Vault 保护数据，以及使用动态清单。

第 4 章，使用 Packer 优化基础设施部署，介绍了使用 Packer 创建虚拟机镜像，包括其安装以及如何在 Azure 中创建镜像。

第 5 章，使用 Vagrant 编写开发环境，解释了如何使用 IaC 和 Vagrant 构建本地开发环境。

第 6 章，使用 Git 管理源代码，探讨了 Git 的使用，包括其安装、主要命令行、工作流、分支系统概述，以及 GitFlow 工作流示例。

第 7 章，持续集成与持续交付，展示了使用三种不同工具（Jenkins、GitLab CI 和 Azure Pipelines）创建端到端 CI/CD 管道。对于每种工具，我们将详细介绍其特性。

第 8 章，使用 CI/CD 管道部署基础设施即代码，讨论了使用 Azure Pipelines 中的 CI/CD 管道自动执行 Packer、Terraform 和 Ansible。

第 9 章，使用 Docker 容器化你的应用程序，介绍了 Docker 的使用，包括本地安装、Docker Hub 注册表概述、编写 Dockerfile，以及演示如何使用它。一个应用程序示例将被容器化，在本地执行，然后通过 CI/CD 管道部署到 Azure 容器实例。

第 10 章，使用 Kubernetes 有效管理容器，解释了 Kubernetes 的基本用法，包括本地安装和应用程序部署，然后是使用 Azure Kubernetes 服务管理的 Kubernetes 示例。

第 11 章，使用 Postman 测试 API，详细介绍了使用 Postman 测试 API 示例，包括本地使用，以及在 CI/CD 管道中使用 Newman 和 Azure Pipelines 进行自动化。

第 12 章，使用 SonarQube 进行静态代码分析，解释了使用 SonarQube 分析应用程序中的静态代码，包括其安装、使用 SonarLint 工具进行实时分析，以及将 SonarQube 集成到 Azure Pipelines 中的 CI 管道。

第 13 章，安全与性能测试，讨论了 Web 应用程序的安全性和性能，包括演示如何使用 ZAP 工具测试 OWASP 规则，以及使用 Postman 测试 API 性能。

第 14 章，DevOps 流程中的安全性与 DevSecOps，解释了如何通过使用 InSpec 测试基础设施合规性，以及使用 Vault 保护敏感数据，在 DevOps 流程中使用安全集成。

第 15 章，减少部署停机时间，介绍了使用 Terraform 减少部署停机时间、蓝绿部署的概念和模式，以及如何在 Azure 中应用它们。还重点介绍了在应用程序中使用功能标志。

第 16 章，开源项目的 DevOps，专门介绍开源。它详细介绍了开源项目的工具、流程和实践，包括 GitHub 协作、拉取请求、变更日志文件、GitHub 发行版中的二进制共享，以及 Travis CI 和 GitHub Actions 中 CI 管道的端到端示例。还讨论了使用 SonarCloud 和 WhiteSource Bolt 进行开源代码分析和安全。

第 17 章，DevOps 最佳实践，回顾了关于自动化、IaC、CI/CD 管道、测试、安全性、监控和项目管理的 DevOps 良好实践列表。

## 如何充分利用本书

理解本书不需要开发知识。你将看到的唯一语言是声明式语言，例如 JSON 或 YAML。除此之外，不需要特定的 IDE。如果你没有，可以使用免费且跨平台的 Visual Studio Code。它可在此处获取：`https://code.visualstudio.com/`。

本书中作为示例的云提供商是 Microsoft Azure。如果你没有订阅，可以在此处创建免费帐户：`https://azure.microsoft.com/en-us/free/`。

关于你需要的操作系统，没有实际的先决条件。我们将使用的大多数工具都是跨平台的，并与 Windows、Linux 和 macOS 兼容。它们的安装将在各自的章节中详细介绍。

| 本书涵盖的软件/硬件 | 操作系统要求 |
|---------------------------------------|-------------------------------|
| Terraform                             | Windows, macOS, 或 Linux      |
| Ansible                               | Linux 或 macOS                |
| Packer                                | Windows, macOS, 或 Linux      |
| Vagrant                               | Windows, macOS, 或 Linux      |
| Docker                                | Windows, macOS, 或 Linux      |
| Azure CLI                             | Windows, macOS, 或 Linux      |
| Helm                                  | Windows, macOS, 或 Linux      |
| Postman                               | Windows, macOS, 或 Linux      |
| SonarLint                             | Windows, macOS, 或 Linux      |
| Java Runtime                          | Windows, macOS, 或 Linux      |

如果你使用本书的数字版，我们建议你亲自输入代码或从本书的 GitHub 仓库访问代码（下一节中提供了链接）。这样做将帮助你避免与复制和粘贴代码相关的任何潜在错误。

## 下载示例代码文件

你可以从 GitHub 下载本书的示例代码文件：`https://github.com/PacktPublishing/Learning-DevOps-Second-Edition`。如果代码有更新，将在 GitHub 仓库中更新。

我们还在丰富的图书和视频目录中提供其他代码包，可在 https://github.com/PacktPublishing/ 查看。请查看！

## 代码演示

本书的代码演示视频可在 https://bit.ly/36xzV7u 查看。

## 下载彩色图片

我们还提供一个包含本书中使用的屏幕截图和图表彩色图片的 PDF 文件。你可以在此处下载：https://static.packt-cdn.com/downloads/9781801818964_ColorImages.pdf。

## 本书使用的惯例

本书中使用了多种文本惯例。

文本中的代码：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。示例：“导航到我们创建 Vagrantfile 文件的文件夹。”

代码块设置如下：

```
pool:
vmImage: ubuntu-latest
steps:
- task: DotNetCoreCLI@2
displayName: "Restore"
inputs:
```

当我们希望引起你对代码块特定部分的注意时，相关行或项目将以粗体显示：

```
[ inputs:
command: 'test'
projects: '**/tests/*.csproj'
arguments: '--configuration Release'
- task: DotNetCoreCLI@2
```







任何命令行输入或输出均按以下方式编写：

```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl \
```

**粗体**：表示新术语、重要词汇或屏幕上显示的词汇。例如，菜单或对话框中的词汇以**粗体**显示。示例：“在选择**调整您的 PATH 环境**选项时，我们可以保留安装程序建议的默认选项。”

提示或重要说明
如下所示。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：如果您对本书的任何方面有疑问，请发送电子邮件至 customercare@packtpub.com，并在邮件主题中注明书名。

**勘误**：尽管我们已尽力确保内容的准确性，但错误难免会发生。如果您在本书中发现了错误，我们将不胜感激，如果您能将其报告给我们。请访问 www.packtpub.com/support/errata 并填写表格。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激，如果您能提供位置地址或网站名称。请通过 copyright@packt.com 与我们联系，并附上材料链接。

**如果您有兴趣成为作者**：如果您在某个主题上有专长，并有兴趣编写或参与编写书籍，请访问 authors.packtpub.com。

## 分享您的想法

读完《Learning DevOps - Second Edition》后，我们很乐意听取您的想法！请点击此处直接前往本书的 Amazon 评论页面并分享您的反馈。

您的评论对我们和技术社区非常重要，将帮助我们确保提供优质内容。
