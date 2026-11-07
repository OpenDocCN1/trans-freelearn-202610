# 2 设置你的练习考试环境

## 设置你的练习考试环境

在上一章中，我们了解了 OpenStack 的起源以及 Certified OpenStack Administrator 考试的一些细节。现在我们将重点介绍随本书附带的 OpenStack Newton 虚拟设备安装。该虚拟设备将为你提供一个可在本地笔记本电脑或桌面上访问的工作 OpenStack 环境！建议你在阅读本书章节之前先将此环境搭建并运行起来。

在阅读每个目标时，请在你的环境中跟随执行操作。如果你已经可以访问 OpenStack 环境，当然也可以使用它——但请注意，该设备已预先填充了用户、项目、实例、网络和模板，以复制真实的 OpenStack 环境。有关创建资源的脚本，请查看官方书籍 Git 仓库 https://github.com/PacktPublishing/Preparing-for-the-Certified-OpenStack-Administrator-Exam。自 2017 年 6 月起，COA 考试测试的是 OpenStack 的 Newton 版本。

在本章中，我们将涵盖以下主题：
- VirtualBox 和 OVA 文件
- 传统 OpenStack 环境
- 一体式 OpenStack 虚拟设备（随本书附带）
- 在 Mac/Linux 和 Windows 上安装 OpenStack 虚拟设备

![](img/da864e0670c65edf6f5ab861ef9906b3_63_0.png)

OpenStack 可以安装在多种操作系统环境上，包括 Ubuntu、CentOS、Fedora、Red Hat Enterprise Linux、SUSE Linux Enterprise Server 和 OpenSUSE。自 2017 年 6 月起，考生在安排考试时可以选择在 Ubuntu 或 SUSE Linux Enterprise Server 上参加考试。本书将重点关注 Ubuntu 16.04。

完成本章后，你将在本地机器上拥有一个工作的 OpenStack 环境，并准备好开始学习通过 Certified OpenStack 考试所需的目标。

## 关于 VirtualBox

VirtualBox 是一款流行的可供 Windows、Mac、Linux 和 Solaris 下载的超visor。与 VMware Workstation Pro for Windows 和 VMware Fusion for Mac 不同，VirtualBox 是免费的开源软件。它非常适合测试或开发，并允许你在同一系统上设置多个操作系统。VirtualBox 被认为是 Type-2 超visor，旨在在主机操作系统上运行。Type-1 超visor，也称为原生或裸机超visor，直接在主机的硬件上运行。

## Open Virtual Appliance（OVA）

为了使你的练习 COA 考试环境部署变得简单易行，本书为你提供了一个 OVA 或 Open Virtual Appliance，以便轻松导入 VirtualBox。OVA 是一个压缩的 .tar 文件，包含以下内容：
- **虚拟机磁盘文件（VMDK）**：磁盘内容的快照。包含在你的 OVA 中的特定 VMDK 包含一个预先配置的 OpenStack Newton 环境。
- **开放虚拟化格式（OVF）**：描述 VMDK 如何运行的 XML 文件。它包括虚拟 CPU 数量、内存大小、网络配置和虚拟设备的信息。
- **清单（MF）**：它包含每个文件的哈希值，以验证软件包的真实性和完整性。

## 传统的 OpenStack 云

典型的生产 OpenStack 云由以下部分组成：
- **Controller**：控制器是 OpenStack 云的支柱。它通常包含：
  - **MariaDB 数据库**：MySQL 关系数据库的社区开发分支。大多数 OpenStack 服务使用 SQL 数据库来存储列和行表中的数据。例如，想象一下查看 Nova 数据库中的所有表。你会看到实例的名称、它们的唯一标识符（UUID）、拥有这些实例的项目以及其他 Nova 相关元数据。
  - **RabbitMQ**：一种流行的消息队列服务，通过 AMQP 协议提供消息服务。RabbitMQ 允许特定服务内的守护进程通信状态信息并协调特定操作的履行。虽然 OpenStack 支持多种消息队列程序，但 RabbitMQ 是最常用的，因为大多数 Linux 发行版都支持它。
  - **API 守护进程**：所有与 OpenStack 相关服务关联的 API 守护进程都在控制器上运行。将这些 API 视为通往每个服务的首要网关。当用户想要与服务交互时，他们会向驻留在控制器上的 API 守护进程发送 API 调用。
  - **Horizon 仪表板**：Horizon 仪表板运行在流行的 Apache Web 服务器下，也驻留在控制器上。
