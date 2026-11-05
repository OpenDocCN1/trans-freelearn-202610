
## 9

## SOA Suite 克隆

软件维护通常占总体成本的 70%，其中包括许可费用。企业必须担心软件维护工作，其中包括产品更新、安全更新、硬件更新和环境刷新。这些都是耗时的工作，需要在多个领域具备人力专业知识。随着软件老化，企业仍需为遗留软件维持这种人力专业知识，而遗留软件虽然对业务至关重要，但可能代价高昂，有时还会通过增加拥有成本来抵消整体业务价值。

越来越多的企业转向自动化生命周期管理工具，以帮助完成与软件生命周期维护相关的繁重工作。制定维护计划时拥有正确的策略非常重要。维护应是整体策略的一部分，而非事后考虑。

本章涵盖：
- 挑战
- 解决方案
- 分步练习：
  - 将 Oracle Application Server 集群添加到 Grid Control
  - 停止应用服务器进程
  - 在 Grid Control 软件库中创建组件
  - 克隆 Oracle Application Server 和 SOA 应用
  - 验证新创建的集群

### 挑战

在当今的数据中心，服务通常需要在短时间内进行配置。这些服务可能由复杂的分层配置组成，例如 Oracle Application Server 和 SOA 应用软件。管理员必须经过多个步骤才能将 SOA Suite 从一个安装克隆到另一个安装。这些步骤包括克隆复制的文件、配置它们，并进行更改以反映新的安装。一些 Oracle 管理员具备数据库克隆技能，但大多数人并不具备克隆应用服务器的专业知识，更不用说 SOA Suite 了。这种服务的手动安装通常耗时且容易出错。

### 解决方案

Grid Control 提供了将测试过的镜像克隆到多个主机的灵活且可扩展的方式。具体来说，Grid Control 提供了一个预定义的、完全可定制的用于克隆 Oracle Application Server 和 SOA 应用的流程。该流程使管理员能够基于现有的参考应用服务器或集群创建新的 Oracle Application Server 实例或 Oracle Application Server 集群；从而自动化常见的 IT 任务，例如基于测试或暂存环境创建生产 SOA Suite 环境，或扩展现有的 Oracle Application Server 集群以适应负载增加。

### 分步练习

本套分步练习将演示如何克隆部署在 Oracle Application Server 10.1.3.x 上的 Oracle SOA Suite 10.1.3.x。

### 将 Oracle Application Server 集群添加到 Grid Control

第一步是创建应用服务器集群目标，以便以后进行克隆。

![](img/7529bbd615e859608c21b965162348b2_138_0.png)

> 为了通过克隆创建新集群，Grid Control 必须知道一个已安装的现有集群以供克隆。

1. 在 Grid Control 主页上，单击 Targets | Middleware 选项卡。

![](img/7529bbd615e859608c21b965162348b2_139_0.png)

在表中，您应该至少看到两个 Oracle Application Server 目标：一个托管 Grid Control 应用，另一个托管 SOA Suite，如前几章所述。虽然托管 SOA Suite 的 Oracle Application Server 实例已在 Enterprise Manager 中被发现，但它所属的集群尚未被发现。

2. 从 Add 下拉菜单中选择 Oracle Application Server Cluster 并单击 Go，如以下屏幕截图所示：

![](img/7529bbd615e859608c21b965162348b2_139_1.png)

3. 出现一个提示输入现有集群信息的页面。为集群提供一个名称，例如 Cluster 10131。

4. 要指定集群成员的应用服务器实例，请单击手电筒图标。

![](img/7529bbd615e859608c21b965162348b2_140_0.png)

5. 在 Search and Select: Targets 页面上，选择集群成员的应用服务器实例的名称并单击 Select 按钮，如以下屏幕截图所示：

![](img/7529bbd615e859608c21b965162348b2_140_1.png)

6. Add Oracle Application Server Cluster 页面上的剩余字段将根据您的选择自动填充，如下一屏幕截图所示。单击 OK。

### 添加 Oracle Application Server 集群

为了将 Oracle Application Server 集群添加到 Grid Control 以进行监控和集中管理，您必须指定要添加的 Oracle Application Server 集群成员的 Oracle Application Server（版本 10.1.3 或更高）实例的名称。

Oracle Application Server 集群名称 Cluster 10.1.3.1
应用服务器名称 soa.gcsoa.us.oracle.com
OPMN 请求端口 6003
应用服务器控制 URL http://gcsoa.us.oracle.com:7777/em

提示 应用服务器实例名称及其 OPMN 请求端口用于联系应用服务器，并查询其 Oracle Application Server 集群成员资格。根据所选的应用服务器实例，Enterprise Manager 指定 OPMN 请求端口的默认值。如果 Enterprise Manager 指定的端口值不正确，则请查看应用服务器的 opmn.xml 文件（即 $ORACLE_HOME/opmn/conf/opmn.xml）以获取正确的值。

7. 屏幕顶部出现确认消息，确认集群已成功添加到 Enterprise Manager，并且 Cluster 10.1.3.1 作为新目标出现在表中，其中 soa.gcsoa.us.oracle.com 应用服务器显示为该集群的成员，如以下屏幕截图所示：

### 确认

Oracle Application Server 集群 Cluster 10.1.3.1 已成功添加，现在正由 Grid Control 进行监控。

### 应用服务器

| 选择名称 | 类型 | 状态 | 状态详情 | 警告 | 策略违规 | CPU 使用率 (%) | 内存使用量 (MB) |
|---|---|---|---|---|---|---|---|
| 所有应用服务器 |  |  |  |  |  |  |  |
| Cluster 10.1.3.1 | Oracle Application Server 集群 | n/a | 4 (↑4) | 0 0 | 0 0 0 |  |  |
| soa.gcsoa.us.oracle.com | Oracle Application Server |  | 4 (↑4) | 0 0 | 0 0 0 | 4.15 | 88.39 |
| EnterpriseManager() gcsoa.us.oracle.com | Oracle Application Server |  | 5 (↑5) | 0 0 | 0 0 0 | 3.87 | 964.18 |

提示 有关本页所用图标和符号的说明，请参阅 Icon Key。

### 相关链接

自定义表列

> 现在 Oracle Application Server 集群已添加到 Grid Control，您可以使用该集群作为执行各种操作的参考或源。为本次练习的目的，我们将使用已发现的集群在 Grid Control 软件库中创建“gold image”组件，然后使用库中的 gold image 组件创建新的 Oracle Application Server 集群目标。

### 停止应用服务器进程

建议停止应用服务器进程以便复制数据和配置。

> 在创建 gold image 组件或通过克隆创建新集群之前，您应先停止机器上已运行的现有已安装应用服务器进程。

要停止 Oracle Application Server 进程，请按照以下步骤操作：
- 1. 在 Linux 或 Windows 机器上以应用服务器所有者身份登录（例如，oracle）。
- 2. 导航到 bin 目录，即 `cd /u01/app/oracle/product/OracleHomes/soa/opmn/bin`。
- 3. 使用以下命令停止应用服务器进程：`./opmnctl stopall`。
- 4. 从应用服务器主页，您可以验证实例的所有组件是否已停止。

> 如果应用服务器实例状态仍显示为 Up，请单击页面上的 Refresh 图标。
