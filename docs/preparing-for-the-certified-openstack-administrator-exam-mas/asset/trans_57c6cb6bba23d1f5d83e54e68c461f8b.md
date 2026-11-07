
- 8. **禁用网关**：未选中。
- 9. 参见图 6.50。**启用 DHCP**：用户不会在提供者网络上启动实例，因此请确保此项**未选中**。
- 10. **分配池**：这允许我们限制之前定义的网络地址 `172.16.0.0/24` 的范围。这将成为本章最后一节中将使用的**浮动 IP** 池。输入 `172.16.0.10,172.16.0.30`。
- 11. **DNS 名称服务器**：我们将留空，因为没有实例附加到此提供者子网。
- 12. **主机路由**：我们将留空，因为我们不会注入任何主机路由。
- 13. 选择 **创建**。

![](img/da864e0670c65edf6f5ab861ef9906b3_223_0.png)

图 6.50：Horizon 仪表板上的提供者子网 DHCP 选项

参见图 6.51。我们现在应该有一个新创建的子网！

![](img/da864e0670c65edf6f5ab861ef9906b3_224_0.png)

图 6.51：Horizon 仪表板上新配置的提供者子网

## 命令行界面

让我们在 CLI 上创建提供者子网。我们将首先 source openrc。

```
$ source openrc
```

验证正确变量是否已设置：

```
$ export | grep OS
```

因为我们为 CLI 示例使用不同的 provider-physical-network，所以我们将使用不同的子网、网关和分配池。

```
$ openstack subnet create --network provider-network-cli --subnet-range 172.16.1.0/24 --gateway 172.16.1.1 --no-dhcp provider-subnet-cli
```

您应该会看到以下输出：

![](img/da864e0670c65edf6f5ab861ef9906b3_225_0.png)

图 6.52：'openstack subnet create' 命令的输出

通过运行以下命令确认 provider-network-cli 是否成功创建：

```
$ openstack subnet list --long
```

恭喜！您已在 CLI 上创建了一个全新的提供者子网。

## 考试目标 - 管理路由器 - 北/南 流量

此时，我们的东/西路由器流量场景应与图 6.26 相似。这是一个很好的例子，说明 Neutron 路由器如何帮助您路由内部 Neutron 租户网络。现在我们已经创建了一些提供者网络，得益于我们的路由器，我们应该能够将实例的租户网络连接到它们。连接后，我们的实例将可以访问互联网！

在开始此练习之前，请验证运行虚拟设备的宿主机是否具有有线/无线互联网访问权限。请务必禁用任何 VPN 或代理。它们可能会干扰外部连接。

## Horizon 仪表板

是时候将我们的路由器网关连接到之前创建的提供者网络，以便 tenant-network1-dashboard 和 tenant-network2-dashboard 上的实例可以访问互联网：

- 1. 参见图 6.53。从 OpenStack 概览页面，通过选择顶部下拉菜单将范围限定到 logistics 项目。
- 2. 选择 Project-Network 面板组中的面板。这里您应该能看到我们之前创建的路由器：router-dashboard。
- 3. 在 router-dashboard 旁边选择 Set Gateway。

![](img/da864e0670c65edf6f5ab861ef9906b3_226_0.png)

图 6.53：Horizon 仪表板上设置路由器网关

- 4. 参见图 6.54。**外部网络**（必填）：这里我们将选择之前创建的提供者网络：`provider-network-dashboard`
- 5. 选择 **提交**。

![](img/da864e0670c65edf6f5ab861ef9906b3_227_0.png)

图 6.54：为路由器指定提供者网络网关

恭喜！您刚刚将实例连接到互联网！在我们继续测试连接之前，请参见图 6.55。这是我们当前网络拓扑的高级视图。路由器的默认网关设置为 `provider-network-dashboard`。所有未发往 `tenant-network-dashboard` 和 `tenant-network2-dashboard` 的流量都将流经此处。

![](img/da864e0670c65edf6f5ab861ef9906b3_228_0.png)

图 6.55：北/南 流量网络拓扑

要从 OpenStack 实例测试互联网连接，运行 VirtualBox 的个人笔记本电脑或台式机必须连接到互联网。

让我们现在访问 logistics 项目中的 `instance1-dashboard`，以确保它可以访问互联网：

