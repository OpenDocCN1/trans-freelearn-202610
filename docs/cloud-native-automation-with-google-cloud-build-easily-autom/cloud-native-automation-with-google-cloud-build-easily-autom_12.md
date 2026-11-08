# 第四部分：展望未来

在本书的最后一部分，我们将聚焦于尾声，涵盖 Cloud Build 作为自动化平台的未来发展方向、其与 Cloud Deploy 的集成，以及利用 Tekton 在混合云环境中运行 Cloud Build。

本部分包含以下章节：

- 第 11 章，Cloud Build 的展望

### Cloud Build 的展望

到目前为止，我们已经回顾了 Cloud Build 的基础、其常见用例以及在实践中应用 Cloud Build 的模式。

然而，Cloud Build —— 无论是在出版时还是此后 —— 仍在不断扩展其功能，并与 Google Cloud 生态系统的其他部分更深入地集成。

一个例子包括创建用于执行这些流水线的 API，该 API 使用 Cloud Build 来执行有主见的 CI/CD 流水线。

在本章中，我们将回顾与 Cloud Build 有关的一些展望领域，具体涵盖以下主题：

- 使用 Cloud Deploy 实现持续交付

### 使用 Cloud Deploy 实现持续交付

之前，在第 8 章《使用 Cloud Build 保护向 GKE 的软件交付》中，我们回顾了如何使用 Cloud Build 将应用程序的新版本发布到 Google Kubernetes Engine。当使用 Cloud Build 定义这些发布时，Cloud Build 中几乎没有内置的主见 —— 最终，用户负责定义每个构建步骤，将构建步骤连接成完整的构建，并可能为每个构建定义定制的构建触发器。

用户需要考虑的关键问题之一是如何在环境之间推广应用程序。环境及其各自的功能可以包括以下示例：

- **开发：** 应用程序可以在其中运行的开发环境，开发人员可以在此测试其应用对集群中超出其所有权的其他服务的依赖。与在本地 Kubernetes 集群中将所有其他依赖项拉取到本地环境不同，他们可以改为部署到此环境：

![](img/2fe9576240c146c49e3a21d5e7f6af00_226_0.png)

图 11.1 – 部署到开发环境

- **暂存：** 暂存环境通常与最终在生产中运行的环境几乎相同。理想情况下，错误和问题已在更早阶段的环境中被识别 —— 不仅是开发环境，还有可能是一个专门的 QA 或测试环境，其中更复杂的测试套件（如集成测试）已经通过。在此环境中，您可能希望执行更接近最终用户体验的测试，例如冒烟测试。您还可能希望在此类环境中实施混沌工程，以尝试在堆栈的各个组件随机中断时测试整个系统的弹性：

![](img/2fe9576240c146c49e3a21d5e7f6af00_226_1.png)

图 11.2 – 将发布版提升到暂存环境

- **生产：** 最后，您的应用程序最终会进入生产环境，在那里为最终用户提供服务。发布到生产环境本身可能并不简单，因为在部署到生产环境时，您可能会实施多种方法来创建应用的新版本以及如何将流量转移到该新版本：

![](img/2fe9576240c146c49e3a21d5e7f6af00_227_0.png)

图 11.3 – 将发布版提升到生产环境

要成功地将应用程序推广以遍历这些各种环境，仅靠 Cloud Build 自己定义所有这些内容可能具有挑战性。此外，如果开发人员自行定义构建来实现此目的，组织中可能会出现漂移和孤岛的风险。

因此，为了简化用户这一日益关键的过程，Google 发布了 **Cloud Deploy** —— 一种有主见的运行模板化 CI/CD 流水线的模型，旨在将应用程序发布到代表软件交付周期不同阶段的多个目标环境中。在出版时，Cloud Deploy 专注于为将应用程序部署到 **Google Kubernetes Engine (GKE)** 和 Anthos 集群的用户提供此用例。

Cloud Deploy 提供了一种使用其 API 在幕后在 Cloud Build 工作器上编排软件发布的模型。与要求用户在途中定义每个构建步骤不同，Cloud Deploy 暴露了允许声明式流水线的资源，抽象掉了 Cloud Build 资源复杂的底层编排。

在 Cloud Deploy 中，用户不是与构建和构建步骤交互，而是与 Kubernetes 和 Cloud Deploy 特定资源的组合交互。从 Kubernetes 角度来看，用户将编写与 Kubernetes API 资源一致的配置，无需任何更改。

然而，要告诉 Cloud Deploy 读取哪些 Kubernetes 配置，您将编写一个 Skaffold 配置。Skaffold 是一个开源工具，它封装了 kubectl 等工具、helm 或 kustomize 等清单渲染工具，以及 buildpacks 等构建工具，以便将 Kubernetes 配置部署到集群。

以下是一个 skaffold.yaml 文件的示例，它定义了 Skaffold 配置文件，将环境映射到将部署到它们各自的 Kubernetes 配置：

```
# For demonstration purposes only, to run this build would require setup beyond the scope of its inclusion
apiVersion: skaffold/v2beta16
kind: Config
profiles:
  - name: dev
    deploy:
      kubectl:
        manifests:
          - config/dev.yaml
  - name: staging
    deploy:
      kubectl:
        manifests:
          - config/staging.yaml
  - name: dev
    deploy:
      kubectl:
        manifests:
          - config/prod.yaml
```

用户随后开始使用 Cloud Deploy 特定资源来定义 DeliveryPipeline 资源。连同前面提到的 skaffold.yaml 文件，这些资源定义了将 Kubernetes 配置交付到多个环境的模板。

DeliveryPipeline 资源可能类似于以下 YAML 配置：

```
# For demonstration purposes only, to run this build would require setup beyond the scope of its inclusion
apiVersion: deploy.cloud.google.com/v1
kind: DeliveryPipeline
metadata:
```

