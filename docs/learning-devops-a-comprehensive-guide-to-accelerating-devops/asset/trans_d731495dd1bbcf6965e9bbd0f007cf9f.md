
要测试与 AKS 的连接，我们可以执行以下命令，`kubectl get nodes`，该命令显示创建 AKS 群集时配置的节点数量，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_352_0.png)

图 10.26 – kubectl get nodes 列表

本章中我们在 _Kubernetes 应用程序部署的第一个示例_ 部分看到的所有操作，无论是使用 AKS 还是 `kubectl` 部署应用程序，都是相同的。

在了解在 Azure 中创建 AKS 服务的步骤后，我们将现在提供其优势的概述。

## AKS 的优势

AKS 是在 Azure 中托管的 Kubernetes 服务。这有助于与 Azure 集成，并具有此处列出的其他一些优势：

- **即用型**：在 AKS 中，Kubernetes Web 仪表板原生安装，位于 `https://docs.microsoft.com/en-us/azure/aks/kubernetes-dashboard` 的文档说明了如何访问它。

- **集成监控服务**：AKS 还具有 Azure 的所有集成监控服务，包括容器监控、群集性能管理和日志管理，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_352_1.png)

图 10.27 – AKS 监控

- **非常易于扩展**：AKS 允许通过门户或脚本快速直接扩展群集的节点数量。

如下图所示，我们在 Azure 门户中选择所需的节点数量，更改会立即生效：

![](img/d3344b39c888aab0b832625465f837c9_353_0.png)

图 10.28 – AKS 扩展

如果我们有 Azure 订阅并且想要使用 Kubernetes，安装是直观且快速的。AKS 具有许多优势，例如 Azure 门户中的集成监控和扩展。使用 `kubectl` 工具不需要与本地 Kubernetes 实例相比进行任何更改。

在本节中，我们讨论了 AKS，它是 Azure 中的托管 Kubernetes 服务。然后，我们创建了 AKS 实例并配置了 `kubeconfig` 文件以连接到该 AKS 实例。最后，我们列出了其优势，主要是集成监控和快速可扩展性。

在下一节中，我们将看到一些关于如何使用 Azure Pipelines 的 CI/CD 管道在 Kubernetes 中部署应用程序的资源。

## 使用 Azure Pipelines 为 Kubernetes 创建 CI/CD 管道

到目前为止，我们已经了解如何使用 `kubectl` 在本地 Kubernetes 群集或使用 AKS 的远程群集中部署容器化应用程序。

在本书的第一版中，我解释了如何在 Azure DevOps 中构建完整的管道，从创建新 Docker 镜像并将其推送到 Docker Hub 到在 AKS 群集中部署。

自第一版以来，不同 CI/CD 工具的许多功能已得到改进，以便在 Kubernetes 中部署。

这就是为什么在第二版中，我不会再详细解释它，而是为您提供一些对我日常工作有用的不同资源，如下所示：

- 第一个资源是一个非常完整的视频，解释了使用 Azure DevOps 在 AKS 中部署应用程序的所有详细信息，位于以下链接：https://www.youtube.com/watch?v=K4uNl6JA7g8

- 关于同一主题，在 Azure DevOps 上有一个很棒的实验室，位于以下链接：https://www.azuredevopslabs.com/labs/vstsextend/kubernetes/

- 要创建用于部署到 Kubernetes 的 YAML 格式的 Azure DevOps 管道，请阅读此处的官方文档：https://docs.microsoft.com/en-us/azure/devops/pipelines/ecosystems/kubernetes/aks-template?view=azure-devops

- 然后，以下文档还提供了一个使用 Jenkins 和 Azure DevOps 将应用程序部署到 Kubernetes 的教程：https://docs.microsoft.com/en-us/azure/devops/pipelines/release/integrate-jenkins-pipelines-aks?view=azure-devops

在本节中，我们报告了一些用于端到端 (E2E) DevOps CI/CD 管道的资源，以便使用 Azure Pipelines 在 Kubernetes 群集（我们示例中的 AKS）中部署应用程序。

