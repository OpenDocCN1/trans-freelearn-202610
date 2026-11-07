
如果用户希望持久化其数据，则鼓励他们使用 S3 对象存储服务。

2008 年 4 月，AWS 做出了一项改变这一切的巨大公告。“我们即将推出的持久存储功能将使您能够为 EC2 创建可靠的持久存储卷”公告博文称。“这些卷可以被视为原始的未格式化驱动器，可以进行格式化后按需使用（如果您愿意，甚至可以作为原始存储使用）。”

这是 IT 历史上的一个令人难以置信的时刻：弹性云持久块存储的引入！

类似于便携式 USB 驱动器，用户可以创建指定为千兆字节的块卷。一旦附加到虚拟机实例，卷在操作系统内部就会显示为原始未格式化的硬盘。格式化驱动器后，它可以像传统硬盘一样挂载并使用。写入磁盘的所有数据实际上都驻留在 AWS 数据中心内的专用服务器上，而不是实例所在的计算节点。参见图 7.2。

![](img/da864e0670c65edf6f5ab861ef9906b3_244_0.png)

现在，用户还可以使用其主驱动器（即 `/dev/xvda`）作为 EBS 卷。参见图 7.3。这在实例故障时为基础操作系统提供完整的数据持久性；它被称为“EBS-backed”实例。

![](img/da864e0670c65edf6f5ab861ef9906b3_245_0.png)

图 7.3：主硬盘也可以由块卷支持

受 AWS 弹性块存储启发，OpenStack 在 2011 年 Bexar 版本中于 Nova 项目中引入了块卷的概念。2012 年，OpenStack Folsom 版本将块存储作为其自己的项目引入，名为 Cinder。

## Cinder 架构

Cinder 由四个守护进程组成，如图 7.4 所示：

- **cinder-api**：Cinder 的 API 和主要网关。必须与 `cinder-api` 交互以创建、列出、删除和管理块卷、快照和备份。
- **cinder-scheduler**：评估并过滤所有可用的存储节点，以确定为要创建的块卷选择最佳存储节点。`cinder-scheduler` 的行为可以根据容量或存储能力等特定特征进行修改。
- **cinder-volume**：驻留在 Cinder 存储节点上，负责块卷的创建和删除。
- **cinder-backup**：协助将块卷备份到 OpenStack Swift 或其他第三方存储目标。

![](img/da864e0670c65edf6f5ab861ef9906b3_246_0.png)

图 7.4：Cinder 架构的高级视图

## Cinder 背后

默认情况下，Cinder 由两种特定技术提供支持：

- 1. **LVM (Logical Volume Manager)**：LVM 工具集在 1990 年代后期开发，允许用户在不中断服务的情况下添加和更换硬盘。这是通过创建整个硬盘的单个逻辑卷来实现的，从而允许动态调整大小。
- 2. **iSCSI (Internet Small Computer Systems Interface)**：iSCSI 是一种基于 IP 的存储标准，用于在网络上提供对存储设备的块级访问。

考虑一个用户创建 Cinder 块卷的情况。用户通过 Horizon 仪表板或 CLI 向 cinder-api 发送 API 请求。一旦 cinder-api 收到请求，它就会使用有关卷的详细信息更新 Cinder 数据库并将其放置在消息总线上。cinder-scheduler 确定为请求的卷配置最佳的 Cinder 存储节点。一旦选定，cinder-volume 负责生成 LVM 命令以从预定义的卷组创建逻辑卷。然后用户向 nova-api 发送 API 调用以将卷附加到其选择的实例。此连接过程实际上是实例的计算节点（发起方）与 Cinder 存储节点（目标）之间的 iSCSI 连接。参见图 7.5。用户然后可以继续在实例的操作系统内格式化块设备并写入数据。写入 Cinder 的所有数据都存储在 Cinder 存储中。用户然后可以从实例分离卷并将其重新附加到另一个实例，从而重新建立到不同计算节点的 iSCSI 连接。

![](img/da864e0670c65edf6f5ab861ef9906b3_247_0.png)

图 7.5：Cinder 使用 LVM 和 iSCSI 向 Nova 实例呈现原始块设备

