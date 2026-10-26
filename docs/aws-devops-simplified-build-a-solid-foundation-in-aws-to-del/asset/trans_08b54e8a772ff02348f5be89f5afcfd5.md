
图 11.3 – AWS Control Tower 对不同 AWS 服务的编排

此图并非详尽展示 AWS Control Tower 在后台处理的每一项功能；相反，它旨在突出账户创建工作流中涉及的不同步骤，以及随后向其中部署资源的过程。让我们更详细地看看这张图：

- 1. 平台开发人员使用 AWS Control Tower 控制台界面提交新的账户创建请求，并指定该账户应加入的目标 OU。
- 2. AWS Control Tower 将请求传递给 Account Factory（一个 Service Catalog 产品），由其启动账户预置工作流。
- 3. 在后台，该请求会调用 AWS Organizations API 来创建新账户。
- 4. 系统会创建一个 AWS 账户，并将其放入目标 OU，该 OU 决定了此账户中托管的工作负载将适用哪些策略控制和治理规则。
- 5. AWS CloudFormation StackSets 是一项可自动将堆栈部署到账户-区域组合的服务功能，AWS Control Tower 服务已经为其定义了一些可复用模板。AWS Control Tower 会为 StackSet 定义创建新的堆栈实例，从而在目标账户中启动 AWS CloudFormation 模板的预置。
- 6. AWS CloudFormation 堆栈预置在目标账户内的特定 AWS 区域中开始，并部署诸如 AWS Config 记录器、IAM 角色以及其他以合规方式管理此账户所需的资源。
- 7. AWS Config 设置一个 *记录器*，用于监控账户内所有资源的变更，并设置一个交付通道。
- 8. 配置记录器是一种记录所有资源配置变更并将数据发送到集中式 S3 存储桶以满足审计需求的机制；同时，通知会发送到 SNS 主题。SNS 通知按区域聚合，当发生意外资源变更时，安全团队可以使用这些通知进行高级分析和自动告警。
- 9. 当你首次安装 AWS Control Tower 时，它会创建一个组织级的 AWS CloudTrail 跟踪，自动获取组织中的新账户，并将跟踪数据聚合到 LogArchive 账户中的中央 S3 存储桶。它还将日志推送到管理账户中的 CloudWatch 日志组。这为中央安全团队提供了审计和调查的可能性，而无需访问 Landing Zone 中的所有账户。

Control Tower 开箱即用地提供了所有这些功能，安装大约需要 1 小时。你可能已经注意到，该服务并不涵盖任何应用程序或组织特定的部署需求。到目前为止，它是一种基于良好实践的规定性设置，会在账户创建后立即推出。要推出其他 AWS 服务和资源，你需要一个部署解决方案或框架来填补缺口，以构建完整的 Landing Zone 平台。

接下来，我们来看看 CfCT。这是一个 AWS 支持的解决方案，可与 AWS Control Tower 服务集成，以推出由 AWS CloudFormation 模板定义的自定义资源。假设你需要为安全扫描第三方平台准备一个特定的 IAM 角色，并且该角色应在所有 AWS 账户预置后自动部署到其中。CfCT 可以帮助实现这一点。或者，你可能希望将某些 Config 规则部署到生产 OU 中的所有 AWS 账户，而不是其他账户。

## 使用 CfCT 部署资源

CfCT 是一个与你的 Control Tower 服务部署集成的部署框架。它需要安装在管理账户中，且必须位于最初设置 Control Tower 的同一区域。它会挂接到服务事件上，并运行一个工作流，在组织内的目标账户中预置 SCP 策略或 AWS CloudFormation 资源。

该框架用于部署两类资源：SCP 策略和 CloudFormation StackSets。每类资源都有专门的部署工作流，通过 AWS Step Functions 实现。用户与框架的交互通过一个具有定义良好的 YAML 格式的清单文件进行。每当文件提交到版本控制时，框架都会获取变更并执行管道。

我们可以查看**图 11.4** 来了解部署框架中涉及的不同组件：

![](img/ecd265dfca9bb64161408ba8a944a4d9_276_0.png)

图 11.4 – CfCT 框架的组件（来源：https://docs.aws.amazon.com）

整个解决方案可以作为 CloudFormation 模板部署在你的 AWS 管理账户中。如你所见，CodePipeline 管道会从版本控制（S3 或 CodeCommit）拉取最新更改，并触发一个 CodeBuild 项目来初步验证清单文件。如果一切正常，框架会继续调用 SCP Step Functions 状态机和 CloudFormation Step Functions 状态机。如果你之前没有接触过 AWS Step Functions 服务，它是一个工作流编排器，可以帮助你构建可编程的流程，调用各种 AWS 服务来完成工作。让我们看一下该框架用于在 Landing Zone 中部署资源的清单文件示例：

```yaml
---
region: us-east-1  # Control Tower Home Region
version: 2021-03-15
resources:
  - name: test-preventive-guardrails
    description: Prevent deleting or disabling resources
    resource_file: s3://marketplace-sa-resources-ct-us-east-1/ctlabs/preventive-guardrails.json
    deploy_method: scp
    deployment_targets:
      organizational_units:
        - Security
        - Sandbox
...
- name: create-iam-role
  resource_file: s3://marketplace-sa-resources-ct-us-east-1/ctlabs/describe-regions-iam-role.template
  deploy_method: stack_set
  deployment_targets:
    organizational_units:
      - Security
      - Sandbox
    regions:
      - us-east-1
```

清单文件通过 YAML 结构中的键回答了三个问题：

- `resource_file`：需要部署的代码在哪里？
- `deploy_method`：需要部署什么？该键有两个可能的值——`scp` 或 `stack_set`。
- `deployment_targets`：资源应部署到哪里？在这里，你可以使用单个账户或代表一组账户的 OU。

一旦此文件提交到解决方案所跟踪的 Git 仓库，管道就会触发，从而将 SCP 和 CloudFormation 模板部署到目标位置——在本例中，即安全 OU 和沙箱 OU。

使用 AWS 提供的解决方案的一个好处是，如有需要，你可以获得 AWS Support 的保障。这与你可能开发的其他自定义实现不同。

## 总结

在本章开头，我们确定了多账户结构的必要性，以及它在 AWS 中托管多个工作负载时提供的好处。多种服务、解决方案和部署框架满足了特定需求，但重要的是要明白，它们本身并不能单独帮助你。组织要实现其特定需求，总是需要一定程度的定制。在明确了需求和好处之后，我们接着讨论了在设计 Landing Zone 概念时应该考虑的关键事项。我们还研究了多账户结构、安全与 IAM、DevOps 与 Config 管理以及运维方面的一些最佳实践。

在本章的后半部分，我们更深入地了解了 AWS Control Tower 的内部工作原理，以及它与 CfCT 解决方案的开箱即用集成。客户从这些解决方案中获得的关键好处之一是它们由 AWS 提供支持，就像 AWS 支持任何其他服务一样。

在下一章 *遵循 AWS Well-Architected 原则* 中，我们将探讨一个坚实基础的关键支柱，你可以为自己的应用程序工作负载考虑这些支柱。
