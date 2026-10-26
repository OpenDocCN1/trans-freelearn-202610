

```yaml
requests:
  cpu: 100m
  memory: 256Mi
```

## 其他注意事项

在前面的章节中，我们研究了如何唯一标识 Cloud Pak for Data 工作负载，如何使用最小权限原则为每个微服务授予 RBAC，以及如何限制每个微服务消耗的资源。在本节中，我们将介绍适用于 *多个* 微服务甚至整个 Cloud Pak for Data 安装的安全方面。

我们将探讨以下内容的含义：

- 确保两个微服务之间传输数据的<|message|>
- 静态数据保护
- 了解传统防病毒软件的相关性

### 传输中加密和保护入口点

Cloud Pak for Data 仅公开一个 HTTPS 端口作为浏览器用户体验和 API 访问的主要访问点。该端口以 OpenShift 路由的形式公开。命名空间内部和入站的所有通信均使用 SSL 进行加密，仅启用 **TLS 1.2** 高安全加密算法。

在 OpenShift 命名空间中安装 Cloud Pak for Data 会导致生成自签名 TLS 证书。默认情况下，所有 HTTPS 客户端都不信任此证书，应将其替换为企业自己的签名 TLS 证书 [13]。

Cloud Pak for Data 中的所有服务都必须支持基于 TLS 的加密，用于服务到服务通信以及与外部系统通信。例如，DB2 通过 ODBC 和 JDBC 为客户端应用程序公开 TLS，即使从集群外部连接也能确保安全连接并确保传输中的数据也得到加密。连接外部客户标识的数据库和数据湖，例如 Data Virtualization、Watson Studio 服务，也可以配置为通过 TLS 进行。

请注意，Cloud Pak for Data 服务中此类传输中的基于 TLS 的加密是在基础设施中可能配置的任何内容之上的。例如，Kubernetes 节点间通信网络也可能作为云基础设施或 VPN 技术的一部分进行加密。

最好 *不要* 将这些 Kubernetes 节点视为通用 Linux 系统，而应将其视为由 OpenShift 作为集群进行保护和管理的专用机器。OpenShift 为 *整个堆栈* 从操作系统到容器再到单个工作负载提供安全性和运维。传统防病毒软件无法覆盖整个堆栈，并且可能会干扰集群的运行或可靠性。

Red Hat 和 OpenShift 提供额外的安全加固，以减少对防病毒软件的需求 [19]。请注意，传统扫描程序通常用于在共享文件系统（例如由 Samba 提供服务并挂载在 Windows 客户端上的文件系统）中查找病毒。但是，通过在 OpenShift Container Platform 和 RHEL CoreOS 上以规定方式运行 Kubernetes，不应在这些 Kubernetes 节点上首先设置此类任意文件服务。

在本节中，我们探讨了在操作系统和 OpenShift 中可用的安全结构。我们还研究了 Cloud Pak for Data 如何通过审慎使用 Kubernetes RBAC 来确保每个服务帐户的最小权限，并使用标签和注释的约定来标记所有工作负载。它还限制计算资源的分配，以确保安全可靠的运行。在下一节中，我们将研究 Cloud Pak for Data 管理员可用于配置身份验证并确保每个用户获得适当授权级别的控制。

## 用户访问和授权

Cloud Pak for Data 本身就是一个平台，是一个多用户系统，因此也为用户提供访问控制。它还与企业 LDAP 或 Active Directory 以及其他身份提供者集成，以确保对其服务的安全访问。还支持审计以跟踪对 Cloud Pak for Data 平台的访问。

与任何多用户系统一样，Cloud Pak for Data 会识别授予特定用户或用户组的角色和权限。特权用户负责确保只有授权用户才能访问 Cloud Pak for Data 主机服务和内容。

### 身份验证

虽然 Cloud Pak for Data 包含一个开箱即用的简单机制来识别用户并授予他们访问权限，但这仅用于初始运行，并不意味着用于安全生产用途，并且无法满足任何合规性法规要求。例如，密码策略或多因素身份验证无法使用此默认机制支持。相反，企业应设置与身份提供者的连接。

### 授权

通过权限和角色，企业可以定义自己的角色并授予他们对不同 Cloud Pak for Data 服务的特定访问权限。

角色是用于定义授予特定用户角色的一组权限（特权）的实体。

