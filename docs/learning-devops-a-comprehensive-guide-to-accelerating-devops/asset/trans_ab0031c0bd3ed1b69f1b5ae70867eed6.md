

## 技术要求

本章将以在 Azure 基础架构中使用 Packer 创建 VM 镜像为例，介绍如何在云基础架构中使用 Packer。因此，你需要一个 Azure 订阅，可以在此处免费获取：https://azure.microsoft.com/en-us/free/。

在“在 Packer 模板中使用 Ansible”部分，我们将学习如何编写使用 Ansible 的 Packer 模板，因此你需要在机器上安装 Ansible 并了解其工作原理，详细内容请参阅第 3 章《使用 Ansible 配置 IaaS 基础架构》。

本章的最后一部分将介绍将 Terraform 与 Packer 镜像结合使用的示例；对于其应用，需要安装 Terraform 并了解其操作，详细内容请参阅第 2 章《使用 Terraform 配置云基础架构》。

本章的完整源代码可在此处获取：https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/tree/main/CHAP04。

请观看以下视频，查看代码实际运行情况：https://bit.ly/35exxSo。

## Packer 概述

Packer 是 HashiCorp 开源工具套件的一部分，Packer 的官方页面在此：https://www.packer.io/。它是一个开源命令行工具，允许我们从 JSON 文件在多个平台上创建任何操作系统的自定义 VM 镜像（这些镜像也称为 **templates**）。

Packer 的操作非常简单；它基于不同云提供商提供的基础操作系统，通过执行 JSON 或 HCL 模板中描述的脚本来配置临时 VM。然后，从这个临时 VM 中，Packer 生成可用于配置 VM 的自定义镜像。

除了 VM 镜像，Packer 还提供其他类型的镜像，例如 Docker 镜像或 Vagrant 镜像。对 Packer 进行简要介绍后，让我们来看看不同的安装模式。

## 安装 Packer

与 Terraform 类似，Packer 是一个跨平台工具，可以安装在 Windows、Linux 或 macOS 上。Packer 的安装几乎与 Terraform 的安装相同（请参阅第 2 章《使用 Terraform 配置云基础架构》），可以通过两种方式完成：手动安装或通过脚本安装。

### 手动安装

要手动安装 Packer，请使用以下步骤：

- 1. 访问官方下载页面（`https://www.packer.io/downloads.html`）并下载与你的操作系统对应的软件包。
- 2. 下载后，解压缩并将二进制文件复制到执行目录中（例如，在 `c:\Packer` 内）。
- 3. 然后，必须将 `PATH` 环境变量设置为二进制目录的路径。

> **Tip**
> 有关如何在 Windows 上更新 `PATH` 环境变量的详细说明，请参阅此文章：`https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/`，对于 Linux，请参阅此文章：`https://www.techrepublic.com/article/how-to-add-directories-to-your-path-in-linux/`。

现在我们已经了解了如何手动安装 Packer，让我们来看看使用脚本安装它的可用选项。

### 通过脚本安装

也可以使用自动脚本安装 Packer，该脚本可以安装在远程服务器上并用于 CI/CD 过程。确实，Packer 可以在本地使用，正如我们将在本章中看到的那样，但它的真正目标是集成到 CI/CD 管道中。这个自动化 DevOps 管道将允许构建和发布统一的 VM 镜像，这些镜像将保证基于这些镜像的中间件和 VM 安全性的完整性。

让我们来看看针对不同操作系统（即 Linux、Windows 和 macOS）的这些脚本的结构。

## 在 Linux 上通过脚本安装 Packer

在 Linux 上安装 Packer 二进制文件，我们有两种解决方案：

- 第一个解决方案是使用以下脚本安装 Packer：

