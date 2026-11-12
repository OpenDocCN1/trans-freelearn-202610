
与使用 **DHCP 拆分作用域** 的高可用性解决方案不同，该解决方案在网络上使用两台活动的 DHCP 服务器。作用域在两台服务器上 *水平拆分*，因此每台服务器都控制同一作用域的一部分。以下图表示此情况：

![](img/cf35f46a8dd5f11e58349c1d81611b75_178_0.png)

主服务器在辅助服务器的 **作用域** 属性中设置了 `Delay` 配置属性；你控制主服务器和辅助服务器。在此 *拆分作用域* 场景中，主 DHCP 服务器响应客户端请求，如果主服务器不可用，则辅助服务器响应客户端请求。

在本节中，我们介绍了实现 DHCP、DHCP 服务器授权以及向客户端分配 IP 地址信息的高可用性。在下一节中，我们将介绍如何管理 DHCP。

## 管理 DHCP

通过 DHCP 控制台和 DHCP PowerShell cmdlet 管理 DHCP。使用 Server Manager 安装 DHCP 服务器角色时，会安装这些工具，或者你可以使用 **远程服务器管理工具 (RSAT)**。

以下屏幕截图显示了 DHCP 控制台：

![](img/cf35f46a8dd5f11e58349c1d81611b75_178_1.png)

图 5.19 – DHCP 控制台

若要使用 Windows Admin Center 管理 DHCP，必须在 DHCP 服务器上安装 DHCP PowerShell cmdlet。如果未安装，将显示一条消息，并提供一个按钮来执行远程安装。

以下 PowerShell cmdlet 用于在安装 DHCP 服务器角色时包含管理工具：

```
Add-WindowsFeature DHCP -IncludeManagementTools
```

## DHCP 组

每台 DHCP 服务器都包含用于 *委派管理* DHCP 的组；这些组如下所示：

- DHCP Administrators 本地组：此组用于本地 DHCP 服务器管理
- DHCP Users 本地组：此组用于浏览配置并提供本地 DHCP 服务器的状态信息

使用 Server Manager 安装方法时，这些组作为安装的一部分创建。使用 PowerShell cmdlet 或 Windows Admin Center 安装 DHCP 角色时，则必须作为额外步骤使用以下 PowerShell *手动添加* 这些组：

```
Add-DhcpServerSecurityGroup -Computer DhcpServerName
```

本节介绍了使用 DHCP 网络服务分配 IP 地址的主题。我们介绍了 DHCP 及其工作原理、如何实现它以及如何管理它。在下一节中，我们将介绍 IPAM 及其用于管理本地 IP 寻址的用途。

## 实现和管理 IPAM

本节将介绍使用 IPAM 功能管理本地 IP 寻址。

在本节中，我们将首先介绍 *什么是 IPAM？*

### 什么是 IPAM，它如何工作？

IPAM 是一个框架，用于为 IPv4 和 IPv6 地址空间提供可见性和控制、规划、分配和管理，以及对组织的 DHCP 和 DNS 服务器进行集中控制和管理。

IPAM 在 AD DS 上下文中工作，是一种感知域服务的功能，因为 IPAM 服务器必须加入域，而不是独立的 *工作组* 服务器。

IPAM 作为 Windows Server 内置的一组工具提供，可以通过客户端/服务器模型实现，如下所示：

- IPAM 服务器：提供托管服务器数据收集。它使用 **Windows 内部数据库 (WID)** 或 SQL Server 数据库，并通过 **基于角色的访问控制 (RBAC)** 提供访问控制。

- IPAM 客户端：为 IPAM 服务器提供前端客户端界面。使用 Windows PowerShell cmdlet，它与托管服务器交互。它可以安装在 *客户端* 或 *服务器* 操作系统上。

当在带有 *桌面体验* 的 Windows Server 上安装 IPAM 角色时，IPAM 客户端会自动在同一服务器上本地安装。如果使用 *服务器核心版*，则另一台服务器或客户端必须从 IPAM 服务器远程安装客户端。RSAT: IPAM 客户端功能用于在服务器或客户端操作系统上安装客户端。

IPAM 包含四个模块；如下所示：