## Cinder 后端软件和硬件驱动程序

Cinder 支持多种软件和硬件驱动程序用于存储后端。在硬件驱动程序的情况下，这允许用户将现有的硬件存储设备（如 SAN 或 NAS）用作 Cinder 存储节点。

| 软件驱动程序 | 硬件驱动程序 |
|---|---|
| LVM | NetApp |
| NFS | Dell EqualLogic |
| GlusterFS | EMC V-MAX |
| Ceph | SolidFire |

## Cinder 概念

- **Volume**：可附加到 Nova 虚拟机实例的原始未格式化块设备。然后实例的操作系统可以像传统硬盘一样使用它。
- **Snapshot**：卷内容的只读、时间点副本。可以从当前使用中或可用状态的卷创建快照。然后快照可用于创建新卷。
- **Backup**：存储在 Swift 对象存储容器中或 Google Cloud Platform 等其他第三方提供商中的卷内容的压缩归档文件。

## 考试目标

让我们看看 Cinder 考试目标。

以下图表分解了您在 COA 考试中应负责了解的各种 Cinder 相关目标。如您所见，所有这些任务都可以在 Horizon 仪表板和 CLI 上完成。记住：考试期间时间就是一切，在许多情况下，在仪表板上处理任务会快得多。

![](img/da864e0670c65edf6f5ab861ef9906b3_249_0.png)

图 7.6：Cinder 考试目标可以在 Horizon 仪表板或 CLI 上完成

## 考试目标 - 管理 Cinder 卷

创建 Cinder 卷非常简单，应该是在考试中轻松得分的方式。让我们看看！

## Horizon 仪表板

让我们在 Horizon 仪表板上创建一个卷。参见图 7.7。

![](img/da864e0670c65edf6f5ab861ef9906b3_250_0.png)

图 7.7：从 Horizon 仪表板创建块卷

- 1. 从 OpenStack 概览页面，通过选择顶部下拉菜单将自己限定为 **quality-control** 项目。
- 2. 从 **Project-Compute** 面板组中选择 **Volumes** 面板。
- 3. 选择 **Create Volume**。
- 4. 参见图 7.8。**Volume Name (optional)**：卷的名称。输入 `volume-dashboard`。
- 5. **Description (optional)**：卷的描述。输入 `COA exam prep dashboard volume`。
- 6. **Volume Source (optional)**：默认情况下，您创建的任何卷都是全新的且为空。卷源是您希望复制到卷中的现有数据源。以下是卷源的示例：
  - 1. **No Source, Empty Volume**：一个空卷。卷必须由虚拟机实例的操作系统格式化和挂载。我们将选择此项。
  - 2. **Image**：这会将现有 Glance 镜像的内容复制到您的块卷。您必须确保卷的大小等于或大于指定镜像的大小。这将自动使您的镜像 **bootable**，以便在启动实例时将其指定为主要磁盘（启动源）。这是一个强大的功能，将导致您的 `/dev/vda` 文件夹驻留在 Cinder 存储节点上，类似于 AWS EBS-backed 实例。
  - 3. **Volume**：这会将现有卷的内容复制到您的块卷。您必须确保卷的大小等于或大于指定卷的大小。
- 7. **Type (optional)**：允许用户指定由 OpenStack 基础设施工程师先前配置和定义的特定卷后端。一个例子是只有 SSD 驱动器的 Cinder 存储服务器/节点组。另一个例子可以是具有传统 SATA 驱动器的 Cinder 存储节点组。这赋予了您的 OpenStack 用户选择的权力。
- 8. **Size (required)**：卷的大小（以千兆字节为单位）。输入 1。
- 9. **Availability Zone (optional)**：类似于 Nova 可用区，Cinder 可用区在安装期间定义，并为您提供分隔 cinder 存储节点的通用方法。默认情况下，所有 Cinder 存储节点都属于 Nova 可用区。
- 10. 选择 **Create Volume**。

![](img/da864e0670c65edf6f5ab861ef9906b3_251_0.png)

图 7.8：在 Horizon 仪表板上提供块卷详细信息
