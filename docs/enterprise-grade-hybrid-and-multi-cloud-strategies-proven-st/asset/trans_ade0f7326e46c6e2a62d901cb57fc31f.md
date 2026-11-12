

## 臃肿的容器

企业构建云原生应用时面临的另一个常见反模式是**臃肿容器**的挑战。臃肿容器是指容器体积增大的情况，这可能导致多种挑战，影响应用集群的效率、性能和可管理性。容器镜像体积可能因多种因素而增加，例如低效的依赖管理、未使用的构建产物、不必要的层缓存、使用完整操作系统作为基础层构建镜像，以及包含大型二进制文件（如编译后的可执行文件或媒体文件）构建镜像。该反模式的一些主要挑战包括：

*   **资源消耗低效**：臃肿容器消耗更多的 CPU、内存和存储资源。这可能导致资源利用率低下，从而增加成本并降低整体集群性能。
*   **可扩展性**：过大的容器会影响 Kubernetes 高效扩展应用的能力。当扩展基于资源利用率时，较大的容器可能导致不准确的扩展决策，从而造成资源的过度配置或配置不足。
*   **启动时间缓慢**：较大的容器通常需要更长的启动时间，因为它们需要加载更多的资源和依赖项。这会拖慢部署和扩展流程，影响应用的响应能力。
*   **攻击面增加**：较大的容器可能包含不必要的软件或依赖项，增加潜在的攻击面和安全风险。未修补或不必要的软件可能使应用容易受到漏洞利用和攻击。
*   **故障排查复杂性**：臃肿容器会使故障排查和调试更加复杂，因为当容器包含大量组件和依赖项时，识别性能瓶颈、资源争用问题或兼容性问题会变得更难。
*   **网络流量**：依赖项过多的容器会产生更多网络流量，影响整体网络性能，导致拥塞或延迟问题。
*   **资源碎片化**：大型容器可能需要的资源超过单个节点上的可用资源。这可能导致资源碎片化和利用率不足，因为原本可以在同一节点上调度多个较小的容器。
*   **可维护性**：随着时间的推移，管理大型或复杂的容器变得更具挑战性。升级、修补和维护此类容器容易出错且耗时。

## 可变基础设施

**可变基础设施**是指直接对现有基础设施组件进行更改，而不是用新组件替换它们的做法。在云计算语境下，这涉及修改现有资源（如虚拟机、数据库和网络组件），而不是从头创建新实例，这可能是一种反模式。云原生架构主张将基础设施组件视为不可变的，这指的是创建新实例而不是修改现有实例的做法。云环境中可变基础设施的一些挑战包括：

*   **配置漂移**：随着时间的推移，在可变基础设施中，不同实例上的配置设置可能会偏离其原始状态，导致配置漂移。这可能导致不一致，并难以维护一个可预测且稳定的环境。
*   **依赖管理**：随着可变基础设施中对组件进行增量修改，这些组件之间的依赖关系可能变得复杂且难以管理。确保对一个组件的更改不会无意中影响其他组件，需要仔细考量。
*   **回滚挑战**：如果更新失败或更改导致问题，在可变基础设施设置中回滚更改可能更具挑战性。这要求回滚机制定义明确且经过测试，以最大限度地减少停机时间和潜在数据丢失。
*   **版本控制与追踪**：如果没有适当的版本控制和追踪机制，管理可变基础设施组件的更改将极具挑战性。追踪更改内容、更改人及更改原因对于故障排查和审计至关重要。

当组织踏上云转型之旅时，反模式可能会阻碍云价值的最佳利用。理解并识别这些反模式对于组织避免昂贵的错误、最大化效率、提高可靠性并充分发挥云优势至关重要。

# 总结

在本章中，我们重点介绍了组织如何构建一个稳健、开放且敏捷的面向云的企业基础，以支持当前和未来的业务需求。我们探讨了云采用框架的四个阶段，即评估、规划、部署和优化，以及每个阶段中能为云转型计划提供结构化方法的关键要素。我们还详细讨论了企业可以采用的各种混合云和多云部署模式，特别关注拥有大量本地 IT 资产的组织，并确认了云采用面临的挑战。

当组织在混合云和多云部署的复杂性中航行时，一个可扩展且安全的网络架构对于支持高可用性、性能优化和规模扩展等关键考量至关重要。考虑到这一点，我们回顾了一些核心基础网络架构蓝图、其适用性及注意事项。我们还深入探讨了企业如何将安全架构为其云环境的一等公民，重点关注身份与访问管理、基础设施安全和应用安全等各个安全方面。随后，我们探讨了 AWS、Azure 和 Google Cloud 等主要云服务提供商提供的安全模型。

最后，我们讨论了组织从本地迁移到云时常见的五个云原生反模式陷阱。在下一章中，我们将探讨一些大型企业的云用例。

# 延伸阅读

本章使用的参考资料及一些额外资源列表如下：

1.  AWS 共享责任模型：[https://aws.amazon.com/compliance/shared-responsibility-model/](https://aws.amazon.com/compliance/shared-responsibility-model/)
2.  AWS 安全基础：[https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/foundations.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/foundations.html)
3.  Azure 共享责任模型：[https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility)
4.  Azure 安全基准基础：[https://learn.microsoft.com/en-us/azure/governance/blueprints/samples/azure-security-benchmark-foundation/](https://learn.microsoft.com/en-us/azure/governance/blueprints/samples/azure-security-benchmark-foundation/)
5.  Google Cloud 共同命运模型：[https://cloud.google.com/architecture/framework/security/shared-responsibility-shared-fate](https://cloud.google.com/architecture/framework/security/shared-responsibility-shared-fate)
6.  Google Cloud 企业基础：[https://cloud.google.com/architecture/security-foundations#a_defense-in-depth_security_model](https://cloud.google.com/architecture/security-foundations#a_defense-in-depth_security_model)
7.  12 Factor App：[https://12factor.net/](https://12factor.net/)

# 7. 混合云用例