- IPAM 发现模块：用于发现，将所有已定位的安装了 DNS 或 DHCP 角色的服务器（包括域控制器）添加到数据库中；服务器也可以手动添加。

- IP 地址空间管理模块：用于检查、管理和监控 IP 地址空间。IP 地址可以动态或手动分配给客户端。可以跟踪地址空间利用率，并检测重叠的 DHCP 作用域。

- DHCP 和 DNS 多服务器管理和监控模块：用于监控和管理跨多个已部署 DHCP 和 DNS 服务器的任务，并提供 DNS 运行状况。

- 操作审计和 IP 地址跟踪模块：用于管理和监控配置更改。从托管的 DHCP 服务器收集租约信息，并从 AD DS 域控制器和 **网络策略服务器 (NPS)** 服务器收集登录信息和数据。

这四个模块允许从 *单窗格视图* (SPOG) 控制台执行全面的 DHCP 服务器、DNS 区域和 IP 地址管理任务，以实现更高的效率和控制。

在本节中，我们介绍了什么是 IPAM 以及它如何工作。在下一节中，我们将介绍如何实现 IPAM。

## 实现 IPAM

数据收集有两种拓扑选择：*集中* 拓扑或 *分散* 拓扑。

### 集中拓扑

在此拓扑中，在所有位置部署单个 IPAM 服务器。当存在双向林信任时，这使用单个控制台视图在林和多个林之间提供 IP 地址空间的可见性、控制和管理任务。

集中拓扑在以下图中表示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_181_0.png)

图 5.20 – IPAM 集中拓扑

IPAM 可以通过带有 RSAT: IPAM 客户端功能的另一台计算机的客户端或 Server Manager 进行管理；这可以是 Windows Server 操作系统或客户端操作系统。

### 分散拓扑

在此拓扑中，在每个位置部署 IPAM 服务器。在处理大规模地址空间管理且需要扩展并交接给分布式本地服务器时，这很典型。当希望向业务单位 (BU) 或单个位置授予自治权时，也可以使用此拓扑。

分布式拓扑在以下图中表示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_181_1.png)

图 5.21 – IPAM 集中拓扑

此外，可以实现 *混合拓扑*。在此拓扑中，IPAM 服务器在每个站点作为本地 IPAM 服务器部署以监控本地服务器，集中 IPAM 服务器可以远程监控托管服务器。应考虑，在此拓扑中，IPAM 服务器不会将本地服务器的数据收集信息复制到中央服务器；每台服务器都是独立的数据点，即一个 *孤岛*。

部署时应考虑以下要求：

- IPAM 角色应是服务器上的唯一角色，且不执行其他功能
- IPAM 服务器必须 *加入域*，并且必须使用 *域帐户* 登录才能安装该角色
- 不支持在 *AD DS 域控制器* 上安装 IPAM 角色
- 如果 IPAM 服务器角色安装在 DHCP 服务器上，则无法发现其他 DHCP 服务器
- IPAM 服务器需要 WID 或 SQL Server 实例；不能在同一服务器上运行另一个 **结构化查询语言 (SQL)** 实例
- 单个 IPAM 服务器可支持 500 个 DNS 服务器、150 个 DNS 区域、150 个 DHCP 服务器和 6,000 个 DHCP 作用域
- 应规划所需存储空间；存储 10,000 个客户端的利用率数据大约需要 1 GB 空间
- 不检查路由器和交换机的 IP 地址一致性

在满足先前要求后，IPAM 服务器的高级别实现步骤概述如下：

- 根据所选拓扑和域加入创建用于 IPAM 的服务器。
- 从 Server Manager、Windows Admin Center 或 PowerShell 安装 IPAM 服务器功能，使用以下命令：
  `Install-WindowsFeature IPAM -IncludeManagementTools`
- 使用以下 PowerShell 命令配置 IPAM 服务器和所需 **组策略对象 (GPO)**：
  `Invoke-IpamGpoProvisioning`
- 配置并运行 DNS 和 DHCP 服务器发现。
- 选择并管理发现的服务器。

关于 IPAM 客户端，可以使用 RSAT 的 IPAM 客户端功能在 Windows Server 或客户端操作系统上安装它。
