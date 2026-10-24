

2. 然后，选择使用 GitHub 账号登录，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_511_0.png)

图 16.22 – SonarCloud – 使用 GitHub 登录

现在，我们必须配置 SonarCloud，以便创建一个将包含我们 GitHub 项目分析结果的项目，该项目位于 `https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP16/appdemo`。为此，请执行以下步骤：

1. 使用 GitHub 账号连接到 SonarCloud 后，在主页上单击 **Analyze new project** 按钮：

![](img/d3344b39c888aab0b832625465f837c9_511_1.png)

图 16.23 – SonarCloud – 分析新项目

## 2. SonarCloud 提供几个步骤来选择要分析的目标 GitHub 仓库。首先，选择 **Import an organization from GitHub**。然后，选择 **Only select repositories** 选项并选择目标仓库：

![](img/d3344b39c888aab0b832625465f837c9_512_0.png)

图 16.24 – SonarCloud – 选择仓库

## 3. 接下来，标记仓库的复选框，并通过单击 **Set Up** 按钮将其与您的组织关联：

### 分析项目 - 选择仓库

![](img/d3344b39c888aab0b832625465f837c9_512_1.png)

图 16.25 – SonarCloud – 设置仓库

## 4. 将显示项目分析仪表板，并指示代码尚未分析。

至此，我们已成功配置 SonarCloud 项目。现在，让我们通过执行以下步骤以最基本的方式分析项目：

1. 在 SonarCloud 项目中，在 **Configure** 下，选择推荐的选项，即 **With GitHub Actions**（如我们在上一节 _Getting started with GitHub Actions_ 中所学）：

图 16.26 – SonarCloud – 使用 GitHub Actions 模板

2. 然后，按照 SonarCloud 提供的指示，在 GitHub 仓库中创建 GitHub 密钥（带有 `SONAR_TOKEN` 密钥）并添加一个包含一些 Sonar 配置的新文件：

图 16.27 – SonarCloud GitHub Actions 说明

3. 然后，提交这个新的 GitHub Actions 工作流文件。这将自动触发该项目的 SonarCloud 分析。

4. 几分钟后，您将看到两次执行的结果。首先，您将看到 GitHub Actions 工作流已成功：

![](img/d3344b39c888aab0b832625465f837c9_514_0.png)

图 16.28 - GitHub Actions Sonar 分析

然后，您将在 SonarCloud 仪表板中看到分析结果，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_514_1.png)

图 16.29 - SonarCloud 分析结果

因此，每当对该仓库进行新的代码提交时，无论是直接提交还是通过合并拉取请求，都将触发代码分析，并且 SonarCloud 仪表板将被更新。

很明显，我们的最终目标是将 SonarCloud 分析集成到 CI/CD 管道中，因此这里有一些资源可以帮助我们集成它：

- 如果您使用 Azure DevOps，这里有一个完整的教程，可以帮助您将 SonarCloud 集成到管道中：`https://docs.microsoft.com/en-us/labs/devops/sonarcloudlab/`。

- 如果您使用 Travis CI（我们在本章中介绍过），请查看以下文档：https://docs.travis-ci.com/user/sonarcloud/。

在本节中，我们学习了如何配置 SonarCloud，这是一个分析静态代码的云平台。我们这样做是为了通过持续集成过程分析 GitHub 上开源项目的源代码。然后，我们查看了仪表板上的分析结果。

在下一节中，我们将探讨开源代码安全的另一个方面，即使用 WhiteSource Bolt 分析代码漏洞。

## 使用 WhiteSource Bolt 检测安全漏洞

由于公开可见性，开源项目或组件高度暴露于安全漏洞，因为更容易无意中将包含安全漏洞的组件（包或其依赖项之一）注入其中。

除了静态源代码分析之外，持续检查开源项目中引用或使用的包的安全性也非常重要。