```
```yaml
name: app
description: deploys to dev, staging, and prod
serialPipeline:
  stages:
  - targetId: dev
    profiles:
    - dev
  - targetId: staging
    profiles:
    - staging
  - targetId: prod
    profiles:
    - prod
```
```

请注意，DeliveryPipeline 概述了将部署 skaffold.yaml 文件中定义的清单的目标集群。它还概述了清单将应用于每个环境的顺序，代表了更改通过 dev、staging 和 prod 等环境的进展。

最后，请注意每个目标还引用了一个配置文件 —— 该配置文件与 skaffold.yaml 配置文件中定义的配置文件和它们对应的特定清单保持一致。这使用户能够执行诸如在不同环境之间切换配置或环境变量等操作。

用户必须随后创建 Target 资源，引用各自的 GKE 集群及其完全限定资源名称。以下 Target 资源映射到 DeliveryPipeline 资源中定义的 dev、staging 和 prod 环境：

```
```yaml
# For demonstration purposes only, to run this build would require setup beyond the scope of its inclusion
apiVersion: deploy.cloud.google.com/v1
kind: Target
metadata:
  name: dev
description: development cluster
gke:
  cluster: projects/PROJECT_ID/locations/us-central1/clusters/
dev
---
apiVersion: deploy.cloud.google.com/v1
kind: Target
```
```



```
metadata:
  name: staging
description: staging cluster
gke:
  cluster: projects/PROJECT_ID/locations/us-central1/clusters/
  staging
---
apiVersion: deploy.cloud.google.com/v1
kind: Target
metadata:
  name: prod
description: prod cluster
gke:
  cluster: projects/PROJECT_ID/locations/us-central1/clusters/
  prod
```

现在用户已定义各自的目标后，他们就可以开始使用其 DeliveryPipeline 模板来创建单个 Release——该 Release 引用 DeliveryPipeline 模板并同时接收 skaffold.yaml 配置文件。

以下命令展示了用户如何通过 Release 资源将应用程序的新版本部署到所需目标以启动部署：

```
$ gcloud deploy releases create release-01 \
  --project=PROJECT_ID \
  --region=us-central1 \
  --delivery-pipeline=my-demo-app-1 \
  --images=app-01=gcr.io/PROJECT_ID/image:imagetag
```

此资源会为每个 Target 编译成一个 Rollout。

这与仅通过 Cloud Build 执行相比存在明显差异。Cloud Deploy 使您能够将工件通过每个 Rollout 逐步推广，同时在将该工件部署到每个 Target 环境时提供便捷的回滚机制：

![](img/2fe9576240c146c49e3a21d5e7f6af00_231_0.png)

图 11.4 – 创建 Release 时的关系结构

在介绍了 Cloud Deploy 的基本用法后，让我们现在回顾一下 Cloud Deploy 如何使用 Cloud Build 实现其功能。

## Cloud Build 与 Cloud Deploy 之间的关系

Cloud Deploy 中给定 Release 的每个 Rollout 都将 Skaffold 作为单个构建步骤运行，以将适当资源部署到各自的目标环境。

![](img/2fe9576240c146c49e3a21d5e7f6af00_231_1.png)

图 11.5 – Release 和 Rollouts 执行时的关系结构

用户无需导航到 Cloud Build——然而，为了监控和了解其发布的阶段，Cloud Deploy 控制台提供管道可视化，将此信息呈现给用户：

![](img/2fe9576240c146c49e3a21d5e7f6af00_232_0.png)

图 11.6 – Cloud Deploy 中的管道可视化

您可以使用管道可视化导航到每个 rollout，并回滚或推广发布以启动下一个 rollout。

由于对目标的每个 rollout 都是其自己的 Cloud Build 构建，您还可以提供在 rollout 到目标之前要求审批的功能。对于生产等环境，用户可能希望实施双人审批和发布，这非常有用。

启动发布也可以设置在其自己的 Cloud Build 构建步骤中，如以下配置所示：

```yaml
...
- name: gcr.io/google.com/cloudsdktool/cloud-sdk
  entrypoint: gcloud
  args:
    [
      "deploy", "releases", "create", "release-${SHORT_SHA}",
      "--delivery-pipeline", "PIPELINE",
      "--region", "us-central1",
      "--annotations", "commitId=${SHORT_SHA}",
      "--images", "app-01=gcr.io/PROJECT_ID/image:${COMMIT_SHA}"
    ]
...
```

此构建步骤可以单独运行，也可以在其中前面的构建步骤执行 CI 并构建将通过 Cloud Deploy 发布容器镜像的构建中附加。

![](img/2fe9576240c146c49e3a21d5e7f6af00_233_0.png)

图 11.7 – 使用 Cloud Build 启动发布时的关系结构

Cloud Deploy 于 2022 年 1 月发布至通用可用性，是一个有明确观点的资源集合的典型示例，它编排了 Cloud Build 可以执行的工作。它提供了一种独特的方式来实现模式，同时也呈现了可能复杂但相关的任务的可视化。

Cloud Deploy 也反映了常见模式——用户运行多个 GKE 环境和集群。这扩展到用户运行存在于 Google Cloud 以及其他地方的 Kubernetes 集群，例如他们自己的数据中心或托管环境。

### 摘要

展望 Cloud Build 的下一步，我们可以欣赏抽象如何满足不同角色和用户概念。我们回顾了 Cloud Deploy 如何提供抽象，在 Cloud Build 的构建和审批等功能之上简化 GKE 的 CI/CD 管道编写。Cloud Build 将继续扩展其支持用例和模式的数量，增强我们在本书中回顾的功能，以及在此章中特别回顾的新焦点领域。
