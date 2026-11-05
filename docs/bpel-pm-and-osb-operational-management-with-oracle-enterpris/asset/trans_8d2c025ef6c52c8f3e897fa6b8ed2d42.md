
## SOA Suite 克隆

18. 当状态刷新为 Succeeded 时，克隆操作已完成。在本练习中，这大约需要 21 分钟。

![](img/7529bbd615e859608c21b965162348b2_164_0.png)

## 验证新创建的集群

部署过程完成后，我们需要验证新创建的应用服务器实例。为本练习的目的，我们将验证新的 Oracle Application Server 目标是否出现在 Enterprise Manager 中，然后登录到 BPEL 控制台：

- 1. 导航到 Targets | Middleware 子标签页。
- 2. 新创建的 Oracle Application Server 目标名为 soaCloned.gcsoa.us.oracle.com 会出现在表格中。点击该名称，如以下屏幕截图所示：

![](img/7529bbd615e859608c21b965162348b2_165_0.png)

- 3. 新实例的 Oracle Application Server 主页出现。确保 Components 表中出现的组件与源 Oracle Application Server 目标中存在的组件相同（即 home、HTTP_Server、oc4j_soa、Oracle bpel）。点击 Oracle bpel 组件，如下一屏幕截图所示：

![](img/7529bbd615e859608c21b965162348b2_166_0.png)

- 4. Oracle BPEL Process Manager 主页出现。在页面底部的 Related Links 部分，点击 Administer (BPEL Console)，如以下屏幕截图所示：

![](img/7529bbd615e859608c21b965162348b2_167_0.png)

- 5. 在 BPEL 控制台登录页面，提供以下信息：
**Username:** oc4jadmin
**Password:** welcome1

![](img/7529bbd615e859608c21b965162348b2_168_0.png)

您也可以使用 BPEL Admin 管理域列表：
转到 BPEL Admin

- 6. BPEL 控制台出现，页面上列出了多个 **Deployed BPEL Processes**。确保目标服务器上的这些进程与源服务器上的进程相同。

![](img/7529bbd615e859608c21b965162348b2_168_1.png)

## 摘要

管理软件和服务的生命周期是一项耗时且容易出错的任务。管理员可能花费数天数周来规划和实施配置和修补任务。Grid Control 大大简化了这些复杂任务，使管理员能够专注于更具战略性的领域，企业也能更灵活地管理软件和服务的生命周期。

本章讨论了克隆 Oracle Application Server 和 SOA Suite 的步骤。具体而言，我们讨论了将 Oracle Application Server 集群添加到 Grid Control、停止应用服务器进程、复制应用服务器并将其存储在 Grid Control 软件库中、克隆应用服务器和 SOA Suite，以及最后验证新创建的集群。需要注意的是，Grid Control 会将数据和配置从源集群复制到目标集群。配置和克隆解决方案得到了修补解决方案的补充。新功能包括在 BEA 收购后对 Oracle WebLogic 服务器上的 SOA Suite 的类似覆盖范围，这为管理员管理 WebLogic 服务器上的 SOA 部署提供了便利。

## 10

## Web 应用监控

衡量和管理终端用户的性能变得越来越重要，以确保客户满意度。虽然服务和基础设施在数据中心防火墙内可能健康且可用，但消费者通过互联网外部访问服务时仍可能遇到问题。这就变成了一个猜测游戏，以确定问题是出在客户的互联网服务提供商还是提供商的故障服务上。此外，在 Twitter 和 Facebook 时代，提供商面临分布式拒绝服务类攻击的风险日益增加。如今许多提供商，如 Amazon、Google 和 Salesforce.com，都提供有关其服务当前和历史状态的公开信息。

越来越多的服务等级协议与终端用户使用服务的体验挂钩。如果终端用户无法访问服务，那么在 Twitter 上的一条简单的“tweet”可能导致用户投诉的连锁反应迅速蔓延，很快成为提供商负面宣传的来源。在当今世界，当消费者面临不可用或性能不佳的服务时，提供商无处可藏。在某些情况下，这可能导致基于合同义务的退款请求。

本章讨论：

- 挑战
- 解决方案
- 分步练习：
  - 创建 Web 应用
  - 将 Web 事务记录为 Service Test
  - 添加 Beacons 以运行 Service Test
  - 添加性能和使用指标
  - 查看 Web 事务回放

## 挑战

Web 应用管理员希望更好地了解终端用户所看到的可用性或性能。管理员希望密切关注一系列“始终预期保持活跃和响应”的关键 URL，或从不同区域、域或特定访客组监控 Web 应用的性能，以确保消费者获得最佳性能。了解性能问题影响的能力至关重要。了解受性能问题影响的页面、访客、域和区域也很重要。

## 解决方案

Grid Control 具有事务记录功能，可让您记录模拟终端用户的 Web 事务。Grid Control 跟踪所有访问过您应用的访客的所有 URL 的所有响应时间。Grid Control 不仅可以模拟 Web 流量，还可以模拟 tnsping、IMAP、SQL 和其他几种协议。通过监控点击次数结合性能指标，并按 URL、域、区域、访客和 Web 服务器分析响应时间，您可以清楚地评估性能下降问题对用户群的影响。进一步的下钻可提供 Web 服务器响应时间和负载分布信息，帮助您高效平衡服务器资源。

## 分步练习

这组分步练习将指导您管理 Web 应用。

## 创建 Web 应用

Web 应用是 Grid Control 中的一种特定目标类型，用于监控一个或多个 URL 的可用性、性能和 SLA 报告。

![](img/7529bbd615e859608c21b965162348b2_172_0.png)

请仅在 Internet Explorer 上执行此练习。您需要连接到互联网以便在此练习中下载 Web 事务记录器的 Microsoft 库。

- 1. 从 Grid Control 主页，点击 Targets | Web Applications 选项卡。
- 2. 点击 Add 按钮添加新的 Web 应用，如下一屏幕截图所示。

> Web 应用是 Grid Control 中的一种特殊服务。还有其他服务类型，如通用服务、聚合服务等。

![](img/7529bbd615e859608c21b965162348b2_173_0.png)

- 3. 在 General 步骤中，如以下屏幕截图所示：
  - 将 Web 应用名称输入为 SOA Order Booking Client。
  - 选择 Time Zone 为 Use System Time Zone。
  - 将 Homepage URL 输入为 http://gcsoa.us.oracle.com:7777/soademo。

## Web 应用监控

![](img/7529bbd615e859608c21b965162348b2_174_0.png)

- 点击 **Select System**。
- 在弹出窗口中为 **soa_infra_system** 选择单选按钮。
- 点击 **Select**，如下一屏幕截图所示：

> System 是 Grid Control 中目标的集合。在基于该 System 创建 Web 应用（或其他服务）之前，必须从 Systems 选项卡创建 System。

![](img/7529bbd615e859608c21b965162348b2_174_1.png)

- 查看 System 的关键组件。您可以取消选中 SOA Order Booking Client 不依赖的某些组件。
- 点击 Next，如以下屏幕截图所示：

![](img/7529bbd615e859608c21b965162348b2_175_0.png)

- 4. 在 Availability 步骤中：
  - 选择基于 Service Test 定义可用性。
  - 点击 Next，如下一屏幕截图所示：

> 可用性可基于 Service Test 或 System 定义。此外，可用性可基于上下线状态以及使用指标定义。基于 Service Test 的可用性可定义为一个或多个记录的 Web 事务的状态，由一个或多个代理（Beacons）测试。基于 System 的可用性取决于底层系统组件的状态——根据需求，全部或一个组件可以处于上线状态。
