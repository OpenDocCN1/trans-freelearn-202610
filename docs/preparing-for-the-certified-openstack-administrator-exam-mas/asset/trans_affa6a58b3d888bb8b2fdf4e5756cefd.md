
2. Putty 运行后，填写用户名、IP地址和端口。见图 2.9。

![](img/da864e0670c65edf6f5ab861ef9906b3_74_0.png)

图 2.9：在 Windows 上使用 Putty 通过 SSH 连接到虚拟设备

3. 提示输入密码时，输入 openstack。

## 使用 CLI 验证 OpenStack 服务连通性

- 1. 连接到虚拟设备后，source openrc 文件：
- 2. 使用 python-openstackclient 确认您可以成功检索令牌：

```
$ source openrc
```

```
$ openstack token issue
```

![](img/da864e0670c65edf6f5ab861ef9906b3_75_0.png)

## 使用 Horizon 仪表板验证 OpenStack 连通性

Horizon 仪表板可访问 ...（由于模糊，文本无法辨认）

- 1. 打开一个互联网浏览器并导航 ...（由于模糊，文本无法辨认）
- 2. 加载页面可能需要一些时间 ...（由于模糊，文本无法辨认）

![](img/da864e0670c65edf6f5ab861ef9906b3_75_1.png)

3. 输入以下凭据并单击 Connect：

- 用户名: admin
- 密码: admin

4. 然后您将看到 Horizon 概览屏幕。见图 2.12。

![](img/da864e0670c65edf6f5ab861ef9906b3_76_0.png)

图 2.12 Horizon 仪表板概览屏幕

TIP 在生产 OpenStack 云中，您将通过 HTTPS（SSL/TLS）连接到 Horizon 仪表板和 API 端点。

## 摘要

您现在应该拥有自己完全部署并在笔记本电脑或台式机上运行的工作 OpenStack 环境！本章讨论了 VirtualBox 以及 OVA 格式的便利性，以及构成传统 OpenStack 环境的组件。

到目前为止，您应该能够通过系统上的 Horizon 仪表板和终端客户端连接到虚拟设备。您当然可以自由使用自己的 OpenStack 环境，但本书中的练习和模拟考试将需要先前在提供的虚拟设备中创建的资源。

在下一章中，我们将讨论 Keystone 标识服务，并通过 CLI 和 Horizon 仪表板完成所有 Keystone 考试目标。让我们开始吧！

## 3

## Keystone 标识服务

让我们通过讨论 Keystone、OpenStack 标识服务，来开始准备 Certified OpenStack Administrator 考试目标。理解 Keystone 是通过 COA 考试的主要构建块之一。

在本章中，我们将涵盖以下主题：

- 身份验证和授权？
- Keystone 架构
- Keystone 概念
- 身份验证实践
- Horizon 仪表板和 CLI 上的项目范围
- 考试目标 - 管理配额
- 考试目标 - 管理用户、角色、组、项目和域
- 考试目标 - 管理服务目录 - 服务和端点

完成本章后，您应该对 Keystone 有扎实的理解，并具备成功完成考试中所有与 Keystone 相关的目标所需的技能。

## 关于 Keystone

所有用户和服务都依赖 Keystone。没有 Keystone，用户将无法正确进行身份验证并创建虚拟机、网络和其他资源。OpenStack 服务将无法正确进行身份验证以成功履行用户请求，或向其他 OpenStack 服务发出请求。

## 身份验证或授权？

在开始深入 Keystone 的细节之前，让我们花点时间思考 **身份验证** 和 **授权** 的概念。这两个词有时会被误用，因此让我们提供一个正确的定义。

**身份验证** 是确认特定用户身份的行为——换句话说，证明用户就是其声称的那个人。

**授权** 是确定该特定用户的访问权限的功能。

