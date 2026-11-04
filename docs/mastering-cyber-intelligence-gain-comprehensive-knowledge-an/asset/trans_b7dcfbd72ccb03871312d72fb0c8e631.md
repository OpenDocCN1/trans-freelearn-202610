
-   **威胁情报：** 大多数先进的 SIEM 工具提供集成威胁情报的能力，了解过去和现在的威胁、活动、对手及其 TTP，以便有效保护系统。SIEM 中的威胁情报可促进自动响应和主动防御。
-   **威胁狩猎：** 现代 SIEM 系统不仅集成威胁情报，还集成威胁狩猎，使组织能够领先于威胁和对手。
-   **SOC 自动化：** 可能是任何 SIEM 系统的初始关键要素，SOC 自动化对于安全运营至关重要。SIEM 工具会生成安全警报、事件和异常行为，供分析师用于日常安全运营。例如，当 SIEM 工具发现并发出入侵警报时，取证和事件响应团队会立即收到通知。SOC 自动化可以减少采取安全措施所需的时间。SIEM 工具上适当的 **入侵指标 (IOCs)** 和事件优先级在 SOC 中至关重要。

SIEM 系统可以具有其他组件，并且可以与其他安全功能接口，例如恶意软件逆向工程和分析团队、网络防御和渗透测试团队，以及在需要时解决方案架构团队。

> **注意 – SIEM 解决方案选择与采用**
> 现代 SIEM 工具对中小企业来说可能很昂贵。选择对业务运营和强大的安全态势至关重要的组件至关重要。例如，一级组织可以利用具有最少但有效的组件的开源 SIEM 工具。他们可以专注于内部数据收集和一些 OSINT 源——成本极低或无需成本的任务。然后他们可以利用开源 BI 工具进行仪表板和基础分析。后者可用于流和事件优先级排序以及通知，以支持 SOC 活动。二级组织可以根据预算利用功能更强大的开源或商业 SIEM 工具。拥有成熟 CTI 程序和威胁狩猎能力的组织可以部署完整的 SIEM 系统。

SIEM 系统对 SOC 和其他安全功能很重要。因此，建议希望集中管理安全事件和日志的组织采用。下一小节我们将介绍一个实用的 SIEM 用例。

## SIEM 安全应用 – OTX 与 OSSIM 用例

