

IBM 在其云平台的 Cloud Pak for Data SaaS 方案中采用了复杂的 multi-tenancy 技术，并且 Cloud Pak for Data 软件栈的架构基础也能够使企业在同一个 OpenShift Kubernetes 集群上支持多个租户。第 9 章《Technical Overview, Management, and Administration》介绍了如何通过审慎使用 Kubernetes 命名空间来实现多租户，即单个 OpenShift 集群在多个租户之间共享。在安全可靠地实施多租户时，需要仔细权衡，同时要兼顾企业的需求、策略以及租户所期望的服务保证。

在本章中，我们将涵盖以下主题：

- Tenancy considerations
- Architecting for multi-tenancy
- A review of the tenancy requirements
- In-namespace sub-tenancy with looser isolation

## Tenancy considerations

我们将首先仔细审视多租户意味着什么、租户为何重要，以及总体而言，在规划此类共享部署时需要做出哪些架构选择、权衡和关于费用的妥协。

### Designating tenants

每个企业在识别其租户时通常有非常不同的理解，但总体而言，租户被定义为被授予特定权限以访问共享软件安装的选定用户组。当租户代表完全不同的公司时，一个租户的用户通常不知道其他租户用户的存在。然而，即使这种隔离也可以在同一企业内部的用户之间放宽，公司内的员工可能是多个租户“账户”的成员。

在每个租户范围内，可以授予特定用户管理权限并将其范围限定在其实例中，以使租户具备一定的自助服务能力。这允许租户管理员执行部分 Cloud Pak for Data 管理功能，包括自行管理用户访问和故障排除，如第 9 章《Technical Overview, Management, and Administration》所述。

> **A note on the role of tenant users**
>
> 包括这个 Tenant Admin 角色在内，所有来自租户组的用户都应被视为终端用户；即专注于使用 Cloud Pak for Data 服务提供的功能的人群，而不是负责自行管理安装或集群基础设施的人员。
>
> Tenant Admins 也是终端用户，但在自助管理方面扮演着稍重要的角色——他们架起了运维团队与 Cloud Pak for Data 实例使用者之间的桥梁。

### Onboarding tenants

在这种多租户部署中，需要有一个正式流程来识别租户、授予他们对分配资源适当的（有限）访问权限，并计量其使用情况。在 Cloud Pak for Data 中，规定做法是运维团队为部署 Cloud Pak for Data 服务创建唯一的 OpenShift 项目命名空间，并配备适当的 ResourceQuotas。

根据对 Kubernetes 访问的策略，OpenShift 集群管理员可以向租户管理员用户授予对该 OpenShift 项目命名空间的有限 RBAC 权限。这将允许此类租户根据需要部署 Cloud Pak for Data 服务（或升级它们）。

> **Note**
>
> 如第 9 章《Technical Overview, Management, and Administration》所述，操作员的使用旨在帮助运维团队（操作员毕竟是将运维知识编码到软件中）和租户本身。集群管理员根据其租户用例（以及相关的容器镜像）引入所需的任何操作员并保持其最新。当租户需要在其命名空间中部署服务时，他们只需使用适当的自定义资源规范“声明”其需求，而无需成为理解如何自动化安装（或升级）任何服务的专家。

## Architecting for multi-tenancy

在本节中，我们将探讨使用 Cloud Pak for Data 实现多租户意味着什么，从如何在 OpenShift 集群上规划拓扑以及如何在同一集群内分离租户开始。

### Achieving tenancy with namespace scoping

正如我们之前所建立的，为每个唯一租户分配唯一的 OpenShift 项目（Kubernetes 命名空间）是 Cloud Pak for Data 多租户部署的推荐方法。

下图显示了多个租户如何被沙箱化到独立的 OpenShift 项目中，以及 Operations 区域（由集群管理员管理）与 Tenants 区域（租户用户可能被授予访问权限）之间的逻辑分离：

![](img/50e1dac0750b69f2b8e8efff000eb13b_300_0.png)

Figure 12.1 – Individual tenant namespaces in a shared Kubernetes cluster

