
我们现在应该已经拥有一个由 quality-control 项目拥有的新创建的卷。此时，quality-control 项目中的所有用户都应该有权修改该卷，但它在附加到实例之前无法真正使用。

我们一次只能将块卷附加到一个实例；需要注意的是，我们无法将此卷对所有项目公开。我们可以做的是，使用卷面板中的 Create Transfer 选项将卷的所有权转移到另一个项目。这将生成一个特殊的 ID 和授权密钥，我们将其提供给另一个项目中的用户。另一个项目中的用户然后输入 Accept Transfer，输入 ID 和 auth key，然后获得对该卷的完全控制权和所有权。卷转移到另一个项目后，除非原始所有者接受回传，否则将无法再访问。

![](img/da864e0670c65edf6f5ab861ef9906b3_252_0.png)

我们也有将卷设为可启动的选项。可启动卷是可以作为 Nova 启动源选择的卷。为了成为合适的启动源，卷必须包含引导加载程序或虚拟机实例 BIOS 可以读取的指令。选择 Edit Instance 并勾选 Bootable 框即可使卷可启动。

## CLI

请回忆提供的 openrc 文件包含用于正确向 Keystone 进行身份验证并设置项目范围的变量形式凭据。

在本章中，我们将在 quality-control 项目中执行所有 Cinder 相关任务。让我们将原始 openrc 文件复制到名为 openrc-quality-control 的新文件中。

```
$ cp openrc openrc-quality-control
```

现在我们已经制作了副本，让我们编辑 openrc-quality-control 文件并更新项目和租户名称变量，以反映我们希望对 logistics 项目的范围。如果您更习惯使用其他编辑器，请在此处随意使用：

```
$ nano openrc-quality-control
```

将以下变量更改为 quality-control：

```
export OS_PROJECT_NAME=quality-control
export OS_TENANT_NAME=quality-control
```

![](img/da864e0670c65edf6f5ab861ef9906b3_253_0.png)

我们更改 OS_TENANT_NAME 变量，因为一些较旧的基于服务的客户端可能仍依赖于旧变量 OS_TENANT_NAME。

载入新创建的 openrc-quality-control 文件：

```
$ source openrc-quality-control
```

验证是否已正确设置变量：

```
$ export | grep OS
```

现在我们已正确限定范围，从现在开始创建的任何新卷都将由 quality-control 项目拥有。与 python-openstackclient 的其他一些 list 命令不同，openstack volume list 仅显示我们当前限定范围的项目内的卷。在考试期间务必牢记这一点。运行以下命令：

```
$ openstack volume list --long
```

我们应该会看到在上一练习中创建的 volume-dashboard 卷。作为管理员，我们应该能够查看和列出环境中所有项目的所有卷。运行以下命令：

```
$ openstack volume list --all-projects --long
```

您现在应该会看到一个之前在虚拟设备上创建的额外卷。

让我们继续使用 CLI 创建一个全新的 1 GB 卷。我们将它命名为 volume-cli，描述为 COA exam prep cli volume：

```
$ openstack volume create --size 1 volume-cli --description "COA exam prep cli volume"
```

我们应该会看到与图 7.9 相似的输出。

![](img/da864e0670c65edf6f5ab861ef9906b3_254_0.png)

图 7.9：openstack volume create 命令的输出

让我们确认卷是否已成功创建：

```
$ openstack volume list --long
```

我们应该会看到与图 7.10 相似的内容。

![](img/da864e0670c65edf6f5ab861ef9906b3_254_1.png)

图 7.10：'openstack volume list --long' 命令的输出

请记住，此命令仅显示您所限定范围的项目内的卷！要查看 OpenStack 环境中所有项目的所有卷，请使用 --all-projects 标志。

## Horizon 仪表盘

在之前的 Horizon 仪表盘练习中，我们创建了一个 1 GB 的 Cinder 块卷。为了使用该卷，我们必须将其附加到虚拟机实例。在进行此练习之前，请确保您没有来自之前章节练习的当前正在运行的实例。

我们需要启动一个 Nova 实例。然后我们将 Cinder 卷附加到此实例并写入一些文件。如果您忘记了如何创建租户网络、子网和虚拟机实例，请参阅第 5 章和第 6 章中的说明。

以下是详细信息。对未指定的任何内容假设使用默认值：

### Network/subnet

- **Project**: `quality-control`
- **Network Name**: `tenant-network-dashboard`
- **Subnet Name**: `tenant-subnet-dashboard`
- **Network Address**: `192.168.1.0/24`

