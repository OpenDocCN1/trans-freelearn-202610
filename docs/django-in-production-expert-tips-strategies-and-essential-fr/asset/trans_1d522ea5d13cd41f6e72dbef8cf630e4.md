

我们可以通过 Elastic Beanstalk AWS 控制台仪表板进一步配置 Beanstalk 环境。更多详情，请阅读官方文档 [https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options.html)。

Elastic Beanstalk 还允许开发者使用 `.ebextensions` 以编程方式配置环境。Elastic Beanstalk 是一个功能强大的平台，它让我们能够在很大程度上控制如何配置我们的应用程序。AWS 文档是收集有关如何配置 AWS Elastic Beanstalk 以及配置哪些内容的全部信息的最佳资源。

进一步阅读

我们目前以开发模式运行 Django 应用程序，以便您可以轻松跟随所有示例。随着我们希望将应用程序扩展到生产环境，我们需要添加 `nginx` 和 `gunicorn`，并根据需求设置应用程序。每个应用程序都有其规模和需求。要了解更多信息，请阅读 AWS 文档并根据您的需求和规模设置应用程序。

在下一节中，我们将讨论在使用 AWS 服务时应遵循的最佳实践。

# 遵循 AWS 基础设施的最佳实践

在本节中，我们将学习在使用 AWS 服务时应遵循的最佳实践。AWS 拥有一个良好的架构框架，AWS 建议组织遵循该框架。更多详情，请查看 [https://aws.amazon.com/architecture/well-architected/](https://aws.amazon.com/architecture/well-architected/)。

AWS Well-Architected Framework 非常全面，可能需要额外的工作才能达到其标准。我们不会深入探讨 AWS Well-Architected Framework，而是仅列出可以帮助开发者更好地配置 AWS 服务并确保系统中没有漏洞的最佳实践。我们将从 RDS 的最佳实践开始。

## RDS 的最佳实践

在本节中，我们将讨论所有应遵循的 RDS 最佳实践，以便在生产环境中运行 RDS：

*   **启用集群删除保护**：在生产环境中启用此标志是必须的，这样我们就不会意外删除 RDS 集群。
*   **启用 Multi-AZ**：数据库是应用程序中最重要的组件，因为它保存了对业务至关重要的一切持久信息。通过启用 RDS Multi-AZ，我们确保 RDS 服务器具有高可用性/故障转移。如果任何一个 AWS 可用区变得不可用，RDS 将自动切换到另一个可用区。
*   **性能洞察**：使用 Performance Insights，我们可以微调数据库的性能调优和监控功能。这有助于我们快速评估 RDS 服务器的负载，并帮助非专家检测性能问题。
*   **在低 I/O 时间启用 RDS 自动备份**：为 RDS 启用自动备份很重要。我们应在应用程序使用量较低时安排 RDS 备份——例如，对于面向消费者的应用程序，可安排在午夜后。
*   **不要使用默认端口**：不要使用 5432 的默认端口，这样攻击者和黑客就无法识别实际端口。
*   **RDS 不可公开访问**：确保 RDS 服务器不可公开访问。它只能在 AWS 网络内部访问，不能直接通过互联网访问。
*   **使用 CloudWatch 监控**：AWS RDS 有许多默认的 CloudWatch 指标会推送到 AWS CloudWatch。我们可以使用 CloudWatch 更好地监控 RDS，并在适当的阈值上设置警报。
*   **加密 RDS 静态数据**：始终确保 RDS 使用 AWS `Key Management Service` `KMS` 加密数据库。

## ElastiCache 的最佳实践

在本节中，我们将介绍在使用 ElastiCache 时可以遵循的最佳实践：

*   **启用 Multi-AZ 集群**：启用 ElastiCache Multi-AZ 集群将有助于我们获得更高的可用性。AWS 将保证更高的 `service-level agreement` `SLA`。每当我们的主节点宕机时，AWS 将自动执行故障转移。
*   **不要使用默认端口**：不要为 ElastiCache 使用 6379 的默认端口。
*   **使用 CloudWatch 监控**：为 Redis 集群设置 CloudWatch 监控将有助于我们为 ElastiCache 集群设置警报。默认情况下，ElastiCache 提供主机级指标（例如 CPU 使用率）和缓存引擎特定指标（例如缓存命中、未命中等）。

## Elastic Beanstalk 的最佳实践

现在，让我们来看 Elastic Beanstalk 的最佳实践：

*   **保护网络**：运行 Elastic Beanstalk 的 EC2 实例不应通过互联网在外部访问。添加 ALB 并确保所有流量都通过 ALB 路由将有助于将 ALB 保留在公共子网中，而 EC2 实例运行在私有子网中。
*   **IAM 最小权限访问**：Elastic Beanstalk 将使用服务账户角色执行不同操作。我们应为服务账户角色授予有限的权限。
*   **滚动部署并增加额外批次**：AWS 的 Beanstalk 部署策略可以帮助控制应用程序的部署方式。滚动部署并增加额外批次是最安全的选项，因为我们不会移除现有机器群，而是添加运行最新应用程序代码的新服务器。
*   **设置 CloudWatch 指标和监控**：为关键 Elastic Beanstalk 指标设置警报。

## IAM 和安全性的最佳实践

以下是 IAM 和安全性的最佳实践：

*   **永远不要使用 root 账户**：永远不要使用 root 账户进行任何操作。root 账户不应使用，除非有绝对必要的且其他 IAM 用户无法完成的需求。
*   **强制使用双因素认证（2FA）**：强制所有 AWS IAM 人类用户使用 2FA 将确保额外的安全性。
*   **应用最小权限原则**：在创建 IAM 策略时，始终明确指定要提供的权限，而不是授予 blanket 权限。
*   **定期审查并删除未使用的用户和角色**：进行持续监控并删除冗余用户和角色。

至此，我们已了解在使用 AWS 组件时应遵循的最佳实践。我们已尝试提供全面的视图并在一个地方列出所有最佳实践。然而，强烈建议您在将应用程序移至生产环境之前遵循官方 AWS 文档。

# 总结

在本章中，我们学习了部署 Django 应用程序到生产环境所需的各种 AWS 组件。我们使用 AWS Elastic Beanstalk 配置并部署了 Django 应用程序到生产环境。我们还在 Django 应用程序中使用了 AWS RDS 和 ElastiCache 用于数据库和缓存目的。AWS 确保我们可以轻松扩展应用程序基础设施。

AWS Elastic Beanstalk 是一种托管平台服务，可以通过帮助我们部署和扩展应用程序来编排应用程序。AWS 封装了部署和运行应用程序所需的所有重要配置以及 AWS 服务之间的链接，以便实现规模化。

运行稳定且性能良好的应用程序很重要。在下一章中，我们将学习如何使用 New Relic、Rollbar 等工具持续监控生产环境中运行的 Django 应用程序。

# 14

# 监控 Django 应用程序

在 [*Chapter 13*](#B18867_13.xhtml#_idTextAnchor256) 中，我们学习了如何将 Django 应用程序部署到 AWS 并在生产环境中运行代码。现在是应用程序开发的最后也是最重要的一部分——监控和维护。