**Cluster Operations zone** 包括 OpenShift 和 Kubernetes 提供的所有管理功能（为简洁起见，前述图中未显示全部）。**Operator Lifecycle Manager (OLM)** 提供操作员和服务，用于安装和管理 Cloud Pak for Data 本身。它对于实现多租户也非常重要，因为它提供了扩展集群的正确抽象，并使不同的功能（通过服务操作员）能够在集群中可用。为了满足职责分离要求，OLM 可以完全由集群管理员控制并集成到其 CI/CD 流程中，而租户，包括其管理员，被拒绝访问。

**Cloud Pak Foundational Services (CPFS)** 通常位于 *ibm-common-services* 命名空间中。这包括 *namespace-scope operator*，它对于确保 Cloud Pak for Data 操作员仅受限于管理特定租户命名空间而不是 *所有* 命名空间至关重要。这些操作员根本不需要监视 *非* Cloud Pak for Data 命名空间，而且出于安全原因（最小权限原则），最好甚至不授予这些操作员该权限。虽然 Cloud Pak for Data 服务操作员可以与 CPFS 一同位于 *ibm-common-services* 中，但它们也可以安装在不同的 *cpd-operators* 命名空间中，如前述图所示。例如，当同一集群中存在其他 Cloud Pak 时，您可能会选择使用 *cpd-operators*。虽然 CPFS 操作员需要管理所有 Cloud Pak 命名空间，但 Cloud Pak for Data 操作员并非如此，并且通过将它们隔离到 *cpd-operators*，您可以确保它们仅限于 Cloud Pak for Data 命名空间。

> **Note**
>
> 如前述图所示，每个租户命名空间都有自己独立的 Cloud Pak for Data 控制平面副本，并根据需要进行扩展，包括带有自己私有 ingress/route 的前端入口。OpenShift 能够通过唯一的租户 URL 暴露此前端入口供访问。这使得客户端应用能够从集群外部连接到适当的租户实例。

每个租户的身份验证和审计可以独立配置。用于生成 bearer token 的签名密钥对每个租户都是唯一的。因此，使用一个租户实例的身份验证令牌将无法访问另一个租户。授权形式为 *roles and permissions*，可以帮助租户管理员以精细的方式向该租户的用户授予授权。此租户的 Cloud Pak for Data 安装中的对象和功能仅对授权用户可用。

OpenShift 还会自动为每个项目生成唯一的 *UID* 范围。因此，一个 Cloud Pak for Data 租户命名空间内的 Linux 进程运行的 UID 与另一个命名空间的进程不同。这与 **RHEL's SELinux** 能力相结合，确保即使来自两个租户的 Pod 被调度到同一 Linux 主机上，这些进程也无法相互交互。因此，可能感染了某个租户 Pod 的恶意进程，或可能意外引入的用户错误，都不会影响另一个租户的 Pod 进程。请注意，某些 Cloud Pak for Data v4 服务依赖于通过使用自定义 SCC 的固定 UID（如 *Chapter 10, Security and Compliance* 中所述）。

通过网络策略，可以在命名空间内允许访问，微服务可以相互通信。然而，来自另一个租户的微服务的访问可以被有效阻止，除非是通过前端入口进入的请求，并且只有经过正确授权的请求。

### Choosing dedicated versus shared compute nodes

在 OpenShift 中，SELinux 和 Linux 进程的唯一租户 UID 在不同租户的进程并排执行、共享同一操作系统内核运行时时提供了相当大的安全性。借助命名空间 ResourceQuotas 以及 Cloud Pak for Data 监控和执行使用配额的能力，还有额外的控制措施可确保嘈杂的租户工作负载不会对另一个租户产生不利影响，并且集群中的 SLA 可以得到保证。

然而，企业中的安全策略可能不允许将一个租户的进程与另一个租户共置，或 SLA 要求租户之间某种程度的物理隔离。显然，由于为单个租户分配独立的 Kubernetes 集群在运营上成本过高，OpenShift 容器平台提供了支持此类要求的选项。