### Instance

- **Project**: `quality-control`
- **Instance Name**: `instance-dashboard`
- **Source**: `cirros`
- **Flavor**: `m1.tiny`
- **Networks**: `tenant-network-dashboard`

现在我们已经在 **quality-control** 项目的 **tenant-network-dashboard** 网络上拥有了 **instance-dashboard**，让我们将一个卷附加到它。见图 7.11。

![](img/da864e0670c65edf6f5ab861ef9906b3_256_0.png)

图 7.11：从 Horizon 仪表盘将卷附加到实例

1. 从 OpenStack 概览页面，通过选择顶部下拉菜单将自己限定到 **quality-control** 项目。
2. 从 **Project-Compute** 面板组中选择 **Instances** 面板。
3. 在 **instance-dashboard** 旁边的 **Actions** 下拉菜单中选择 **Attach Volume**。
4. 见图 7.12。**Volume ID (required)**：这是您想要附加到实例的卷。选择 `volume-dashboard`。
5. 选择 **Attach Volume**。
6. 如果我们想分离卷，可以轻松地从 **Actions** 下拉菜单中选择 **Detach Volume** 并再次选择该卷。

![](img/da864e0670c65edf6f5ab861ef9906b3_256_1.png)

图 7.12：在 Horizon 仪表盘上指定要附加到实例的卷

我们可以通过转到仪表盘上的 **Volumes** 面板组并观察 **Attached To** 列来验证卷是否已成功附加到实例。见图 7.13。卷已作为 `/dev/vdb` 挂载到实例。让我们现在进入实例，看看如何使用该卷。

![](img/da864e0670c65edf6f5ab861ef9906b3_257_0.png)

图 7.13：Cinder 卷成功附加到 Nova 实例

如第 5 章 Nova Compute Service 中所述，访问 **instance-dashboard** 虚拟机实例的 Nova 控制台。

使用以下默认 Cirros 用户名/密码登录实例：

- **Username:** cirros
- **Password:** cubswin:)

现在我们已登录，可以使用流行的 fdisk 实用程序列出任何已识别的块设备：

```
$ sudo fdisk -l
```

您应该会看到与图 7.14 相似的输出。

![](img/da864e0670c65edf6f5ab861ef9906b3_258_0.png)

图 7.14：/dev/vdb 是新附加的 Cinder 块卷

然后我们可以用 ext3 文件系统格式化此原始块设备：

```
$ sudo mkfs.ext3 /dev/vdb
```

尽管我们的磁盘已格式化，但仍需要将其挂载才能使用：

```
$ sudo mount /dev/vdb /mnt
```

现在文件已挂载，我们应该能够在卷上创建文件。让我们使用 touch 实用程序在卷上创建一个名为 helloworld.txt 的文件：

```
$ sudo touch /mnt/helloworld.txt
```

![](img/da864e0670c65edf6f5ab861ef9906b3_258_1.png)

请记住，我们的 OpenStack 虚拟设备只是一台虚拟机，因此它充当超visor 和 Cinder 存储节点。在真实的 OpenStack 环境中，我们的 Cinder 卷将驻留在专用的 Cinder 存储服务器上。

让我们将卷从 /mnt 文件夹卸载：

```
$ sudo umount /dev/vdb /mnt
```

现在我们可以轻松地将此卷从 instance-dashboard 分离并将其附加到另一个已启动的实例。写入此卷的任何数据都可供可以附加它的任何实例访问。

![](img/da864e0670c65edf6f5ab861ef9906b3_259_0.png)

在我们的练习中，我们手动挂载并格式化了驱动器。某些操作系统在识别到新附加磁盘时可能会自动执行此过程。

请务必在本次练习结束时分离卷。您可以通过在虚拟机实例旁边的 Actions 下拉菜单中选择 Detach Volume 来执行此操作。

## CLI

让我们通过 python-openstackclient 创建一个新实例并附加一个新卷。

载入新创建的 openrc-quality-control 文件：

```
$ source openrc-quality-control
```

验证是否已正确设置变量：

```
$ export | grep OS
```

在 quality-control 项目中启动一个新实例：

```
$ openstack server create --image cirros --flavor m1.tiny instance-cli
```

instance-cli 启动后且其状态为 ACTIVE，将 volume-cli 卷附加到它：

```
$ openstack server add volume instance-cli volume-cli
```

您可以通过运行以下命令确认卷已附加到实例：

```
$ openstack volume list --long
```
