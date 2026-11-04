
-   **Technique description**: 每种技术都在 MITRE Techniques 页面上有所描述。例如，T1112 技术允许攻击者操纵 **Windows 注册表**。他们可以删除、添加和修改注册表中的信息以实现持久化或执行恶意文件。
-   **Procedures used**: 攻击者（*APT19* 组织）使用的方法包括例如使用 SSH 端口 22 的恶意软件变种来操纵注册表键。然而，T1112 控制注册表还有其他几种方法，包括可以修改注册表的 **Cobalt Strike**。
-   **Mitigation approach**: 限制对注册表配置单元的访问，确保只有授权用户可以修改注册表键和文件。
-   **Detection techniques**: 将分析与 SOC 系统集成，以监控并记录注册表配置单元的任何更改。MITRE ATT&CK 框架提供了良好的检测和参考，用于跟踪注册表的更改并防范 *APT19* 组织的 T1112 技术。
-   **Software that can be used to conduct the attack**: **Cobalt Strike** 和 **Empire**。这些工具使用不同的技术来发起攻击。技术包括 **Abusive Elevation Control Mechanism**、**Access Token Manipulation**、**Account Discovery**、**Application Layer Protocol**、**System process modification** 等。

威胁情报团队可以研究这些组织并与防御团队共享信息以加强组织的安全性。此步骤必须针对所有威胁行为者及其技术执行（必须将威胁搜索限制在组织的范围内）。

Level 1 组织可以利用 MITRE ATT&CK 框架来分析针对相同范围内的组织所使用的不同威胁 TTP，并将输出与团队其他成员共享以创建预防性防御系统。下一节将介绍 Level 2 组织如何将威胁情报集成到业务中。

## 案例研究 2 – Level 2 组织的 CTI

Level 2 组织是指拥有成熟威胁情报计划的组织。它们已通过 Level 1 阶段，并准备将 CTI 纳入其文化。我们假设 Level 2 组织已建立 CTI 团队，定期监控威胁组织。

### 目标

Level 2 组织的信息情报目标是将情报分析 *self-map* 到特定的框架模型（如 MITRE ATT&CK 或 Cyber Kill Chain 模型）。通过利用威胁情报框架可以轻松实现这些目标。

### 策略

Level 2 组织可以通过遵循以下战略步骤集成 CTI 程序：

-   执行前一节的 Level 1 流程以了解对手的技术并收集内部日志和其他外部数据。
-   将分析输出（或报告）映射到所选框架。例如，要将安全报告映射到 MITRE ATT&CK，CTI 团队必须研究并理解对手的行为，将该行为链接到 ATT&CK 战术，并识别该特定战术中使用的 ATT&CK 技术。
-   将输出结果（情报分析到情报框架的映射）与社区共享，并将流程结果与其他威胁情报专业人员进行比较。

### 示例

以下是如何 Level 2 组织可以集成安全情报的示例：

-   **Description**: 假设在 Level 1 流程期间以及检查内部网络流量时，安全团队发现系统到外部元素的异常远程连接——可能的攻击。
-   **Study the behavior**: 该攻击创建到对手域的 **SOCKS connection**。SOCKS 连接在网络攻击中广泛存在，允许对手连接到受害者系统并执行恶意操作。因此，它使对手能够远程执行命令。
-   **Convert the behavior to a tactic**: 从行为研究中，我们可以将攻击链接到 MITRE ATT&CK 框架中的一种战术。在这种情况下，最合适的是 C2 战术。
-   **Map the tactic to the technique**: 在特定战术下的技术列表中搜索，以将威胁行为者行为映射到一个或多个 ATT&CK 技术。查看 MITRE C2 战术，我们可以看到 *Socket* 和 *Secure* 属于 **Non-Application Layer Protocol Technique**，即 *T1095*。这种映射使威胁情报团队能够创建威胁和对手行为的数据库。
-   **Share the result**: 将结果与公司其他部门和情报社区共享，以丰富通用框架。共享分析结果还允许 Cobalt Strike CTI 框架通过社区贡献而发展。

中型企业需要将信息映射到框架模型以促进未来的威胁分析并最大限度地减少检测使用与现有已识别和记录的威胁相同的 TTP 的潜在威胁所需的时间。具有深入业务知识的分析师应执行威胁和攻击的映射。映射后的数据随后可以与防御团队共享以实施保护计划。下一节将介绍 Level 3 组织如何从威胁情报计划中受益。

## 案例研究 3 – Level 3 组织的 CTI

Level 3 组织拥有先进的安全情报能力并具备足够的<|message|> 安全基础设施，拥有更多资源。Levels 1 和 2 被假定为先进组织业务系统的一部分。目标和重点将与 level 1 和 2 组织不同。先进组织拥有更多信息，内部和外部，因此数据来源更多。数据来源在 *Chapter 7, Threat Intelligence Data Sources.* 中有所介绍。

### 目标

Level 3 组织的信息情报目标是威胁和防御技术的优先级排序。在将不同信息（IOCs、TTPs、报告、外部情报等）映射到框架模型之后，先进组织必须根据其对系统的影响来优先考虑 TTP。

### 策略

通过遵循以下战略步骤，Level 3 组织可以将安全情报集成到其业务运营中：

-   **Identify the organization's top threats**: 这是在执行威胁建模和对对手和组织的有效侦察后实现的。
-   **Map the threats to the framework model**: 使用 MITRE ATT&CK 框架，分析人员必须将所有信息映射到 ATT&CK 框架。
-   **Prioritize the threats with a high impact on the system**: 基于情报和业务需求，CTI 团队与内部安全团队和其他战略领导者一起可以建立对手技术排名系统，以识别最危险的对手技术。**ATT&CK Navigator** (`https://github.com/mitre-attack/attack-navigator`) 可用于此目的。
-   **Aggregate the attacks by technique to facilitate the prioritization process**: 这也将为防御团队提供良好的输入，以了解哪些威胁必须首先关注。或者，将利益相关者和内部安全团队提供的业务和情报需求与分析结果进行匹配，以识别组织最关心的威胁。

### 示例

以下是如何 Level 3 组织可以使用 CTI 与 MITRE ATT&CK 框架来优先考虑威胁的示例：

-   **Top threats**: 在情报分析期间，CTI 团队关注 *APT19* 和 *APT3*，因为它们直接影响组织业务。
-   **Map the threat to the model**: 使用 ATT&CK 框架，我们识别 *APT19* 和 *APT3* 使用的技术。此信息可以直接从 MITRE 网站提取。
-   **Identify techniques with a high impact**: 我们使用 attack-navigator 创建两个组织的技术矩阵，给 *APT19* 和 *APT3* 分别打分为 *1* 和 *2*。
-   **Aggregate and prioritize the techniques**: 使用 ATT&CK Navigator，我们合并技术以识别两个高优先级威胁（*APT19* 和 *APT3*）共同使用的方法。然后我们优先考虑共同技术，如下图所示以绿色显示：

