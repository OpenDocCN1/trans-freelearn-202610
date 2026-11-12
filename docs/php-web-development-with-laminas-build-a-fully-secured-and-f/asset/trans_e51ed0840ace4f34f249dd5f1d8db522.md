

![](img/e6cf5550804ccae9d1859270da8688f5_29_0.png)

图 1.1 – Laminas 文档主页

文档主页上的 **组件** 和 **MVC** 选项与本书内容直接相关。不过，Laminas 还包含微框架 **Mezzio**（原名 Expressive）和 **API 工具**，后者是一个基于 Laminas 组件构建的 API 生成器。

所有组件的源代码可在 `https://github.com/laminas` 处供下载和贡献。你会发现有超过 100 个包含成熟组件的仓库。在此页面上，你可以打开 issue 来修复错误或请求新功能——请记住，对于功能，你获得的响应程度将取决于社区的兴趣。如果你有改进现有组件甚至创建新组件的想法，请提交实现代码，而不仅仅是规范说明。为自由开源项目做贡献是了解软件开发的绝佳方式，也能帮助你在同行中建立知名度，他们随后可能成为你的顾问。为 Laminas 做贡献的指南可在 `https://github.com/laminas/.github/blob/main/CONTRIBUTING.md` 处找到。

Roger Pressman 将软件定义为计算机程序及其相关文档的组合。人也是软件文档的一部分，因为他们通常持有尚未转化为显性知识的隐性知识。当文档不足时，我们可能需要寻求他人的帮助，录音对话也可以成为文档。你可以在官方讨论论坛中与参与或关注 Laminas 的人交流，该论坛可在 `https://discourse.laminas.dev` 访问。

你可以通过向 `security@getlaminas.org` 发送消息来报告潜在安全漏洞。如需了解 Laminas 的安全策略，请阅读 `https://getlaminas.org/security`。

你可以通过打开 issue 并在论坛上发帖提问来获得社区支持；你甚至可以通过扩展组件自己解决问题。但如果你需要 *按需咨询支持* 或 *长期支持*，可以考虑 Zend Technologies 的企业支持。如需了解该服务的更多信息，请阅读 `https://www.zend.com/products/laminas-enterprise-support`。

在对 Laminas 进行此概述后，下一章我们将准备使用该框架进行开发的环境。

## 摘要

软件就像一个生命体：它诞生、成长，终有一天会消亡。当然，我们不希望自己的软件过早消亡。为此，我们需要赋予软件一些特性，使其能够长期生存。本章我们讨论了有助于创建可重用且易于维护软件的框架。

我们介绍了 Laminas 的历史，它是一个用于构建 PHP Web 应用程序的免费开源框架。我们展示了它的演进过程，以及它作为可演进应用程序框架的设计方案。

在下一章，我们将准备使用 Laminas 开发 PHP Web 应用程序的环境。

## 为我们的电子商务应用程序设置环境

要建造一栋房子，我们需要各种工具和材料。工具越先进，盖房子就越容易；材料越好，房子越耐用。软件亦是如此。合适的工具可以帮助提高生产力，合适的组件可以使软件更容易维护。在本章中，我们将为本书中将要构建的示例应用程序设置开发环境：一个允许客户注册和购买商品的虚拟商店。所选工具均为面向世界上最常用操作系统的免费开源软件（OSS）。

本章将涵盖以下主题：

- 安装通用 PHP：超文本预处理器（PHP）环境，包括 Web 服务器、PHP 解释器和关系数据库管理系统（RDBMS）
- 安装和使用 Laminas 的两种不同方式
- 安装集成开发环境（IDE）——Eclipse 平台
- 将 Laminas 相关命令集成到 Eclipse 工具栏
- 本书练习中将使用的数据模型

具体来说，本章将包含以下部分：

- 安装 Laminas
- 配置 IDE——适用于 PHP 开发者的 Eclipse
- 集成 Laminas 和 Eclipse
- 管理 MySQL

## 技术要求

本书使用 Laminas MVC 3.3.4，该版本至少需要 PHP 7.4。然而，PHP 7.4 的支持已于 2022 年结束，因此建议至少使用 PHP 8.0，并鼓励使用 PHP 8.1。你可以在 https://www.php.net/supported-versions.php 上查看 PHP 支持的截止日期。

