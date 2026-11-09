
![](img/23d1402649b6528b085cd6b900a2c9c3_29_0.png)

如你在前一图中可以看到，通信通道中的底层是传输层，而 ASDK 基于的适配器恰好就位于通道栈中的这一层。

WCF 的优点在于它在设计时考虑了可扩展性，这使你能够使用针对特定需求定制的自定义传输绑定。与标准 WCF 传输（如 TCP、命名管道、HTTP 和 MSMQ）类似，ASDK 基于的适配器只是可由 BizTalk 应用程序通过标准 WCF 自定义适配器使用的自定义传输绑定元素。如以下图所示，在出站场景中，ASDK 基于的适配器不会像标准 WCF 传输那样通过网络发送消息，而是直接与 LOB 系统通信，然后向客户端发送响应消息。

![](img/23d1402649b6528b085cd6b900a2c9c3_29_1.png)

在入站场景中，ASDK 基于的适配器会监视或监听目标 LOB 系统针对特定事件的通知，并生成包含事件特定数据的消息供托管应用程序使用。我们将在本章及后续章节中详细介绍。

![](img/23d1402649b6528b085cd6b900a2c9c3_30_0.png)

## 基于 ASDK 的适配器与 WCF 服务

关于新适配器，经常被问到的问题之一是引入 ASDK 技术的原因是什么？有什么问题？那些用了多年、可以完美地被几乎所有应用程序使用的老式 WCF 服务有什么问题？为什么微软要重新发明轮子？嗯，这些都是完全合理的问题，这里是答案。WCF 服务本身没有问题；它们和以前一样好，但有一个很大的“但是”——前提是 LOB 应用程序相对静态。想象一下你的 LOB 应用程序在不断演进，而你作为开发者，必须向客户端应用程序公开新的元数据？你要么更新现有契约，要么实现新的契约，并对主机和 WCF 服务执行后续配置步骤。这对从事 LOB 方面的开发团队来说一直是个大麻烦。

> 元数据通常被定义为关于数据的数据。这是一个非常宽泛的定义；在 WCF LOB Adapter SDK 中，元数据可以被定义为目标 LOB 系统中可用的数据类型和操作。例如，对于 SAP，它可以是 IDOC 和 BAPI，或者对于基于 DBMS 的应用程序，可以是存储过程和数据表。确定并公开特定 LOB 应用程序的元数据是适配器设计者的职责。

相比之下，基于 ASDK 的适配器会自动提供设计时元数据发现和解析，无需开发团队付出任何努力即可使新的元数据（根据我们的定义，即功能）可供使用。此过程的细节超出了本章范围，但如果你有兴趣深入了解，随 WCF LOB Adapters SDK 提供的示例适配器可供使用。查看位于 `C:\Program Files\WCF LOB Adapter SDK\Documents\Samples` 目录中的 Contoso 示例适配器如何实现 `IMetadata*` 接口组，你就会明白自动元数据发现和解析背后的原理。实现 `IMetadata*` 接口确实需要适配器开发人员付出大量努力，但最终结果绝对值得。

现在你已经熟悉了架构基础，让我们继续进行适配器的安装。

## 安装

BizTalk Adapter Pack 2010 的安装过程并不复杂，但与 BizTalk 的一切一样，需要准确性和一些耐心。第一步要做的是确保你的系统满足先决条件要求。你必须在计算机上安装以下组件：

- Windows XP SP2、Windows Server 2003、Windows Server 2008、Windows Vista 或 Windows 7
- 用于开发目的的 Visual Studio 2010
- 带有 Windows Communication Foundation 适配器的 BizTalk Server 2010。WCF 适配器默认未安装；在安装 BizTalk Server 2010 时必须显式启用它。

![](img/23d1402649b6528b085cd6b900a2c9c3_32_0.png)

完成验证后，你可以继续进行安装，本质上是以下步骤的顺序：

- 1. 安装 WCF LOB Adapter SDK。
- 2. 安装 BizTalk Adapter Pack 2010。
- 3. 安装额外的客户端库以在适配器与其目标 LOB 系统之间启用通信。唯一不需要任何额外组件的适配器是 SQL 适配器。

从上述步骤来看，安装额外的客户端库是最耗时且最容易出错的过程。我们将在专门介绍 SAP 适配器的章节中详细探讨这一主题。

尽管安装 WCF LOB Adapter SDK 和 BizTalk Adapter Pack 是直接的过程，但在 64 位环境中存在一个陷阱。如果你的计算机运行在 32 位操作系统下，你只需要安装 WCF LOB Adapter SDK 和 BizTalk Adapter Pack 的 32 位版本。在 64 位环境中，情况稍微复杂一些。首先你必须安装 WCF LOB Adapter SDK 的 64 位版本和 BizTalk Adapter Pack 2010 的 64 位版本。然后，如果你在设计时使用 Visual Studio 和 BizTalk 管理控制台等工具，或在运行时使用 32 位 BizTalk 主机实例，你将必须安装 BizTalk Adapter Pack 的 32 位版本和客户端库。这听起来可能违反直觉，但考虑到 Visual Studio 和 BizTalk 管理控制台是 32 位应用程序，因此只能与 32 位组件交互，这种要求就变得清晰了。因此，如果在 64 位环境中运行 BizTalk 安装程序时，它提供同时安装 BizTalk Adapter Pack 2010 的 32 位和 64 位版本，请不要感到惊讶。现在我们已经熟悉了要求，可以开始 ASDK 适配器的安装了。

## 安装 WCF LOB Adapter SDK

第一步是奠定基础，即安装 WCF LOB Adapter SDK。

- 1. 从 BizTalk 安装 DVD 或 ISO 的 `\Setup\BT Setup` 文件夹启动 `setup.exe`，然后选择 **Install Microsoft BizTalk Adapters**。
- 2. 选择 **Step 1: Install Microsoft WCF LOB Adapter SDK**。请注意，安装程序会自动安装正确版本；你无需在 32 位和 64 位之间显式选择。

![](img/23d1402649b6528b085cd6b900a2c9c3_33_0.png)

- 3. 在 **Welcome** 屏幕上，单击 **Next** 按钮。
- 4. 接受许可条款并单击 **Next**。
- 5. 在 **Installation Type** 屏幕上，单击 **Custom** 按钮。**Custom Setup** 屏幕将显示出来。
- 6. 你始终必须安装运行时组件，无论是否计划使用 UI 工具。如果你在计划使用 UI 设计时组件的计算机上安装，例如 Visual Studio 的 **Add Adapter Service Reference plug-in**，以及 BizTalk 管理控制台的 **BizTalk Server Add-in**，你必须启用 **Tools** 和 **BizTalk Server Add-in** 组件的安装。本章后面的演练中，你还需要随 WCF LOB Adapter SDK 提供的 **Samples**。

![](img/23d1402649b6528b085cd6b900a2c9c3_34_0.png)

- 7. 单击 **Next**，然后单击 **Install** 开始安装。
- 8. 安装完成后，单击 **Finish** 按钮关闭 WCF LOB Adapter SDK 安装程序并返回主适配器安装窗口。

## 安装 BizTalk Adapter Pack 2010

我们现在将安装 BizTalk Adapter Pack 2010。请注意，如果你处于 64 位环境，安装程序会提供同时安装 BizTalk Adapter Pack 2010 的 32 位和 64 位版本。

