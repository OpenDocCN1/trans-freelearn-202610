
## 镜像文件格式

磁盘镜像存储硬盘的所有内容，并采用多种文件格式。以下是一些最受欢迎的选项：

- **QCOW2（QEMU 复制时写入）**：与 QEMU 和 QEMU-KVM 超级管理器一起使用，其名称源于其磁盘存储优化策略，该策略会延迟存储分配，直到实际需要时为止。
- **VMDK（虚拟机磁盘）**：最初设计用于 VMware 系列超级管理器，但现在是一种开放格式，与 VirtualBox 等其他非 VMware 超级管理器兼容。
- **VHD 和 VHDX（虚拟硬盘）**：最初由 Connectix Corporation 为类型 2 超级管理器 Virtual PC 创建，2003 年被微软收购。它通常与 Microsoft 相关产品和服务一起使用，例如 Hyper-V 和 Azure。
- **VDI（虚拟磁盘映像）**：由 Oracle 的 VirtualBox 使用。
- **ISO（国际标准化组织，ISO9660）**：光盘的归档文件。它由光盘上每个已写入扇区的数据内容组成，包括光盘文件系统。
- **RAW**：未压缩的磁盘映像。Nova 会在启动虚拟机实例时解压缩任何压缩的云映像并将其置于原始格式。

![](img/da864e0670c65edf6f5ab861ef9906b3_120_0.png)

虽然我们使用 Glance 来注册、存储和检索镜像，但它并不关心存储的数据类型。事实上，Glance 会存储您扔给它的 .txt、.doc 或任何其他文件类型。确保 Glance 镜像与环境中部署的超级管理器（或超级管理器）兼容是 OpenStack 管理员的责任。

## Glance 架构

参见图 4.1。Glance 由两个主要守护进程组成：

- **glance-api**：Glance 的主要网关。必须与 `glance-api` 交互才能存储和检索磁盘镜像。
- **glance-registry**：负责在关系数据库中存储与镜像关联的元数据。镜像元数据的示例包括镜像名称、镜像位置、UUID、镜像大小、所有者（项目 ID）、可用性状态和磁盘格式。

Glance 可以将镜像存储在多种数据存储中，包括 Swift、Amazon S3、`glance-api` 守护进程所在的本地文件系统，甚至是公开可访问的 Web 服务器。

![](img/da864e0670c65edf6f5ab861ef9906b3_121_0.png)

图 4.1：Glance 架构的高级图

本书提供的虚拟设备将 `glance-api` 配置为使用 Swift 数据存储。这是生产 OpenStack 环境中最常用的数据存储。

## Horizon 仪表板上的 Glance

Glance 出现在 Horizon 仪表板 **Project Panel Group** 的左侧。参见 *Figure 4.2*。在提供的虚拟设备中，您可以看到一个先前上传的云镜像，名为 **Cirros**。Cirros 是一种轻量级、最小化操作系统镜像，专为用作云的测试镜像而设计。您永远不会在 Cirros 上运行生产工作负载，但它非常适合云教育、开发和测试！这个特定的 Cirros 镜像由 admin 项目拥有。还请仔细注意 **visibility** 列。这个特定 Cirros 镜像的 **visibility** 设置为 *public*，因此使其可供所有域中的所有项目访问并可启动。**Visibility** 是理解 Glance 并通过 Glance 相关 COA 目标的基础。

让我们分解一下镜像的可能可见性状态：

- **Public**：公开的镜像可供 OpenStack 环境中所有域中的所有项目使用。根据默认 Glance `policy.json` 文件，只有具有管理员角色的用户才能将镜像设置为公开。公开镜像只有在您限定到拥有该镜像的项目时才可以修改或删除。
- **Private**：根据默认 Glance `policy.json` 文件，任何具有任何角色的用户都可以将镜像上传到 Glance。为了在 Glance 中创建/上传镜像，用户必须限定到一个项目。镜像成功创建/上传后，它将由用户当时限定到的项目拥有，并显示为 *Private*。该项目中的任何用户现在都可以修改镜像的属性或启动镜像。拥有私有镜像的项目之外的任何用户都无法看到该镜像，除非它已被明确与项目共享。
- **Shared with Project:** 如果用户具有管理员角色，他们可以将镜像与一个特定项目共享。如果用户明确将镜像与您的项目共享，它将显示为 *Shared with Project*。除非您限定到拥有该镜像的项目，否则您无法修改或删除与您的项目共享的镜像。