```
PACKER_VERSION="1.7.3" #Update with your desired version
curl -Os https://releases.hashicorp.com/packer/${PACKER_VERSION}/packer_${PACKER_VERSION}_linux_amd64.zip \
&& curl -Os https://releases.hashicorp.com/packer/${PACKER_VERSION}/packer_${PACKER_VERSION}_SHA256SUMS \
&& curl https://keybase.io/hashicorp/pgp_keys.asc | gpg --import \
&& curl -Os https://releases.hashicorp.com/packer/${PACKER_VERSION}/packer_${PACKER_VERSION}_SHA256SUMS.sig \
&& gpg --verify packer_${PACKER_VERSION}_SHA256SUMS.sig packer_${PACKER_VERSION}_SHA256SUMS \
&& shasum -a 256 -c packer_${PACKER_VERSION}_SHA256SUMS 2>&1 | grep "${PACKER_VERSION}_linux_amd64.zip:\sOK" \
&& unzip -o packer_${PACKER_VERSION}_linux_amd64.zip -d /usr/local/bin
```

```
Note
The code of this script is also available here: https://github.com/PacktPublishing/Learning-DevOps-Second-Edition/blob/main/CHAP04/install_packer.sh.
```

此脚本执行以下操作：

- 下载 Packer 版本 1.7.3 软件包并检查校验和
- 解压缩并将软件包复制到本地目录 `/usr/local/bin`（默认情况下，此文件夹位于 `PATH` 环境变量中）

以下是在 Linux 上安装 Packer 的脚本执行截图：

![](img/d3344b39c888aab0b832625465f837c9_130_0.png)

图 4.1 – 安装 Packer 脚本执行

此解决方案的好处是我们可以选择 Packer 安装文件夹，并且它适用于各种 Linux 发行版，因为它使用常用工具，即 curl 和 unzip。

- 在 Linux 上安装 Packer 的第二种解决方案是对 Ubuntu 发行版使用 apt 包管理器，使用以下脚本：

```
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl \
&& curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add - \
&& sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main" \
&& sudo apt-get update && sudo apt-get install packer
```

此脚本执行以下操作：

- 添加 apt HashiCorp 存储库
- 更新本地存储库
- 下载 Packer CLI

> Important Note
> 有关此脚本以及在其他发行版上安装 Packer 的更多详细信息，请阅读 https://learn.hashicorp.com/tutorials/packer/get-started-install-cli?in=packer/docker-get-started 的文档，并导航到 Linux 选项卡。

## 在 Windows 上通过脚本安装 Packer

在 Windows 上，我们可以使用 **Chocolatey**，它是一个软件包管理器。Chocolatey 是一个免费的公共包管理器，类似于 NuGet 或 npm，但专门用于软件。它广泛用于 Windows 服务器甚至本地机器上的软件自动化。Chocolatey 的官方网站在此：`https://chocolatey.org/`，其安装文档在此：`https://chocolatey.org/install`。

安装 Chocolatey 后，我们只需在 PowerShell 或 CMD 工具中运行以下命令：

```
`choco install packer -y`
```

以下是使用 Chocolatey 在 Windows 上安装 Packer 的截图：

![](img/d3344b39c888aab0b832625465f837c9_131_0.png)

图 4.2 – 使用 Chocolatey 安装 Packer

执行 `choco install packer -y` 将从 Chocolatey 安装最新版本的 Packer。

## 在 macOS 上通过脚本安装 Packer

在 macOS 上，我们可以使用 **Homebrew**（macOS 包管理器，`https://brew.sh/`），通过在终端中执行以下命令来安装 Packer：

```
`brew install packer`
```

## 将 Packer 与 Azure Cloud Shell 集成

正如我们在《第 2 章：使用 Terraform 配置云基础架构》的“将 Terraform 与 Azure Cloud Shell 集成”部分中详细了解的那样，Packer 也与 Azure Cloud Shell 集成，如以下截图所示：

![](img/d3344b39c888aab0b832625465f837c9_132_0.png)

图 4.3 – Azure Cloud Shell 上的 Packer

现在我们已经了解了在不同操作系统上安装 Packer 及其与 Azure Cloud Shell 的集成，接下来我们将检查其已安装的版本。

## 检查 Packer 安装

安装后，我们可以通过运行以下命令检查已安装的 Packer 版本：

```
packer --version
```

