
源: cirros

规格: m1.tiny

网络: tenant-network1-dashboard

安全组: default

![](img/da864e0670c65edf6f5ab861ef9906b3_206_0.png)

图 6.31：在 Horizon 控制台上启动新的虚拟机实例

## Instance2（参见图 6.32）

实例名称: instance2-dashboard

可用区: nova

数量: 1

源: cirros

规格: m1.tiny

网络: tenant-network2-dashboard

安全组: default

![](img/da864e0670c65edf6f5ab861ef9906b3_207_0.png)

图 6.32：验证 instance2-dashboard 是否在 tenant-network2-dashboard 上启动

不要担心如果你的实例在启动时未获得 .3 IP 地址。我们无法保证仅将实例附加到网络时就能获得第一个可用的 IP 地址。如果您想保留或指定特定的 IP 地址，可以通过 Admin-System Networks 面板或通过 python-openstackclient 创建端口来实现。例如，在创建实例之前，您可以通过运行以下命令为 192.168.1.3 创建一个端口： `openstack port create --network <network-uuid> --fixed-ip subnet=<subnet-uuid>,ip-address=192.168.1.3 my-new-port`。创建端口后，在启动实例时指定端口（而不是网络）。有关更多详细信息，请检查 `openstack help port create`。

请参阅图 6.33。我们现在应该在 logistics 项目中拥有两个实例。

![](img/da864e0670c65edf6f5ab861ef9906b3_208_0.png)

图 6.33：Horizon 控制台上 logistics 项目中的两个实例

在这一点上，这些网络上的两个子网彼此完全隔离。为了使 instance1-dashboard 能够与 instance2-dashboard 通信，我们需要创建一个路由器。

请参阅图 6.34。要创建路由器，请按照以下步骤操作：

- 1. 从 OpenStack 概览页面，通过选择顶部下拉菜单将自己限定到 **logistics** 项目。
- 2. 从 **Project-Network** 面板组中选择 **Routers** 面板。
- 3. 选择 **Create Router**。

![](img/da864e0670c65edf6f5ab861ef9906b3_209_0.png)

图 6.34：在 logistics 项目中创建路由器

- 4. 请参阅图 6.35。**Router Name**（必填）：路由器的名称。输入 `router-dashboard`。
- 5. **Admin State**（必填）：控制网络是关闭还是开启。如果将其设置为 DOWN 状态，它将不会路由任何流量。验证其是否设置为 **UP**。
- 6. **External Network**（可选）：选择路由器的网关。我们将在处理南北向流量时更详细地探讨这一点。现在，将其保持在 **Select Network**。

- 7. 选择 **Create Router**。

![](img/da864e0670c65edf6f5ab861ef9906b3_210_0.png)

图 6.35：通过控制台添加路由器详细信息

太好了！我们现在有一个由 logistics 项目拥有的路由器。路由器本身不能做太多事情，因此我们需要将其连接到我们的两个网络。

- 1. 请参阅图 6.36。选择 **router-dashboard**。

![](img/da864e0670c65edf6f5ab861ef9906b3_210_1.png)

图 6.36：在 Horizon 控制台上选择路由器

- 2. 请参阅图 6.37。选择 **Interfaces** 选项卡。

![](img/da864e0670c65edf6f5ab861ef9906b3_211_0.png)

图 6.37：在 Horizon 控制台上向路由器添加接口

- 3. 选择 **Add Interface**。
- 4. 请参阅图 6.38。**Subnet**（必填）：选择要连接到路由器的子网名称。我们将选择 **tenant-network1-dashboard:192.168.1.0/24**。
- 5. **IP Address**（可选）：这允许我们为网关指定特定的 IP 地址。我们将将其留空，因为我们假设它将是默认的 `192.168.1.1`。
- 6. 选择 **Submit**。

![](img/da864e0670c65edf6f5ab861ef9906b3_211_1.png)

图 6.38：通过控制台添加接口详细信息

继续创建另一个路由器接口，执行相同的步骤序列，但添加 tenant-network2-dashboard:192.168.2.0/24。

