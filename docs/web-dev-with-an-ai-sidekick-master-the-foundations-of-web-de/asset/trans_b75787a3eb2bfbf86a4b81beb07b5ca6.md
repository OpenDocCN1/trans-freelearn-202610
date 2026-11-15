
# GitHub Copilot 设置与配置

在本节结束时，您将在 VS Code 中运行 GitHub Copilot，用于内联补全和聊天。您还将配置第二条路径，以便 Copilot Chat 可以使用：

* 内置 Copilot 模型（如果您符合条件，包括免费计划）
* 您自己的模型提供商，包括本地 Ollama 模型，以避免每次请求的 API 成本

VS Code 的 `BYOK` 模型支持是 Copilot Chat 能够由第三方提供商（包括用于本地模型的 Ollama）提供支持的部分方式。

## 先决条件

* 已安装 VS Code
* 一个可从 VS Code 登录的 GitHub 账户。您可以通过以下链接注册：[https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github](https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github)
* 选择以下选项之一：
    * GitHub Copilot 免费计划（无需 API 密钥）
    * OpenAI API 密钥或另一个受支持的提供商密钥用于 BYOK
    * 本地模型运行时，如 Ollama，用于本地推理

`警告！` GitHub Copilot 内联建议（您按 `Tab` 接受的自动补全）与 `Copilot Chat` 并非同一事物。VS Code 的 BYOK 模型系统主要是关于使用不同的模型提供商来支持聊天和编辑体验。某些 BYOK 功能可能不以相同方式适用于补全。请将本地模型支持视为以聊天为主（和以编辑为主）的路径，而非每个 Copilot 功能的保证替代品。

## 在 VS Code 中安装并登录 Copilot

1. 打开 VS Code，通过点击状态栏中的 Copilot 图标触发 Copilot 设置。
2. 使用 GitHub 登录。按照登录提示操作。如果您的 GitHub 账户已有 Copilot 访问权限，VS Code 将使用它。如果您没有订阅，VS Code 可以将您注册到 Copilot 免费计划（有每月使用限制）。
3. 验证 Copilot 处于活动状态。打开任意代码文件，输入几个字符，然后暂停。如果 Copilot 处于活动状态，您应该会看到灰色的 `ghost text` 建议，可以接受（通常是 `Tab`）。
4. 打开 `Copilot Chat` 视图并确认其响应。在 VS Code 中，Copilot Chat 使用聊天输入区域中的模型选择器，如 `图 11.1` 所示：

![](Images/B34553_11_01.png)

图 11.1：在 GitHub Copilot Chat 中选择模型

1. 确认您的计划和限制（尤其是免费计划）。如果您使用 Copilot 免费版，请预期每月上限，如内联建议限制和较少的高级请求数量（由更新或更昂贵的模型使用）。有关当前限制，请参阅以下链接的 Copilot 计划：[https://github.com/features/copilot/plans](https://github.com/features/copilot/plans)

`提示`：请解释令牌限制和上下文窗口如何影响 AI 编码助手在处理大型 Django 代码库时的表现。

# GitHub Copilot 提示

以下是一些改善您使用 GitHub Copilot 体验的提示：

* **在聊天中有意使用模型选择器。** Copilot 支持多种模型，您选择的模型会影响延迟、准确性和成本乘数（高级请求）。对于快速问答或轻量级编辑，较快的模型通常足够。对于复杂的重构或推理密集型调试，在需要时切换到更强的模型。
* **了解什么会消耗高级请求。** 在包含高级请求的计划中，最新的前沿模型通常每次请求成本更高（Copilot 将其记录为乘数）。如果您使用免费计划，请保守使用：用较便宜的模型进行探索，然后在最终的 `do it right`（做到位）阶段花费高级调用。
* **请求结构化输出以减少令牌浪费。** 当您对其进行约束时，Copilot Chat 会变得更好（且更便宜）。示例：
    * “列出三种方法并推荐一种。”
    * “为这两个文件给出补丁风格的差异。”
    * “仅更改此失败测试涉及的方法。”
* **关注 VS Code 中的隐私设置。** 如果您编写专有代码，请查看 VS Code 中的 Copilot 设置，并收紧任何您不满意的内容。

# 确保您有可用的 OpenAI API 密钥

本节适用于希望使用来自 OpenAI 或其他受支持提供商的自己的提供商密钥来在 VS Code 中支持聊天和编码体验的读者：

1. 创建并存储您的 OpenAI API 密钥。在您的 OpenAI 账户中创建 API 密钥并将其存储在密码管理器中。不要将其硬编码到源代码控制中。
2. 通过运行管理模型的命令打开 VS Code 的语言模型管理 UI。例如，在命令面板中，选择菜单项 `Chat: Manage Language Models`。
3. 添加您的提供商密钥，如 OpenAI。选择 `Bring your own language model key`。选择 `OpenAI` 或另一个提供商，然后粘贴密钥。

`警告！` VS Code 的 BYOK 不适用于 Copilot Business 或 Copilot Enterprise 账户（它适用于免费/专业版/专业版+ 等个人计划）。

1. 在 Copilot Chat 中选择提供商支持的模型。在 Copilot Chat 中，使用模型选择器选择您刚添加的模型。
2. 进行快速冒烟测试：

```
Summarize this file and suggest two refactoring steps.
```

现在您已将 VS Code 和 GitHub Copilot 配置为您的编码助手，让我们看看 Docker 如何有助于实现一致的开发设置。

# Docker 用于一致的服务器设置

Docker 是一种将应用程序及其所需的一切（包括运行时、操作系统库、依赖项和配置默认值）打包成一个称为 `image`（镜像）的可重复单元的方式。然后您将该镜像作为 `container`（容器）运行。

对 TallyApp 的实际好处是一致性：Windows、macOS 和 Linux 上的开发人员可以运行相同的 Django 技术栈，具有相同的版本和相同的行为，而无需花费数小时解决本地设置差异。

## 重要概念

以下是您需要了解的关于 Docker 的重要概念：

* **Images（镜像）**：一个构建好的、不可变的模板。您通常会为您的 Django 应用构建一个。预构建的镜像用于常见服务，如数据库引擎和管理软件。
* **Containers（容器）**：镜像的一个运行实例。容器可以轻松启动、停止和替换。
* **Dockerfile**：一个描述如何构建镜像的文本文件。它包含逐层指令，用于选择基础镜像、安装依赖项、复制代码以及如何启动进程。
* **Docker Compose**：一种通过单个命令定义和运行多容器应用程序的方式。例如，web 容器可能是您的 Django 项目，`db` 容器可能托管 PostgreSQL 数据库引擎，依此类推。
* **Volumes（卷）**：由 Docker 管理的持久化存储。这对数据库最为重要。如果您不为 PostgreSQL 使用卷，当数据库容器被替换时，您的数据将会消失。
* **Networks（网络）**：Compose 自动创建一个私有网络，以便容器可以通过服务名称相互通信。例如，Django 容器可以通过主机名 `db` 访问数据库。
* **Ports（端口）**：您暴露给宿主机的入口。您可能会将容器端口 8000 映射到宿主机端口 8000，以便在浏览器中打开站点。

`提示`：Docker 卷和绑定挂载有什么区别？请解释为什么绑定挂载适合开发但在生产环境中有风险。

## 下载和安装 Docker

即使您最初使用 SQLite 部署 TallyApp，学习使用 PostgreSQL 的 Docker 仍然很有用，因为它演示了一个现实的多服务设置。当您以后想要减少 SQLite 的并发限制时，它还为您提供了一条清晰的升级路径。