Cloud Pak for Data 服务引入一个或多个开箱即用的角色以便开始使用，并提供一种帮助管理角色的体验。企业可以使用此类角色来遵守因合规原因而需要的 *职责分离* 政策。例如，管理员可能被拒绝访问数据（这是他们最初未配置的数据库实例的默认设置），并且还拒绝访问数据目录，通过撤销特定权限。

还可以授予细粒度的权限，例如，仅授予特定用户组管理用户访问权限和管理组访问权限的能力，但他们无需获得其他管理功能的访问权限。

管理员通常使用用户管理体验配置角色，如下图所示：

![](img/50e1dac0750b69f2b8e8efff000eb13b_256_0.png)

图 10.5 – 管理角色体验

下表显示了 Cloud Pak for Data 中分配给不同开箱即用角色 [38] 的权限示例，以及相关的服务提供：

- **管理员角色：** **管理员** 是 Cloud Pak for Data 中的预定义角色，包括以下表中列出的特定权限集。存在的权限集取决于已安装的服务。例如，如果未安装 Watson Knowledge Catalog，则 **访问治理工件** 权限将不可用。

  | 权限 | 类别 | 贡献该权限的服务 |
  |---|---|---|
  | 访问治理工件 | 治理工件 | Watson™ Knowledge Catalog |
  | 管理平台 | 平台管理 | Cloud Pak for Data 控制平面 |
  | 创建部署空间 | 部署 | - Watson Knowledge Catalog<br>- Watson Studio |
  | 创建项目 | 项目 | - Watson Knowledge Catalog<br>- Watson Studio |
  | 创建服务实例 | 服务实例 | Cloud Pak for Data 控制平面 |
  | 管理资产发现 | 数据管理 | Watson Knowledge Catalog |
  | 管理目录 | 目录 | - Watson Knowledge Catalog<br>- Watson Studio |
  | 管理数据保护规则 | 治理工件 | Watson Knowledge Catalog |
  | 管理数据质量 | 数据管理 | Watson Knowledge Catalog |
  | 管理数据转换 | 数据集成 | DataStage® |
  | 管理治理分类 | 治理工件 | Watson Knowledge Catalog |
  | 管理信息资产 | 目录 | Watson Knowledge Catalog |
  | 管理元数据 | 数据管理 | Watson Knowledge Catalog |
  | 管理工作流 | 工作流 | Watson Knowledge Catalog |

  图 10.6 – 授予管理员角色的权限

- **数据科学家** 角色：此角色指定通常参与高级分析和机器学习的<|message|>。他们从目录中引用数据，并在项目中与其他用户协作。对于此类用户，通常只需要一小部分权限，如下表所示。这也授予他们使用部署（例如机器学习模型）的权限。

  | 权限 | 类别 | 贡献该权限的服务 |
  |---|---|---|
  | 访问目录 | 目录 | - Watson Knowledge Catalog<br>- Watson Studio |
  | 创建部署空间 | 部署 | - Watson Knowledge Catalog<br>- Watson Studio |
  | 创建项目 | 项目 | - Watson Knowledge Catalog<br>- Watson Studio |

  图 10.7 – 授予数据科学家角色的权限

- **用户** 角色：**用户** 角色很特殊，因为它通常表示分配给某类访客用户的最少权限集。将 **用户** 角色分配给某人仅意味着他们将能够登录并查看不需要任何额外授权的任何内容。将 **用户** 角色分配也用于向平台引入新用户，从而使他们随后可以作为个人添加到用户组或分配对项目、目录甚至数据库的访问权限。

  然而，当安装某些服务（例如 Watson Knowledge Catalog 或 Watson Studio）时，默认情况下，它们会向此 **用户** 角色授予额外权限。这使这些用户至少能够在自己的项目或部署空间范围内私下工作。当然，就像任何预定义角色一样，管理员可以选择更改此角色并删除这些权限或引入其他权限，以符合他们对“最小”特权角色的定义。

  | 权限 | 类别 | 贡献该权限的服务 |
  |---|---|---|
  | 创建部署空间 | 部署 | - Watson Knowledge Catalog<br>- Watson Studio |
  | 创建项目 | 项目 | - Watson Knowledge Catalog<br>- Watson Studio |

  图 10.8 – 授予用户角色的权限

