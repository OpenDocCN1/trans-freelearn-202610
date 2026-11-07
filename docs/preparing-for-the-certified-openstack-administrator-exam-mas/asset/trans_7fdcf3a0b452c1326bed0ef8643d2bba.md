
请参阅图 7.15。观察状态列。这显示卷是否可用或正在使用。请记住：OpenStack 的 Newton 版本一次只允许将卷附加到一个实例。

![](img/da864e0670c65edf6f5ab861ef9906b3_260_0.png)

图 7.15：'Attached to' 列显示卷所附加到的实例

您可以通过运行以下命令获取 instance-cli 控制台 URL：

```
$ openstack console url show instance-cli
```

您可以随意通过虚拟机实例控制台格式化并挂载该卷，如上一节所示。

要分离卷，请运行以下命令：

```
$ openstack server remove volume instance-cli volume-cli
```

## 考试目标 - 管理 Cinder 快照

Cinder 支持捕获卷当前状态的卷快照。这些对于提供备份保护很有用，也可用于实例化包含快照确切数据的新的卷。在幕后，Cinder 快照是 LVM 快照，将驻留在与原始逻辑卷相同的 LVM 卷组中。需要注意的是，快照不是备份。与 Cinder 快照不同，Cinder 备份存储在异地。我们将在下一节介绍它们。让我们在仪表板中逐步创建和恢复快照。

### Horizon 仪表板

请参阅图 7.16。让我们从 Horizon 仪表板创建卷快照。

![](img/da864e0670c65edf6f5ab861ef9906b3_261_0.png)

图 7.16：从 Horizon 仪表板创建快照

- 1. 从 OpenStack 概览页面，通过选择顶部下拉菜单将自己限定到 **quality-control** 项目。
- 2. 从 **Project-Compute** 面板组中选择 **Volumes** 面板。
- 3. 在 **volume-dashboard** 下拉菜单旁边选择 **Create Snapshot**。
- 4. 见图 7.17。 **Snapshot Name (required)**：快照的名称。输入 `volume-snapshot-dashboard`。
- 5. **Description (optional)**：卷快照的描述。输入 `COA exam prep dashboard volume snapshot`。

- 6. 选择 **Create Volume Snapshot**。

![](img/da864e0670c65edf6f5ab861ef9906b3_262_0.png)

图 7.17：在 Horizon 仪表板上为快照提供源卷的名称

您现在应该能在 **Snapshots** 选项卡下看到新创建的卷快照。该快照归 `quality-control` 项目所有。您可以通过选择 **Create Volume** 按钮轻松从中创建卷。

TIP
从现有快照创建新卷时，卷的大小必须等于或大于快照的大小。为了删除具有关联快照的卷，您必须先删除快照。

### CLI

让我们通过 `python-openstackclient` 创建一个新的 Cinder 快照。

载入新创建的 `openrc-quality-control` 文件：

```
$ source openrc-quality-control
```

验证是否设置了正确的变量：

```
$ export | grep OS
```

查看当前所有 Cinder 卷：

```
$ openstack volume list
```

对 volume-cli 卷进行快照。快照名称为 volume-snapshot-cli。

```
$ openstack snapshot create --name volume-snapshot-cli volume-cli
```

您应该会看到与图 7.18 相似的输出。

![](img/da864e0670c65edf6f5ab861ef9906b3_263_0.png)

图 7.18：'openstack snapshot create' 命令的输出

您可以通过运行以下命令确认快照已创建：

```
$ openstack snapshot list --long
```

如果您想从此快照创建卷，可以运行以下命令：

```
$ openstack volume create --snapshot volume-snapshot-cli --size 1 restored-snapshot-volume-cli
```

## 考试目标 - 管理 Cinder 备份

与 Cinder 快照类似，Cinder 备份包含源卷中的所有数据。与 Cinder 快照不同，Cinder 备份是压缩的 bz2 或 zlib 归档文件，存储在 Cinder 存储节点之外。目标备份位置可以根据使用的驱动程序存在于多种位置。驱动程序包括 OpenStack Swift、Google Cloud Platform、GlusterFS，甚至 NFS 共享（仅举几例）。您的虚拟设备目前配置为使用安装在设备上的 OpenStack Swift 部署。我们将在第 8 章《Swift 对象存储服务》中更深入地讨论 Swift。让我们在仪表板中逐步创建和恢复快照。

### Horizon 仪表板