SIEM 系统可以部署在本地、云端，或作为服务由厂商或提供商外包。虽然存在优秀的商业 SIEM 工具，但也有小型企业可以利用的开源 SIEM 工具。其中之一就是 AlienVault OSSIM with OTX。本小节介绍 AlienVault 开源安全信息和事件管理 OSSIM (https://cybersecurity.att.com/products/ossim)。

### AlienVault Open Threat Exchange (OTX)

AlienVault OTX 是 AT&T Cybersecurity (https://otx.alienvault.com) 的开源威胁情报交换平台。OTX 提供构建威胁情报的服务。以下步骤提供了使用 OTX 创建情报的实用指南：

-   1. 在 OTX 网页上注册并登录以激活您的个人资料：https://otx.alienvault.com。
-   2. 打开仪表板视图，如下图所示。在仪表板中，我们可以可视化包含沙箱警报和报告计数的恶意软件集群。我们还可以看到已订阅的脉冲、安全新闻和事件，以及顶级社区贡献者。

![](img/d47f26598025b2da017dd41fcbb6e602_381_0.png)

图 12.2 – 显示恶意软件集群和情报脉冲的 OTX 仪表板页面

-   3. 我们可以按类别查看恶意软件并选择特定恶意软件以获取详细信息。
-   4. 点击与您想要调查的特定恶意软件家族相关的任何脉冲。**pulse details** 页面打开，包含 *description, reference, group, targeted industry,* 和 *MITRE ATT&CK ID*，将威胁链接到不同的 MITRE ATT&CK 技术。如下图所示：

![](img/d47f26598025b2da017dd41fcbb6e602_382_0.png)

图 12.3 – OTX 脉冲描述和信息说明

-   5. 前述脉冲报告了一个针对政府和企业实体的威胁。您可以查看所有 IOC 及其类型。**TYPES OF INDICATORS** 条显示与该威胁关联的每个 IOC 的不同计数，如下图所示：

![](img/d47f26598025b2da017dd41fcbb6e602_382_1.png)

图 12.4 – OTX IOC 图形视图

-   6. 您可以通过点击 **Add to group** | **Create New Group** 或 **Browse a group to join** 将脉冲添加到新的监控组。您可以以 STIX、OpenIOC 或 CSV 等格式下载脉冲。其他活动包括将脉冲嵌入您的网站或克隆脉冲。
-   7. 使用 YARA 规则，您还可以将脉冲添加到 OTX 配置文件。将脉冲添加到 OTX 的内容在 *Chapter 15, Threat Intelligence Sharing and Cyber Activity Attribution - Practical Use Cases* 中介绍

以下示例展示了从 OTX 下载的基本威胁脉冲 STIX 数据：

```
{
    "contact_information": "https://otx.alienvault.com/",
    "created": "2021-12-07T17:02:42.599Z",
    "id": "identity--ab072f15-9b87-4ee1-898f-b584d41f29b0",
    "identity_class": "organization",
    "modified": "2021-12-07T17:02:42.599Z",
    "name": "Open Threat Exchange",
    "type": "identity"
},
{
    "created": "2021-12-07T17:02:44.000Z",
    "description": " / CC=US ASN=AS1239 SPRINTLINK",
    "id": "indicator--3f52e4c7-67cc-4f05-8704-35f85609297f",
    "labels": [
        "scanning_host"
    ],
    "modified": "2021-12-07T17:02:44.000Z",
    "name": "OTX pulse_name=Suspected Russian Activity Targeting Government and Business Entities Around the Globe",
    "pattern": "[ipv4-addr:value = '63.162.179.166']",
    "type": "indicator",
    "valid_from": "2021-12-07T17:02:44.000Z",
    "valid_until": "2022-01-06T00:00:00.000Z"
},
{
    "created": "2021-12-07T17:02:44.000Z",
    "description": "",
    "id": "indicator--121ec8b4-ec70-4da4-95df-a8073204ef0d",
    "labels": [],
    "modified": "2021-12-07T17:02:44.000Z",
    "name": "OTX pulse_name=Suspected Russian Activity Targeting Government and Business Entities Around the Globe",
    "pattern": "[file:hashes.MD5 = '1d3e2742e92264b7063db8cafed6531']",
"type": "indicator",
"valid_from": "2021-12-07T17:02:44.000Z"
},
...
```

我们可以查看脉冲的详细信息。如果您已有 SIEM 工具，OTX 还可以通过直接连接到 OTX 服务或解析脉冲到其中，作为情报数据源集成。

OTX 是开源的，任何威胁情报分析师都可以使用它下载对手的 IOC，以加强安全防御。例如，与前述脉冲关联的 22 个 IOC 可以添加到 SIEM 规则、IDS 和 IPS 中，以隔离和阻止恶意流量。

### AlienVault OSSIM

OSSIM 是开源 SIEM，可以从 AT&T Cybersecurity 网站 (https://cybersecurity.att.com/products/ossim) 下载。ISO 可以安装在 VirtualBox 或 VMware 等虚拟环境中。OSSIM 可以帮助组织执行以下任务：

-   **资产发现：** 该模块识别组织资产和端口。您可以对系统资产和端口执行主动和被动扫描。资产清单和扫描结果会被保留。
-   **漏洞评估：** 该工具使用集成的 OpenVAS 扫描器识别漏洞。它允许您扫描系统（主机、网络和应用程序）以查找漏洞。
-   **威胁检测：** OSSIM 集成 IDS（网络和主机入侵检测）功能。这意味着它可用于检测威胁并添加规则来管理流量。它利用 Snort 等开源 IDS/IPS 工具进行流量异常检测。它还提供文件完整性监控，以了解文件是否已被修改、删除或创建。
-   **行为分析：** OSSIM 可以帮助识别系统行为的变化，例如带宽使用量的激增，这可能是由恶意软件引起的。OSSIM 集成 NetFlow。
-   **安全情报：** 核心处理模块。它可以关联来自不同来源的数据。该模块是 AlienVault 商业 SIEM USM 与开源 SIEM OSSIM 之间的主要区别。后者的许可证有限（大约少于 100 条规则）。

