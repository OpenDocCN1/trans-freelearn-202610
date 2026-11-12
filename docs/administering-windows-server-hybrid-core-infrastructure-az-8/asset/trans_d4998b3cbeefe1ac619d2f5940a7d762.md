

## 进一步阅读

本节提供用于进一步学习参考和考试信息的链接：

- [微软认证：Windows Server 混合管理员助理](https://docs.microsoft.com/en-us/learn/certifications/windows-server-hybrid-administrator/)
- [考试 AZ-800：管理 Windows Server 混合核心基础结构](https://docs.microsoft.com/en-us/learn/certifications/exams/az-800)
- [考试 AZ-800：技能大纲](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWKI0r)
- [Microsoft Learn](https://docs.microsoft.com/en-us/learn/paths/manage-windows-servers-workloads-hybrid-environment/)

## 技能检查

挑战一下你在本章中学到的内容：

- 1. 什么是 WAC 网关服务器？
- 2. WAC 的工作原理是什么？
- 3. WAC 部署需要允许哪些端口？
- 4. 如何访问 WAC？
- 5. 列出在 Azure 门户中实现 WAC 的要求。
- 6. 什么是 PowerShell 远程处理？
- 7. 如何启用 PowerShell 远程处理？
- 8. 什么是第二跃点 PowerShell 远程处理？
- 9. 什么是 JEA？
- 10. JEA 的要求是什么？

## 13 使用 Azure 服务管理 Windows 服务器

在上一章中，我们学习了在混合环境中管理 Windows 服务器的技能。

本章涵盖 AZ-800 管理 Windows Server 混合核心基础结构：在混合环境中管理 Windows 服务器和工作负载的内容。

我们将介绍管理 Azure 托管的 Windows Server 虚拟机 (VM) 的访问权限、使用 Azure Arc 管理本地 Windows 服务器，并介绍 Azure Automation 的功能。随后，我们将探讨使用 Microsoft Defender for Cloud、Azure Policy 和 Azure Sentinel 进行安全和治理。

最后，我们将通过一个实践练习来进一步提升您的技能。

此内容不旨在涵盖所有方面；内容范围仅限于考试目标，将涵盖以下主题：

- 管理 Azure 中 Windows Server VM 的访问权限
- 为混合 Windows 服务器实现 Azure Arc
- 为混合 Windows 服务器实现 Azure Automation
- 为混合 Windows 服务器实现 Microsoft Defender for Cloud
- 实践练习

除了本章列出的主题之外，本章的目标是让您的知识超越考试目标，为从事现实世界中日常混合环境相关的角色做好准备。

## 管理 Azure 中 Windows Server VM 的访问权限

本节探讨 Windows Server Azure 基础设施即服务 (IaaS) VM 的用户访问控制和网络访问方法。

### 用户访问

出于管理目的，对 Azure IaaS VM 的用户访问通过 Azure 基于角色的访问控制 (Azure RBAC) 进行控制。

以下是内置的默认计算类别角色：

- **虚拟机管理员登录**：这只能让您管理 VM。您无法访问 VM、虚拟网络以及它们连接的任何存储帐户
- **虚拟机参与者**：这允许您在门户中查看 VM 并以普通用户身份登录。
- **虚拟机用户登录**：这允许您在门户中查看 VM 并以管理员身份登录。

这些角色通过 VM 资源的访问控制 (IAM) 边栏选项卡进行管理；以下截图显示了 Azure 门户中显示的计算类别角色：

![](img/cf35f46a8dd5f11e58349c1d81611b75_416_0.png)

图 13.1 - 访问控制边栏选项卡

可以使用 PowerShell Get-AzRoleDefinition cmdlet 列出所有角色：

```
Get-AzRoleDefinition | FT Name, Description | sort
```

以下截图示例显示角色分配列表，以表格形式显示名称和描述列，并按字母顺序列出：

| 角色 | 描述 |
| --- | --- |
| 虚拟机管理员登录 | 在门户中查看虚拟机并以管理员身份登录 |
| 虚拟机参与者 | 允许您管理虚拟机，但不能访问它们，也不能访问 |
| 虚拟机本地用户登录 | 在门户中查看虚拟机并以本地用户身份登录 configu |
| 虚拟机用户登录 | 在门户中查看虚拟机并以普通用户身份登录。 |
| VM 扫描操作员 | 提供对磁盘快照的访问权限以进行安全分析的角色。 |
| Web 计划参与者 | 允许您管理网站的 Web 计划，但不能访问它们。 |
| Web PubSub 服务所有者(预览) | 对 Azure Web PubSub Service REST API 的完全访问权限 |
| Web PubSub 服务读取者(预览) | 对 Azure Web PubSub Service REST API 的只读访问权限 |
| 网站参与者 | 允许您管理网站(不包括 Web 计划)，但不能访问它们。 |
| Windows 管理中心管理员登录 | 允许您通过 Windows 管理中心管理资源的操作系统 as |
| 工作簿参与者 | 可以保存共享工作簿。 |
| 工作簿读取者 | 可以读取工作簿。 |
| WorkloadBuilder 迁移代理角色 | WorkloadBuilder 迁移代理角色。 |

图 13-2 – 角色定义

还可以使用以下 Azure CLI 命令作为 PowerShell 的替代方案：

```
az role definition create --role-definition
```

可以使用 PowerShell Get-AzRoleDefinition <role_name> 命令列出特定角色的详细信息：

```
Get-AzRoleDefinition "Virtual Machine Administrator Login"
```

以下是截图示例：

![](img/cf35f46a8dd5f11e58349c1d81611b75_417_0.png)

图 13-3 – 角色信息

如果我们返回 Azure 门户视图并单击其中一个角色，将看到授予该角色的权限详细信息，如以下截图所示：

| 类型 | 权限 | 描述 |
| :--- | :--- | :--- |
| Microsoft.Compute | | |
| 读取 | 获取虚拟机 | 获取虚拟机的属性 |
| 读取 | 汇总最新的修补程序评估操作结果 | 检索最新修补程序评估操作的摘要 |
| 读取 | 列出修补程序评估操作中评估的所有修补程序 | 检索上次修补程序评估操作中评估的修补程序列表 |
| 读取 | 汇总最新的修补程序安装操作结果 | 检索最新修补程序安装操作的摘要 |
| 读取 | 列出修补程序安装操作中考虑的所有修补程序 | 检索上次修补程序安装操作中尝试安装的修补程序列表 |
| 读取 | 获取虚拟机日志定义 | 获取虚拟机的可用日志。 |
| 读取 | 读取诊断设置 | 获取虚拟机的诊断设置。 |
| 读取 | 查看虚拟机扩展 | 获取虚拟机扩展的属性 |
| 读取 | 获取虚拟机实例视图 | 获取虚拟机及其资源的详细运行时状态 |
| 读取 | 获取虚拟机度量定义 | 读取虚拟机度量定义 |
| 读取 | 获取虚拟机运行命令 | 获取虚拟机运行命令的属性 |
| 读取 | 列出可用的虚拟机大小 | 列出虚拟机可更新到的可用大小 |
| Microsoft.Network | | |
| 读取 | 获取负载均衡器 | 获取负载均衡器定义 |

图 13.4 – 角色权限

可以使用以下 cmdlet 列出角色定义的权限，如以下截图示例所示：

```
(Get-AzRoleDefinition "Virtual Machine Contributor").Actions
```

输出如以下截图示例所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_419_0.png)

图 13.5 – 角色权限（续）

返回 Azure 门户视图，我们还可以看到角色的**分配**详细信息，如以下截图所示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_419_1.png)

图 13.6 – 角色分配

可以使用 Cloud Shell 中的 PowerShell 通过 Get-AzRoleAssignment cmdlet 列出角色分配。

以下是列出用户角色分配的示例：

```
Get-AzRoleAssignment -SignInName smiles@milesbettersolutions.com
```

前面的代码在以下图中表示：

![](img/cf35f46a8dd5f11e58349c1d81611b75_420_0.png)

图 13.7 – 用户角色分配

还可以通过使用 -Scope 参数在资源级别列出用户分配；以下是一个示例：

```
Get-AzRoleAssignment -Scope /subscriptions/8123c407-998a-4ace-afed-360e1839938a/resourceGroups/sm-49-az800-rg/providers/Microsoft.Compute/virtualMachines/sm-49-az800-vm1
```

以下是输出：

![](img/cf35f46a8dd5f11e58349c1d81611b75_420_1.png)

图 13.8 – 资源角色分配