有许多可用的工具，我们可以用来分析应用程序中引用的包的安全性，例如 SonaType AppScan (https://www.sonatype.com/appscan)、Snyk (https://snyk.io/) 和 WhiteSource Bolt (https://bolt.whitesourcesoftware.com/)。

> Note
> 有关开源漏洞扫描工具的更多信息，请查看以下文章，该文章列出了 13 个分析开源依赖项安全性的工具：https://techbeacon.com/app-dev-testing/13-tools-checking-security-risk-open-source-dependencies。

在所有这些工具中，我们将研究 **WhiteSource Bolt** (https://bolt.whitesourcesoftware.com/)，它提供免费计划，可以分析多种开发语言的包代码，并允许您直接与 GitHub 和 Azure DevOps 集成。

## Note

关于在 GitHub 中集成 White Source Bolt 的完整文档可在 https://whitesource.atlassian.net/wiki/spaces/WD/pages/556007950/WhiteSource+Bolt+for+GitHub 上查阅。

在我们的案例中，我们将直接在 GitHub 上使用它来分析一个应用程序的安全性，其源代码可在此处查看：https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP16/appdemo。

要执行此安全分析，我们必须在 GitHub 账号上安装和配置 WhiteSource Bolt 并触发代码分析。按照以下步骤了解如何操作：

1. 在 Web 浏览器中，访问 https://bolt.whitesourcesoftware.com/github/ 并单击 **GitHub APP** 按钮，在 GitHub 账号上安装 WhiteSource Bolt。

2. 您将被重定向到 WhiteSource Bolt 应用程序，该应用程序可在 GitHub Marketplace (https://github.com/marketplace/whitesource-bolt) 上找到。要使用免费计划安装它，请单击页面底部的 **Install it for free** 按钮，如下图所示：

## 定价和设置

![](img/d3344b39c888aab0b832625465f837c9_516_0.png)

图 16.30 – 安装 WhiteSource Bolt

3. 单击 **Complete order and begin installation** 按钮确认您以 $0 购买该应用程序。然后，在下一页上，确认 WhiteSource Bolt 已安装在您的 GitHub 账号上。

安装完成后，您将被重定向到 WhiteSource Bolt 账号创建页面，您必须填写全名和国家/地区，然后验证表单。

4. 现在，通过转到包含要扫描代码的仓库的 **Settings** 选项卡并勾选 **Issues** 复选框，在 GitHub 中激活 **Issues** 功能，如下图所示：

## 功能

图 16.31 – GitHub – 激活问题

5. 要在该仓库上配置 WhiteSource 分析，您必须验证并合并安装 WhiteSource Bolt 时自动创建的新拉取请求。

图 16.32 – WhiteSource Bolt 问题配置

此拉取请求添加了一个 `.whitesource` 文件，该文件用于仓库根目录下的配置。

6. 最后，通过在 GitHub 仓库中提交此应用程序的代码来执行代码分析。

7. 几分钟后，您将在仓库的 **Issues** 选项卡中看到一系列安全问题。

![](img/d3344b39c888aab0b832625465f837c9_518_0.png)

8. 要了解安全问题的完整详细信息，只需单击所需的问题，阅读它，并考虑问题描述中提供的信息。

![](img/d3344b39c888aab0b832625465f837c9_518_1.png)

我们将不得不修复所有这些问题，并重新提交代码以触发新的 WhiteSource Bolt 扫描，确保我们为将要使用该应用程序的人提供一个安全的应用程序。

在本节中，我们学习了如何使用 WhiteSource Bolt 分析开源项目的代码。我们安装了它并触发了代码分析，该分析揭示了我们演示应用程序中的安全问题。

## 摘要

本章专门介绍可以应用于开源项目（尤其是 GitHub 上的项目）的 DevOps 最佳实践。在本章中，我们学习了如何协作开源代码，从使用 GitHub 创建仓库和分叉开始。然后，我们学习了如何使用拉取请求以及如何在 GitHub Releases 中共享二进制文件。

之后，我们使用与 GitHub 完全集成的 GitHub Actions 实现了持续集成流程。

最后，我们学习了如何使用 SonarCloud 分析开源代码的静态代码，以及使用 WhiteSource Bolt 分析安全漏洞。

在下一章中，我们将总结本书中讨论的每个 DevOps 最佳实践。

## 问题

1. 在 GitHub 中，我可以修改另一个用户仓库的代码吗？

2. 在 GitHub 中，哪个元素允许我们在两个仓库之间合并代码更改？

3. 哪个元素允许我们简单地显示开源项目中代码更改的历史记录？

4. 在 GitHub 中，本章提到的哪个功能允许我们共享二进制文件？

5. 本章中我们研究的哪两个工具允许我们分析开源项目的源代码？

6. WhiteSource Bolt 检测到的安全问题在哪个 GitHub 选项卡中列出？