- **Network nodes**：网络节点包含 Neutron 守护进程，提供 DHCP 服务，以便实例可以自动获取 IP 地址。网络节点还包含第 3 层服务，允许用户创建将 Neutron 网络相互连接的路由器。还有网络插件代理来协助将实例连接到这些网络，以及 Neutron 元数据守护进程（受 Amazon Web Service 流行的 EC2 Metadata Service 启发）。



- **计算节点**：如果将控制器视为 OpenStack 云的大脑，那么计算节点就是肌肉。计算节点提供磁盘、CPU 和内存资源，供实例消耗。每个节点都包含一个 Type-1 hypervisor，以及用于直接与该 hypervisor 通讯的 Nova-compute 守护进程。这些服务器上还有一个 Neutron 插件代理，帮助实例接入虚拟网络基础设施。
- **块存储节点**：块存储节点为 OpenStack 中运行的实例提供 Cinder 块卷。它们通常运行 **逻辑卷管理器**（LVM），这是一种用于管理存储的流行 Linux 程序，以及一个 iSCSI 目标守护进程。当用户想要将卷附加到实例时，会从实例的计算节点向块存储服务器发起 iSCSI 连接，该卷位于该服务器上。
- **对象存储节点**：对象存储节点构成 Swift 对象存储系统。这些节点由具有廉价、大容量存储的商品服务器组成。在这些服务器上运行的 Swift 守护进程负责确保整个对象存储集群中的数据复制和完整性。

## 一体化 Newton 环境

提供的虚拟设备在单个虚拟机上包含所有核心 OpenStack 服务。要下载设备，请前往官方书籍仓库 https://github.com/PacktPublishing/Preparing-for-the-Certified-OpenStack-Administrator-Exam。

请参见图 2.1。设备在 VirtualBox 中运行，而 VirtualBox 运行在您的主机操作系统上。图 2.2 提供了有关虚拟设备细节的一些额外信息。随着我们逐步阅读章节，我们将对此进行更多讨论。

![](img/da864e0670c65edf6f5ab861ef9906b3_66_0.png)

图 2.1：虚拟设备包含 COA 考试中涉及的所有核心 OpenStack 服务

![](img/da864e0670c65edf6f5ab861ef9906b3_67_0.png)

Ceilometer 遥测服务不是考试的必修学习目标，且未安装在虚拟设备上。

| 操作系统 | Ubuntu 16.04 |
| 操作系统用户凭据 | username: openstack
password: openstack |
| vCPU | 1VCPU |
| 内存 | 6GB RAM |
| 磁盘 | 80GB (dynamic) |
| 主机名 | coa-aio-newton |
| VirtualBox 网络 | Adapter1: NAT
Adapter2: Host-Only (vboxnet0) |
| 操作系统网络 | eth0: 10.0.2.15
eth1: 192.168.56.56 |
| 数据库 | MariaDB |
| AMQP | RabbitMQ |
| OpenStack 服务 | Keystone, Glance, Neutron, Nova, Cinder, Heat, Swift |
| 安装方法 / OpenStack 版本 | Ubuntu Cloud Archive - Newton |
| Horizon 控制面板 URL | http://192.168.56.56/horizon |
| OpenStack 用户凭据 | username: admin
password: admin |
| Horizon 多域支持 | Disabled |

图 2.2：本书附带的虚拟设备信息

## 最低硬件要求

虽然 VirtualBox 的系统要求很低，但随附的一体化 OpenStack Newton 虚拟设备需要：

- 2 GHz 或更快的 64 位 (x64) 处理器，支持 Intel VT-x 或 AMD-V
- 6 GB 可用 RAM
- 10 GB 可用硬盘空间

![](img/da864e0670c65edf6f5ab861ef9906b3_68_0.png)

大多数现代 CPU 都包含硬件辅助虚拟化功能，可帮助加速 Type-1 和 Type-2 hypervisor。在导入 OVA 之前，请确保在 BIOS 或 UEFI 中打开了系统的虚拟化扩展。通过进入设置屏幕访问 BIOS 或 UEFI 固件。进入后，查找以下任何选项并确保已打开：Intel Virtualization Technology、Intel VT-X、AMD-V、Virtual Extensions 等。保存设置并重新启动系统。

## 下载和安装 VirtualBox

一旦您在满足最低系统要求的系统上，请按以下步骤操作：

- 1. 导航到 VirtualBox 下载页面 https://www.virtualbox.org/wiki/Downloads。
- 2. 下载并安装适用于您主机操作系统的最新版 VirtualBox。