![](img/da864e0670c65edf6f5ab861ef9906b3_123_0.png)

图 4.2：Horizon 仪表板上的可见性列决定 Glance 镜像是公开、私有还是与项目共享

## Glance 镜像属性与 Glance 元数据定义

当镜像上传到 Glance 时，只要用户限定到拥有该镜像的项目，用户就可以修改与镜像关联的元数据。由于 Horizon 仪表板上有两种修改 Glance 镜像属性的方法，因此最初可能会有点令人困惑。

参见图 4.3。从 Horizon 仪表板选择 Edit Image 选项允许用户编辑名称、描述、格式、最小磁盘/最小内存、公开和受保护属性。这些不是由 Glance 元数据定义服务设置的定义，而是 Glance 镜像固有的简单镜像属性。

![](img/da864e0670c65edf6f5ab861ef9906b3_124_0.png)

图 4.3：在 Horizon 仪表板上编辑 Glance 镜像属性

另一方面，Glance 元数据定义是可以设置在 OpenStack 资源上的属性。默认 Glance `policy.json` 文件只允许具有管理员角色的用户管理元数据定义，而所有其他具有其他角色的用户可以将这些定义应用于 OpenStack 中所有允许的资源。

图 4.4 显示了在 Admin-System 面板组中可用的元数据定义面板：

![](img/da864e0670c65edf6f5ab861ef9906b3_125_0.png)

图 4.4：由 OpenStack 管理员加载的默认 Glance 元数据定义模板

正如您所见，OpenStack 管理员已加载默认模板。这些元数据定义模板可在 `github.com/openstack/glance/etc/metadefs.` 获取。

典型的 OpenStack 用户将能够将这些已加载的属性应用于他们在 OpenStack 中创建的资源。这里有一个例子：考虑一个由 KVM、Hyper-V 和 XEN 超级管理器组成的多个超级管理器的 OpenStack 环境。如果用户创建 Glance 镜像，他们可以在该镜像上设置元数据，以仅允许在与镜像文件格式兼容的超级管理器上启动它。

要将此元数据应用于镜像，用户上传其镜像，然后可以单击操作下拉菜单中的更新元数据。现在您可以从左侧的可用元数据窗格中选择 hypervisor_type 属性并将其添加到镜像中。

然后您可以选择适当的超级管理器值：

![](img/da864e0670c65edf6f5ab861ef9906b3_126_0.png)

图 4.5：在 Horizon 仪表板上更新与 Glance 镜像关联的元数据定义

## 考试目标

在回顾了 Glance 镜像服务的一些基本细节后，我们可以看到它相当简单。与 Keystone 考试目标类似，并非所有 Glance 相关目标都可以在仪表板上执行。

下表根据第 1 章《介绍 OpenStack 和 Certified OpenStack Administrator 考试》中显示的官方目标列表中的各种目标进行了细分，可在 http://www.openstack.org/coa/requirements 获取：

![](img/da864e0670c65edf6f5ab861ef9906b3_127_0.png)

图 4.6：Glance 考试目标

要在 Horizon 仪表板上将镜像上传到 Glance，镜像必须通过本地文件系统访问。

## 考试目标 - 创建 Glance 镜像

作为 OpenStack 管理员，您将负责创建（也称为上传）Glance 镜像，以便您的 OpenStack 用户可以使用它们来启动实例。

让我们看看如何通过 OpenStack 仪表板和 python-openstackclient 来完成此操作。

我们将使用 python-openstackclient 显示所有 Glance 目标，但您当然也可以使用 python-glanceclient。记住：考试不关心您如何完成目标，只要它们成功完成任务即可。
