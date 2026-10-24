

WS EMS 是一个独立的应用程序，安装在远程设备上。它使用一种名为 AlwaysOn 的 ThingWorx 协议与 ThingWorx Core 通信。WS EMS 支持多种操作系统，且占用空间很小。它可以与大量设备配合使用，为边缘设备与 ThingWorx Core 之间的通信提供一种方式。

根据开发人员为 IoT 设备代码使用的语言，有多个版本的 **ThingWorx Edge SDKs** 可供选择，让你为设备添加连接性。有支持 C、.NET (C#) 和 Java 语言的 SDK，以及 Android 和 iOS 平台的 SDK。

SDK 嵌入在 IoT 设备中：每个设备一个实例。所有 SDK 都有通用接口，并为 Core 提供安全通信通道。

## ThingWorx 概念

我们将讨论在 ThingWorx core 上用于建模环境的主要对象和概念，以更好地了解其工作原理。

## Thing 模板

**thing template** 设置并定义了构建多个 things 的基础功能。这定义了一个通用类别，具有一组共同的属性、服务、事件和订阅。这些将包含在基于该模板的任何 thing 定义中。thing 将 *inherit* 自 thing template。

## Things

**thing** 用于表示一个对象。它基于 thing template，但通常会包含模板中定义的更通用基础 thing 特有的额外属性、服务或事件。

有几种类型的 thing：

- **Things:** 这代表现实世界中的资产、设备或系统。
- **Remote thing:** Remote thing 是一种特殊的 thing 类型，表示远程位置中的资产。使用 ThingWorx Edge SDK 时，运行应用程序的边缘设备需要在 ThingWorx Composer 中使用 RemoteThing 模板创建。
- **Custom things:** 扩展提供了自定义 thing 模板，你可以使用它来创建自定义 things 以表示你的设备。

## Properties

属性是一个表示 thing 行为的变量。

属性具有以下两种之一：

- **Remote bindings:** Remote bindings 支持 egress。当设备连接到 ThingWorx 时，可以写入边缘设备。边缘设备必须知道具有 Remote bindings 的任何属性。
- **Non-remote bindings:** 这些在写入时不会发送到边缘设备，也不会在请求值时从边缘设备读取。

## Services

服务代表 thing 可以执行的功能。它可以作为 thing、thing template 或 thing shape 的一部分定义。

## Events

事件是由属性的状态或值变化触发的。它向订阅它的对象发送数据。

## 4

## 创建 AWS 云分析环境

本章是创建 AWS 云环境的逐步演练。环境专门针对分析和使用 AWS 最佳实践。除了屏幕截图和设置说明外，还将解释正在做什么以及原因。本演练会产生 AWS 使用费用。请确保在演练后删除所有资源，如果你不打算让环境持续运行，因为整个月的总成本可能超过 130 美元。视需要启动和停止 EC2 实例可以 somewhat 降低成本。

本章将涵盖以下主题：

- AWS CloudFormation
- 设置最佳实践虚拟私有云：
  - NAT 网关
  - Bastion 主机
- 如何终止和清理环境

## AWS CloudFormation 概述

AWS CloudFormation 换句话说就是基础设施即代码。它是一个 AWS 服务，你无需安装任何额外软件。这允许开发人员和系统管理员直接从代码模板文件设计和实现整个网络和服务器配置。当模板实施时，CloudFormation 会自动处理资源的排序和创建。当模板启动以创建资源时，它被称为 stack。

我们将使用其中一个 AWS 快速入门模板来创建一个安全环境，以便开始使用 IoT 分析。快速入门模板将 AWS 最佳实践纳入设计。你将在几分钟内拥有自己的迷你数据中心。 我们将使用的模板专为高可用性而设计，如果你决定扩展使用，它将有足够的空间增长。

如果你为公司设置用于标准操作的<|message|> 环境，作者建议你聘请一家专门创建云环境的咨询公司。这是为了确保底层安全性和架构设置正确。从一开始就正确配置要比以后尝试纠正容易得多。

然而，对于实验和一般分析使用，我们将在演练中创建的环境工作得很好。它已实施了若干安全和连接最佳实践。

## AWS 虚拟私有云 (VPC) 设置演练

演练假设你已设置了以下内容：

- **AWS account:** 如果你没有帐户，请访问 AWS 控制台页面并按照说明设置帐户。说明可在 https://aws.amazon.com/free/ 获取。
- **Root accounts with Multi-Factor Authorization (MFA) active:** 这对安全至关重要。如果你尚未启用 MFA，请立即启用。在手机上下载 Google Authenticator 等应用并在帐户上设置 MFA。说明可在 https://aws.amazon.com/iam/details/mfa/ 获取。
- **IAM users with administrative rights and MFA active:** 为自己设置一个具有管理员权限的 IAM 用户，用于日常操作。出于安全目的，避免使用 root 登录。还为 IAM 用户设置 MFA。

- **Follow all other recommended steps on the IAM welcome screen to secure your account:** 页面上的安全状态应全部为绿色勾选，并如下图所示：

![](img/9f2a55db1236f995dcd9392acbc82910_116_0.png)

现在，我们将开始演练的初始步骤：

- 1. 开始演练，使用你的 IAM 用户 ID 登录 AWS 管理控制台。如果登录屏幕要求输入电子邮件地址，则你处于 root 用户登录而非 IAM 用户登录。你需要转到 IAM 用户登录屏幕。如果你难以到达该页面，诀窍是输入以下格式的 URL
  `https://[AccountName].signin.aws.amazon.com/console`，其中将 `[AccountName]` 替换为你自己的帐户名称。此链接也可以在登录后在 IAM 起始页上找到。
- 2. 确保 AWS 区域设置为你想要创建环境的区域。对于大多数用例，最好将其设置为地理位置最近的区域。你可以通过查看管理控制台右上角来检查区域。如有必要，你可以通过单击名称右侧的小箭头来更改它：

## 为 NAT 和 bastion 实例创建密钥对

你需要创建一个公钥/私钥对，以便稍后在快速入门设置中使用。密钥对将保存为文件，你应将其保存在安全的地方，只有需要的人才能访问：

- 1. 在屏幕顶部的 AWS 管理控制台中转到 **Services**：

![](img/9f2a55db1236f995dcd9392acbc82910_117_0.png)

![](img/9f2a55db1236f995dcd9392acbc82910_117_1.png)

Services location on console

2. 然后，在 **Compute** 部分单击 **EC2**：

![](img/9f2a55db1236f995dcd9392acbc82910_118_0.png)

3. 从 EC2 **Dashboard**，单击 **Key Pairs**：

![](img/9f2a55db1236f995dcd9392acbc82910_118_1.png)

4. 单击 **Create Key Pair** 按钮：

![](img/9f2a55db1236f995dcd9392acbc82910_118_2.png)

5. 为其命名，例如 `vpc_keypair` 并单击 **Create**。避免在名称中使用大写字母、空格或非标准字符。这将使其更易于在不同的操作系统和文件存储系统之间移植。