让我们从 Horizon 仪表板创建 Cinder 备份。请参阅图 7.19。

![](img/da864e0670c65edf6f5ab861ef9906b3_264_0.png)

图 7.19：在 Horizon 仪表板上创建备份

- 1. 从 OpenStack 概览页面，通过选择顶部下拉菜单将自己限定到 **quality-control** 项目。
- 2. 从 **Project-Compute** 面板组中选择 **Volumes** 面板。
- 3. 在 **volume-dashboard** 下拉菜单旁边选择 **Create Backup**。
- 4. 见图 7.20。 **Backup Name (required)**：备份的名称。输入 `volume-backup-dashboard`。
- 5. **Description (optional)**：卷快照的描述。输入 `COA exam prep dashboard volume backup`
- 6. **Container Name (optional)**：备份的 Swift 对象容器目标名称。输入 `volume-backup-container-dashboard`

- 7. 选择 Create Volume Backup。

![](img/da864e0670c65edf6f5ab861ef9906b3_265_0.png)

图 7.20：在 Horizon 仪表板上为 Cinder 备份设置目标容器名称

一旦备份创建完成，我们应该会有一个新的 Swift 对象容器，其中包含压缩并归档的卷内容。

我们将在第 8 章更详细地讨论 Swift。现在，请确保您已限定到 **quality-control** 项目，并在 **Project-Object Store** 面板组中选择 **Containers** 面板。请参阅图 7.21。

![](img/da864e0670c65edf6f5ab861ef9906b3_265_1.png)

图 7.21：'openstack volume backup list' 命令的输出

要恢复此备份，我们可以在 **Volume Backups** 选项卡中使用 **Restore Backup** 按钮。

### CLI

让我们通过 `python-openstackclient` 创建一个新的 Cinder 备份。

载入新创建的 `openrc-quality-control` 文件：

```
$ source openrc-quality-control
```

验证是否设置了正确的变量：

```
$ export | grep OS
```

创建备份：

```
$ openstack volume backup create --name volume-backup-cli --description "COA exam prep dashboard volume backup" --container volume-backup-container-cli volume-cli
```

备份完成后，您应该能够确认备份已成功创建：

```
$ openstack volume backup list
```

您应该会看到与图 7.22 相似的输出。

![](img/da864e0670c65edf6f5ab861ef9906b3_266_0.png)

图 7.22：'openstack volume backup list' 命令的输出

如果您想在 CLI 上恢复备份，您必须先手动创建一个空卷，然后运行以下命令：

```
$ openstack volume backup restore volume-backup-cli <target-volume>
```

## 注意

在继续下一章之前，请务必删除所有正在运行的实例以释放可用资源。您可以运行以下命令：

```
$ openstack server list --all-projects
$ openstack server delete <server-uuid>
```

## 总结

您刚刚学习了考试中所有与 Cinder 相关的目标。如您所见，所有与 Cinder 相关的目标都可以在 Horizon 仪表板上完成。您可以通过使用要附加卷的实例旁边的下拉菜单轻松地将卷附加到实例。请记住，在 OpenStack 的 Newton 版本中，一次只能将卷附加到一个实例。在下一章中，我们将讨论 Swift，即 OpenStack 对象存储服务，并完成考试中所有与 Swift 相关的目标。

## 8

## Swift 对象存储服务

在上一章中，我们讨论了 Cinder 块存储服务以及 Certified OpenStack Administrator 考试中的 Cinder 相关目标。现在我们将讨论 Swift。Swift 是 OpenStack 对象存储服务，为用户提供通过 RESTful API 访问的冗余云存储。

在本章中，我们将涵盖以下主题：

- Swift 架构
- Swift 概念
- 考试目标 - 管理容器和对象
- 考试目标 - 在容器上设置 ACL
- 考试目标 - 设置对象过期

在本章之后，您应该对 Swift 有扎实的理解，并具备成功完成考试中所有与 Swift 相关目标所需的技能。

### 关于 Swift

如果您想在 90 年代末或 2000 年代初注册网站托管服务提供商，您通常会使用一个提供商为您提供服务器访问权限的服务。该服务器会为您分配特定量的存储空间供您使用和存储文件。每月支付固定费用。您可以通过 Secure SSH 或 FTP 客户端上传网站的 HTML 页面、照片和其他静态文件。如果您需要额外的存储空间，您将提交请求，托管服务提供商会扩展您的容量。