## 配置 VirtualBox 网络

- 1. 启动 VirtualBox 应用程序。
- 2. 从 VirtualBox 菜单栏中选择“首选项”。
- 3. 在 VirtualBox 首选项菜单中，选择网络图标。
- 4. 验证“仅主机网络”是否已突出显示。

### Mac/Linux

- 1. 如果列表中可以看到 **vboxnet0**，请突出显示并单击工具图标。
- 2. 如果看不到 **vboxnet0**，请单击 + 图标。接下来，突出显示并单击工具图标。单击“确定”保存设置。请参见图 2.3。

![](img/da864e0670c65edf6f5ab861ef9906b3_69_0.png)

图 2.3：在 Mac 或 Linux 上向 VirtualBox 添加 vboxnet0 仅主机网络

### Windows

- 1. 如果列表中可以看到 **VirtualBox Host-Only Ethernet Adapter**，请突出显示并单击工具图标。
- 2. 如果列表中看不到 **VirtualBox Host-Only Ethernet Adapter**，请单击 + 图标。请参见 *图 2.4*。

![](img/da864e0670c65edf6f5ab861ef9906b3_70_0.png)

图 2.4：在 Windows 上向 VirtualBox 添加 VirtualBox Host-Only Ethernet Adapter

## 确认主机专用网络 IP 地址

验证您的 vboxnet0 或 VirtualBox Host-Only Ethernet Adapter 是否如图 2.5 所示。

![](img/da864e0670c65edf6f5ab861ef9906b3_71_0.png)

图 2.5：vboxnet0 或 VirtualBox Host-Only Ethernet Adapter 应具有 192.168.56.1

如果您当前正在 VirtualBox 中为其他虚拟机使用仅主机网络，可以创建一个新的仅主机适配器。验证新的仅主机适配器是否如 图 2.5 所示。导入虚拟设备后，右键单击该设备并选择“设置”。导航到“网络”部分并为适配器 2 选择此仅主机网络。

## 导入设备

- 1. 从 VirtualBox 菜单栏中选择“导入设备”。在 Windows 中它位于“文件”菜单下。
- 2. 单击文件夹图标浏览本地驱动器。
- 3. 选择 coa-aio-newton.ova 并单击“导入”或“打开”。
- 4. 单击“继续”继续安装。
- 5. 单击 **导入**。
- 6. 导入完成后，验证虚拟机是否具有正确的网络设置。请参见 *图 2.6*。

![](img/da864e0670c65edf6f5ab861ef9906b3_72_0.png)

图 2.6：虚拟设备应自动分配两个适配器

- 7. 通过突出显示设备并单击 **开始** 启动虚拟机。请参见 *图 2.7*。

![](img/da864e0670c65edf6f5ab861ef9906b3_72_1.png)

图 2.7：选择“开始”以打开虚拟设备的电源

![](img/da864e0670c65edf6f5ab861ef9906b3_72_2.png)

如果在尝试启动虚拟机时收到错误，请验证您是否满足上述最低系统要求。还请验证 BIOS 或 UEFI 中是否已打开虚拟化扩展。

- 8. 给设备一些时间启动。您会在看到 Ubuntu 登录提示时知道它已启动并运行。请参见 *图 2.8*。

![](img/da864e0670c65edf6f5ab861ef9906b3_72_3.png)

图 2.8：看到登录提示时，虚拟设备已启动并运行

尽管您可以自由使用 VirtualBox 控制台与新导入的 OpenStack 环境进行交互，但您应该从主机操作系统上的终端客户端通过 SSH 连接到设备。

## 通过 SSH 连接到设备

使用终端客户端通过 SSH 连接到虚拟机设备是完成本书目标的最佳方式。连接后，您可以轻松地从本书复制并粘贴命令到终端客户端窗口中。

![](img/da864e0670c65edf6f5ab861ef9906b3_73_0.png)

如果无法直接通过 SSH 连接到实例，您可能已连接到会重定向 LAN 流量的企业或学校 VPN。断开 VPN 连接并尝试重新连接。自定义 SSH 代理配置也可能导致连接问题，应禁用或配置为忽略 LAN 流量。

### Mac/Linux

- 1. 打开内置终端。
- 2. 运行以下命令：
- 3. 提示输入密码时，输入 openstack。

```
$ ssh openstack@192.168.56.56
```

### Windows

- 1. 如果尚未安装终端客户端，请从 http://putty.org 下载 Putty。



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
