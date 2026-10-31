
# 路由和接口配置

## 路由配置

* `Discard`：路由可以设置为“black hole”某些子网。这可用于防止任何数据包到达连接的带外网络，即使安全策略允许这种情况。

* `None`：路由可能没有下一跳，例如路由到 VPN 隧道中的数据包。

`Admin Distance` 和 `Metric` 设置可以根据需要为每个路由更改。

`Route Table` 用于向常规单播路由、多播路由或两者添加路由。

如果您有冗余可用，可以使用 `Path Monitoring` 通过路由发送心跳 ping。如果 ping 失败达到配置的次数，路由将被禁用。路由表将针对匹配的数据包重新评估，并将使用下一个最佳匹配来路由数据包（即具有更高度量值或更大子网的路由）：

![](Images/B31589_02_58.png)

图 2.58：`VR` 默认路由

在三层接口上配置的任何子网都会作为直连网络添加到路由表中，无需添加静态路由。

## 二层接口和 VLAN

将接口设置为二层类型可使防火墙的功能类似于在网络中放置一台交换机。每个接口充当交换机上相当于接入端口（如果需要中继功能，请参阅 `Subinterfaces` 主题）的角色，您可以根据需要添加任意数量的接口。

每个接口应使用不同的区域，以便利用安全策略控制接口之间的流量。设置为同一区域的接口默认情况下会在不检查的情况下交换流量，并且需要一个捕获所有流量的安全策略来启用检查。

要将接口分组为逻辑“交换机”，您需要通过 `Networks` | `VLANs` 创建 VLAN 对象，并添加您之前设置为二层且希望连接的接口：

![](Images/B31589_02_59.png)

图 2.59：VLAN 组

`VLAN Interface` 选项将路由功能作为逻辑三层接口添加到组中。如果您有上游 ISP 路由器或连接到需要交互的三层接口的不同子网，这可能会很有用。

您可以通过 `Network` | `Interfaces` | `VLAN` 配置 VLAN 接口。将其分配给您创建的 VLAN 组，填写 `Virtual Router` 字段，并分配一个区域。此区域在为安全策略与三层接口交互时将代表二层接口：

![](Images/B31589_02_60.png)

图 2.60：VLAN 接口配置

您还需要为 VLAN 接口分配一个 IP 地址，以便二层接口上的客户端可将其用作默认网关或路由下一跳。请确保它与二层接口上的客户端位于同一子网中：

![](Images/B31589_02_61.png)

图 2.61：VLAN 接口 IP 地址

## Tap 接口

Tap 接口可用作被动嗅探端口。如果另一台网络设备设置了端口镜像，其 egress 端口可以连接到 tap 接口以拦截所有数据包并应用应用 ID 和内容 ID。只要 Tap 接口接收到会话的所有数据包，它就能像流量流经防火墙一样检查流量。然而，有一些限制：

* 防火墙并未积极参与数据包的处理，因此如果检测到威胁，它无法采取行动；只能报告。
* SSL 解密只能应用于入站连接，如果服务器证书连同其私钥可以加载到防火墙上。

Tap 接口只需配置安全区域：

![](Images/B31589_02_62.png)

图 2.62：Tap 接口

为了充分利用 Tap 功能，需要创建一个安全规则以允许所有操作，或如果您想限制范围，则允许特定子集。防火墙会在后台丢弃所有数据包，但将安全规则设置为丢弃会使数据包在检查前被丢弃：

![](Images/B31589_02_63.png)

图 2.63：Tap 安全规则

类似于监听端口镜像，防火墙可以将所有未加密的会话数据发送到第三方 `DLP` 或威胁情报设备。它可以通过 Decryption Port Mirror 接口实现。

## Decryption Port Mirror 接口

Decryption Port Mirror 接口允许将解密后的数据包转发到外部设备以进行进一步检查：防火墙将接收设置为解密的 TLS 数据包，因此会执行解密会话所需的所有步骤，然后使用 Decryption Port Mirror 接口作为 egress 接口将明文负载转发给第三方设备进行“第二意见”。例如，这对 DLP 很有用。许可证可通过支持门户免费激活，方法是浏览 [https://support.paloaltonetworks.com](https://support.paloaltonetworks.com) 然后进入 `Assets` | `Devices`。

在那里，您可以找到您的防火墙并单击 `Actions` 按钮。如果您选择激活功能许可证，您将能够激活 `Decryption Port Mirror`：

![](Images/B31589_02_64.png)

图 2.64：激活 Decryption Port Mirror 许可证

要在防火墙上激活许可证，请按照以下步骤操作：

1. 从 `Device` | `Licenses` 中，选择 `Retrieve license keys from license server`。
2. 在 `Device` | `Setup` | `Content ID` | `Content-ID settings` 中，启用 `Allow forwarding of decrypted content`。
3. 在 `Network` | `Interfaces` | `Ethernet` 中，将接口设置为 `Decrypt Mirror` 类型。
4. 在 `Objects` | `Decryption` | `Decryption Profiles` 中，打开解密配置文件并将接口添加到 `Decryption Mirroring`。
5. 在 `Policies` | `Decryption` 中，创建使用解密配置文件的解密规则。
6. 保存更改并连接 Decryption Port Mirror 接口。

除了以太网接口外，还有三种不同的逻辑接口：

* `Loopback`
* `Tunnel`
* `VLAN interface`

现在让我们来看看接口中的瑞士军刀——loopback。

## Loopback 接口

Loopback 接口是一个逻辑三层接口，可用于多种用途。一个常见的用例包括为其自身接口添加额外的公网 IP，以便可以为其添加 VPN 配置。另一个用例是为 loopback 添加管理配置文件，然后利用安全规则允许管理员从异类网络管理防火墙。它需要配置一个 IP 地址（每个 loopback 接口仅支持一个 IP）和一个安全区域，并且需要与 `VR` 关联。

它可以设置为与其中一个三层接口相同子网和区域中的新 IP 地址，因此诸如 `Management Profile`、`Captive Portal` 和 `GlobalProtect` 之类的服务可以托管在与物理接口主 IP 不同的 IP 上。

为了增加额外安全性，它也可以设置为不同的区域，以便客户端能够连接到 loopback 接口时需要匹配的安全规则：

![](Images/B31589_02_65.png)

图 2.65：Loopback 接口

`Interface Name` 旁边的数字是逻辑接口的识别号。可以为接口分配 1 到 9999 之间的数字，并且仅用作接口的识别号。

## Tunnel 接口

Tunnel 接口是逻辑接口，作为隧道流量的入口和出口点，包括站点到站点 VPN 和 GlobalProtect SSL 和 IPSec。物理隧道终止于三层或 loopback 接口，但需要加密的数据包应路由到 tunnel 接口：

![](Images/B31589_02_66.png)

图 2.66：VPN 隧道的静态路由

如以下屏幕截图所示，此接口需要与 `VR` 和安全区域关联：

![](Images/B31589_02_67.png)

图 2.67：Tunnel 接口