在下一节中，我们将学习监控 Kubernetes 上的应用程序和指标的不同方法，并了解可以使用的工具。

## 监控 Kubernetes 中的应用程序和指标

当我们在 Kubernetes 中部署应用程序时，拥有一个监控策略来检查和调试这些应用程序的生命周期以及检查 **中央处理器 (CPU)** 和 **随机存取存储器 (RAM)** 指标非常重要——我认为这是一项要求。

我们将现在讨论在 Kubernetes 中调试和监控应用程序的不同方法。

让我们从基本方法开始，即使用 `kubectl` 命令行。

## 使用 kubectl 命令行

要使用 `kubectl` 命令行调试应用程序，请运行下面详细说明的命令：

- 要显示 Kubernetes 资源的状态，请运行以下命令：

```
kubectl get pods,svc
```

前面命令的输出如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_355_0.png)

图 10.29 – kubectl 获取资源

使用此命令，我们可以查看 pod 是否正在运行并了解服务的状态。

- 要显示应用程序日志，请运行 `kubectl logs pod/<pod name>` 命令，如下图所示：

![](img/d3344b39c888aab0b832625465f837c9_355_1.png)

图 10.30 – kubectl 获取 pod 日志

此命令显示应用程序日志的输出。

有关使用 `kubectl` 调试应用程序的更多详细信息，请阅读此处的文档：

`https://kubernetes.io/docs/tasks/debug-application-cluster/debug-running-pod/`

使用 `kubectl` 命令，我们可以自动化应用程序调试，但有必要学习所有命令行选项。

接下来，我们将讨论使用一些工具调试应用程序。

## 使用 Web 仪表板

如前一节本章第一部分 _安装 Kubernetes_ 中所述，我们可以使用基本仪表板在 Web **用户界面 (UI)** 中显示在 Kubernetes 中部署的所有资源的详细信息。

对于 Azure 的 AKS、Amazon Web Services (AWS) 的 EKS 和 Google Cloud Platform (GCP) 的 GKE 等云托管 Kubernetes 服务，我们可以使用集成和托管的仪表板。例如，对于 Azure 门户中的 AKS，我们可以轻松查看所有 pod 日志。以下是 Azure 门户中 pod 日志的示例：

![](img/d3344b39c888aab0b832625465f837c9_356_0.png)

图 10.31 – AKS 实时日志

有关在 AKS 上调试的更多详细信息，请阅读此处的文档：

`https://docs.microsoft.com/en-us/azure/architecture/microservices/logging-monitoring`

我们还可以使用 Octant 或 Lens 等分层工具。

## 使用分层工具

有许多工具或解决方案可用于监控和显示在 Kubernetes 群集中部署的所有资源。其中包括两个我经常使用的免费工具：Octant 和 Lens。

## Octant

Octant 是一个 VMware 社区项目，是一个在本地或 Docker 容器中启动的 Web 应用程序，用于可视化 Kubernetes 资源和应用程序日志。

Octant 的文档可在此处查看：

`https://octant.dev/`

源代码可在此处查看：
https://github.com/vmware-tanzu/octant/blob/master/README.md

## Lens

Lens 也是一个免费工具，通过客户端二进制文件安装。对我来说，Lens 是可视化和调试应用程序的最佳工具。Lens 的文档可在此处查看：
https://k8slens.dev/。

您可以在下图中查看 Lens 仪表板的概览：

![](img/d3344b39c888aab0b832625465f837c9_357_0.png)

图 10.32 – Lens 仪表板

讨论了调试应用程序的工具后，我们将看到如何显示 Kubernetes 指标。

## 监控 Kubernetes 指标

刚才描述的所有工具对于调试 Kubernetes 资源和托管在 Kubernetes 中的应用程序非常出色。但这还不够——在监控方面，我们还需要监控应用程序使用的 CPU 和 RAM 等指标。