从实例部分选择 `instance1-dashboard`。

选择 **Console** 选项卡。选择 **Show Only Console**。如果仍然看不到任何内容，请点击 **Enter**，这应该会唤醒控制台屏幕。

我们现在可以登录到实例：

username: cirros

password: cubswin:)

登录后，让我们尝试 ping（3 次）openstack.org。请耐心等待！可能需要几分钟才能看到 ping 回复。

```
$ ping -c 3 openstack.org
```

您应该会看到以下响应：

![](img/da864e0670c65edf6f5ab861ef9906b3_229_0.png)

图 6.56：来自 openstack.org 的 ping 响应确认北/南 流量通信成功

如果您在不使用 -c 计数参数的情况下执行 ping，可以通过按 Ctrl + C 来取消它。

## 命令行界面

让我们通过 python-openstackclient 将 router-cli 设置为 provider-network-cli。这是一个相当简单的命令。

开始之前，请务必 source openrc-logistics 凭据文件，因为我们正在 logistics 项目中工作。

```
$ source openrc-logistics
```

验证正确变量是否已设置：

```
$ export | grep OS
```

列出所有可用的路由器：

```
$ openstack router list
```

列出所有可用的网络并验证 provider-network-cli 是否列出：

```
$ openstack network list --long
```

将 router-cli 的网关设置为 provider-network-cli。

对于此命令，我们需要使用基于服务的客户端 python-neutronclient。这是因为 python-openstackclient 版本 2.7.12 不支持设置路由器网关。

要使用 python-neutronclient 设置路由器网关，请运行以下命令：

```
$ neutron router-gateway-set router-cli provider-network-cli
```

![](img/da864e0670c65edf6f5ab861ef9906b3_230_0.png)

截至本书撰写时，python-openstackclient 版本 3.6 支持使用以下命令设置路由器网关：openstack router set router-cli --external-gateway provider-network-cli。注意！考试可能只包含较旧版本的 python-openstackclient 且不支持此功能。

## 考试目标 - 管理浮动 IP

在之前的目标中，我们将路由器网关设置为提供者网络。这为连接到路由器的网络上的所有实例提供了互联网访问权限。但如果我们想从 *OpenStack 环境* 外部访问这些实例中的某个应用程序或资源怎么办？由于分配给我们实例的 IP 地址是内部的且不可公网路由，Neutron 允许用户将浮动 IP 与其实例关联。浮动 IP 类似于 Amazon Web Services 世界中的弹性 IP 概念。使用它们是一个两步过程：

- 1. **分配** 一个浮动 IP 给您的项目。
- 2. **分配** 浮动 IP 给实例。

如果您不再需要浮动 IP，可以轻松地将其从项目中取消分配并释放，发送回原始浮动 IP 地址池。记住：浮动 IP 地址池是在提供者网络子网中定义的。在现实世界中，这可能是由我们的互联网服务提供商提供的外部 IP 地址范围。

## Horizon 仪表板

让我们为 **logistics** 项目中的 **instance1-dashboard** 实例关联一个浮动 IP 地址。然后我们将尝试通过其浮动 IP 从虚拟设备 ping 该实例。由于我们拥有管理员角色，我们将了解如何通过 admin-system 面板组分配特定 IP 地址。让我们开始！

- 1. 参见图 6.57。从 OpenStack 概览页面，选择 **Admin-System** 面板组。
- 2. 选择 **Floating IPs** 面板。

3. 选择 **Allocate IP to Project**。

![](img/da864e0670c65edf6f5ab861ef9906b3_232_0.png)

图 6.57：从 Horizon 仪表板分配浮动 IP 地址

4. 参见图 6.58。**Pool**（必填）：选择我们之前创建的提供者网络，`provider-network-dashboard`。

5. **Project**（必填）：选择将拥有浮动 IP 地址的项目。由于我们一直在 logistics 项目中进行 Neutron 工作，请选择 `logistics`。

6. **Floating IP Address**（可选）：我们可以在此处指定想要的浮动 IP 地址。在我们的练习中，让我们请求 `172.16.0.100`。如果我们不指定 IP 地址，我们将从在 **提供者子网** 练习中创建的提供者子网中定义的 `172.16.0.0/24` 池中分配一个随机的可用浮动 IP 地址。