要创建和运行本书中的代码，你至少需要一个 PHP 解释器、一个 Web 服务器和一个 MySQL/MariaDB 实例（如果你调整数据库脚本，也可以使用 PHP 支持的任何数据库）。每种软件都有多种安装方式，且有些软件包将它们集成在一起。XAMPP 是一个捆绑 Apache HTTP Server、PHP 和 MySQL/MariaDB 的软件包。你可以从 https://www.apachefriends.org/download.html 下载适用于 Windows、Linux 或 macOS 的 XAMPP。由于它可用于世界上大多数操作系统，我们将其作为默认的 PHP 环境。此外，XAMPP 提供了一个控制面板，可让你轻松启动/停止 Apache 和 MySQL。在下面的截图中，我们展示了 Linux 上的 XAMPP 控制面板示例：

![](img/e6cf5550804ccae9d1859270da8688f5_32_0.png)

图 2.1 – XAMPP 控制面板窗口

Apache Friends（XAMPP 的托管项目）的主页有一个简短且不错的视频，介绍了安装过程：https://www.apachefriends.org/index.html。

XAMPP 还附带一个 **phpMyAdmin** 实例用于管理 MySQL 数据库。一旦你通过 XAMPP 控制面板启动了 Apache 和 MySQL，就可以在 `https://localhost/phpmyadmin` 访问 phpMyAdmin。

我们有时会使用命令行。为了避免在终端中使用 PHP 解释器的完整路径，你必须将包含 PHP 解释器的目录添加到 PATH 环境变量中。PHP 位于以下路径：

- c:\xampp\php（适用于 Windows）
- /opt/lampp（适用于 Linux）
- /Applications/XAMPP/bin（适用于 OS X）

安装好 PHP 环境后，你必须安装 **Composer**，它是 PHP 的依赖管理器。你可以从 `https://getcomposer.org/download` 下载它。将 `composer.phar` 文件放在与 PHP 解释器相同的目录中。对于 Linux 和 OS X 用户，只需将文件重命名为 `composer` 并授予其运行权限。对于 Windows 用户，请按照 `https://getcomposer.org/doc/00-intro.md#installation-windows` 中的说明操作。

从现在起，我们的工作区将是 XAMPP 安装目录下的 `htdocs` 文件夹。我们将在该文件夹中创建所有示例。请记住：在本书中，当我提到 **工作区** 时，指的就是 `htdocs`。确保 `htdocs` 文件夹具有读写权限，以便你可以使用自己的用户在其中运行命令，并使用 IDE 打开文件。

> **注意**
> 如果你已经安装了 Web 服务器、PHP、Composer 和 MySQL，可以使用它们，但本书假设使用 XAMPP，因此关于目录结构和程序的引用均与 XAMPP 相关。

## 安装 Laminas

你可以通过以下任一方式安装 Laminas：

- 为现有 PHP 应用程序（或没有 MVC 的小型应用程序）安装 Laminas 的独立组件
- 为基于 MVC 架构的新应用程序安装骨架应用程序

## 通过 Laminas 的独立组件安装 Laminas

让我们以安装对象关系映射（ORM）和数据库抽象组件 laminas-db 为例。在 htdocs 文件夹中，运行以下命令：

```
composer require laminas/laminas-db
```

此命令将创建一个名为 vendor 的文件夹并下载 laminas-db 组件及其依赖项。要加载组件的类，你需要导入 vendor/autoload.php 文件。暂时，你只需检查 Composer 是否正常工作。稍后我们将使用独立组件构建基本示例，所以不必担心。成功下载后，你可以删除 laminas-db。如仅需删除该组件，请运行以下命令：

```
composer remove laminas/laminas-db
```

此命令将删除 vendor 文件夹中的 laminas-db 组件，但会保留 vendor 文件夹。htdocs 根目录下的该文件夹（vendor）不会被使用，因此你可以将其删除。我们将为项目创建一个特定的文件夹。

## 通过 MVC 骨架应用程序安装 Laminas

要使用 Laminas 启动 MVC Web 项目，你有三种选择，如下所示：

- 1. 通过 https://github.com/laminas/laminas-mvc-skeleton Git 仓库克隆骨架。

克隆后，你需要删除版本控制。可以通过进入 laminas-mvc-skeleton 文件夹并删除其中的 .git 文件夹来完成。.git 文件夹是 Git 仓库结构的一部分。它不会显示在 GitHub 页面上，但在克隆仓库时会被创建。之后，你可以将 laminas-mvc-skeleton 文件夹重命名为你的项目名称。

- 2. 通过以下链接下载骨架：https://github.com/laminas/laminas-mvc-skeleton/archive/refs/heads/1.3.x.zip。

解压缩文件并将文件夹重命名为 laminas-mvc-skeleton。

- 3. 使用 Composer 安装骨架。

我们将使用第三种方法。为此，请运行以下命令：

```
composer create-project -sdev laminas/laminas-mvc-skeleton
myfirstlaminas
```