结果应类似于图 6.39。由于我们没有费心手动创建端口，因此此处的 IP 地址可能不同。只需确保记下每个实例的 IP 地址即可。

![](img/da864e0670c65edf6f5ab861ef9906b3_212_0.png)

图 6.39：Horizon 控制台上连接到路由器的两个接口

让我们继续检查实例之间的连通性。通过按照第 5 章 Nova Compute Service 中概述的说明，通过控制台访问 instance1-dashboard 实例。

如果您忘记了，这里是 Cirros 登录凭据：

username: cirros

password: cubswin:)

现在我们已登录，让我们尝试 ping（3 次）instance2-dashboard，地址为 192.168.2.x。如果您记不起 instance2-dashboard 的 IP 地址，请返回控制台上的实例页面进行检查！

```
$ ping -c 3 192.168.2.x
```

我们应该会收到回复！并且会看到类似于图 6.40 的内容。

![](img/da864e0670c65edf6f5ab861ef9906b3_213_0.png)

图 6.40：驻留在另一个租户网络上并响应 ping 的实例。这证实了东西向流量通信成功

![](img/da864e0670c65edf6f5ab861ef9906b3_213_1.png)

如果您在不使用 -c 计数参数的情况下执行 ping，可以通过按 Ctrl + C 来取消它。

## CLI

![](img/da864e0670c65edf6f5ab861ef9906b3_213_2.png)

如果您计划在 CLI 上尝试此练习，请确保删除您刚刚在控制台上创建的实例！虚拟设备很可能无法处理所有这些正在运行的虚拟机！如果您想继续在控制台上进行练习，请随时跳到下一节：Horizon 控制台上的管理提供商网络。

让我们使用 python-openstackclient 设置我们的东西向流量场景。

导入 openrc-logistics 文件：

```
$ source openrc-logistics
```

验证是否已设置正确的变量：

```
$ export | grep OS
```

让我们创建新的附加网络和子网：

```
$ openstack network create tenant-network2-cli
$ openstack subnet create --network tenant-network2-cli --subnet-range 192.168.2.0/24 tenant-subnet2-cli
```

让我们启动我们的实例：

```
$ openstack server create --image cirros --flavor m1.tiny --nic net-id=tenant-network1-cli instance1-cli
$ openstack server create --image cirros --flavor m1.tiny --nic net-id=tenant-network2-cli instance2-cli
```

确认实例是否成功创建：

```
$ openstack server list
```

让我们创建路由器：

```
$ openstack router create router-cli
```

我们应该会看到以下输出：

![](img/da864e0670c65edf6f5ab861ef9906b3_214_0.png)

图 6.41：'openstack router create' 命令的输出

现在让我们将两个子网连接到路由器。在此之前，让我们列出可用的子网：

```
$ openstack router add subnet router-cli tenant-subnet1-cli
$ openstack router add subnet router-cli tenant-subnet2-cli
```

您不会看到这些命令的任何输出。为了验证我们的工作，我们可以使用 --router 标志运行 openstack port list 命令：

```
$ openstack port list --router router-cli
```

我们应该会看到以下输出：

![](img/da864e0670c65edf6f5ab861ef9906b3_215_0.png)

图 6.42：'openstack port list --router' 命令的输出

我们现在可以确认实例应该能够成功相互 ping。

## 考试目标 - 管理提供商网络

现在我们了解了如何管理租户网络和子网，以及如何在两个不同的租户网络之间设置东西向流量，让我们创建一些提供商网络。提供商网络对于任何云环境都至关重要，因为它们使 OpenStack 实例能够访问 OpenStack 环境之外的外部资源。如果 Nova 实例只能与存在于 OpenStack 环境中的资源通信，那将非常无用！如果实例无法访问互联网来下载最新的操作系统软件包，有什么用？如果我们的客户无法从外部访问我们的 Web 应用程序，实例有什么用？提供商网络是提供对 OpenStack 之外事物网络访问的魔力。

## Horizon 控制台

在控制台上创建提供商网络之前，务必记住，默认情况下，只有具有管理员角色的用户才能创建提供商网络。