Keystone 有多种功能，但首先也是最重要的是，它提供 **身份验证**。所有 OpenStack 用户都必须通过 **Horizon 仪表板**、**命令行界面 (CLI)**、SDK 或直接通过 **API** 对 Keystone 进行身份验证。

一旦用户通过身份验证，Keystone 就不再参与确定用户的访问权限。访问权限包括诸如用户是否可以创建 Neutron 虚拟路由器或创建可公开访问的 Glance 镜像等事项。此 **授权** 由名为 `policy.json` 的文件处理，该文件位于每个已安装 OpenStack 服务的配置目录中。考试不要求您了解如何通过此文件修改授权，但在继续之前理解身份验证和授权之间的区别是基础。

## Keystone 架构

让我们简要了解一下 Keystone 架构。Keystone 是整个云的通用身份验证系统，接收的请求比任何其他 OpenStack 服务都多。自 OpenStack Liberty 版本以来，Keystone 开发团队改用 Apache 作为 Keystone 的 API 前端。见图 3.1。Apache 守护进程绑定到端口 5000，是通往 Keystone 的主要网关。所有用户和服务都依赖 Keystone 进行身份验证。OpenStack 环境的关系数据库管理系统（通常是 MariaDB，MySQL 的分支）包含一个 Keystone 数据库，用于存储所有各种 Keystone 资源的数据。

![](img/da864e0670c65edf6f5ab861ef9906b3_80_0.png)

图 3.1：Keystone 是中央 OpenStack 身份验证服务，并使用 Apache Web 服务器作为其前端 Web 服务器。所有用户和 OpenStack 服务都必须对 Keystone 进行身份验证

## Keystone 概念

让我们看一些常见的 Keystone 术语。如果您对这些概念的介绍感到有点困惑，不用担心。随着我们完成特定的考试目标并在实验室环境中执行任务，我们将更详细地介绍它们。

- **域：** 域是用于用户、组和项目的高级逻辑容器。考虑一家新 IT 公司想要创建一个 OpenStack 公有云。如果这家公司真的想在 IT 世界产生影响，他们需要托管和支持多个客户为自己的 OpenStack 账户注册。该公司很可能会将每个新客户放在自己的域中。与 Amazon Web Services 或 Google Cloud Platform 等流行的公有云类似，每个客户都会创建与同一环境中其他客户隔离的资源。客户不仅能够创建自己的虚拟资源，例如虚拟机或网络，还能在该域内创建自己的用户、项目和组。

OpenStack 中的几乎每个资源或对象都会获得一个 **通用唯一标识符 (UUID)**。尽管我们可以为这些资源分配人类可读的名称，但 UUID 用于在日志、数据库和 OpenStack 基础设施的其他区域中唯一标识资源和对象。

- **项目：** 项目代表资源被创建的区域。考虑我们之前提到的运行 OpenStack 公有云的公司。他们为每个客户设置自己的域，并在该域内允许客户创建多个 **项目**。见图 3.2。这些项目很可能代表各种业务单元，以便他们可以轻松隔离资源。存在于其他项目中的用户将无法看到您项目中创建的资源，除非明确共享。会计、财务或人力资源是现实世界 OpenStack 项目中可能的项目的例子。用户当然可以属于一个或多个项目，但他们一次只能对一个项目进行身份验证。自己对单个项目进行身份验证的概念称为 **scope**。经过身份验证的用户创建的任何资源始终归用户当前作用域的项目所有。有一些例外，但我们将在本书后面讨论它们。

如果您听到 OpenStack 世界中的人使用 **tenant** 这个词，不要感到困惑。在 OpenStack 的早期版本中，“tenant”用于描述我们现在所知的项目。您甚至可能会听到有人将项目称为账户！**项目、租户和账户** 在 OpenStack 世界中含义相同。

![](img/da864e0670c65edf6f5ab861ef9906b3_81_0.png)

图 3.2：域是用于用户、组和项目的高级逻辑容器。项目存在于域内，并代表虚拟资源被创建的区域
