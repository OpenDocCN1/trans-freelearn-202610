
## 人工介入与手动审批

与代码一样，在处理与基础设施相关的构建时，可能需要在构建开始前由人工介入并批准。通过像上一节那样将资源分离到不同的构建配置中，我们可以确定在构建流水线执行前哪些资源需要审批。

> **注意**
> 对于本节，我们将通过一个示例进行讲解——它并非 intended for you to copy and paste，因为您的设置和配置可能不同。

在本示例中，我们将使用已连接到 GCP 项目的公开托管 GitHub 仓库。我们需要这样做，因为审批机制仅通过触发器可用。假设您已通过控制台（即 GitHub（Cloud Build GitHub 应用））或您选择的受支持 SCM 提供商连接了仓库，您可以使用以下 CLI 创建触发器。

此示例可在 https://github.com/PacktPublishing/Cloud-Native-Automation-With-Google-Cloud-Build/tree/main/chapter07/terraform 使用 cloudbuild-multiple-network-approval.yaml Cloud Build 配置文件获得。以下是本节的示例命令。仓库作为起点提供，但您的 fork 或仓库布局可能不同：

```
$ gcloud beta builds triggers create github \
  --name=packt-cloudbuild-terraform-approval \
  --repo-name= Cloud-Native-Automation-With-Google-Cloud-Build \
  --repo-owner=PacktPublishing \
  --branch-pattern='^main$' \
  --require-approval \
  --build-config=/chapter07/terraform/cloudbuild-multiple-network-approval.yaml \
  --service-account "projects/**REDACTED-project_id**/serviceAccounts/cloudbuild-network@**REDACTED-project_id**.iam.gserviceaccount.com"
```

正如前一章所述，控制台也可以用于创建触发器。

一旦触发器创建完成，并且构建通过 SCM 提交或在 Cloud Build 中手动 **RUN** 触发，构建将处于等待状态，直到被批准或拒绝。请注意 GCP 控制台 UI 此截图中的 **APPROVE** 和 **REJECT** 选项：

![](img/2fe9576240c146c49e3a21d5e7f6af00_141_0.png)

Figure 7.3 – 等待审批的构建

如果选择 **APPROVE** 选项，将提示用户进行审批确认。

> **注意**
> 要批准或拒绝构建，用户必须在其帐户上分配了 **Cloud Build Approver** 角色。

### 审批构建

审批将导致此构建运行。了解更多

可选消息

approved, this resource is necessary

在构建审批时添加可选消息以显示
36 / 1000

SHOW ADDITIONAL OPTIONS

CANCEL   APPROVE

Figure 7.4 – 审批对话框

一旦在前述对话框中选择了 APPROVE 选项，构建将开始。这也可以使用 gcloud CLI 执行。使用 CLI，用户需要知道要批准或拒绝的构建 ID：

```
$ gcloud alpha builds approve f3da4d17-d832-449a-b925-9ae95245f5b0 \
  --project=**REDACTED-project_id**
```

使用 CLI 执行审批时的示例输出如下：

```
metadata:
  '@type': type.googleapis.com/google.devtools.cloudbuild.v1.BuildOperationMetadata
  ...
    result:
      approvalTime: '2022-04-14T23:45:41.598449Z'
    ...
    decision: APPROVED
  state: APPROVED
```

手动审批允许人工或服务介入以确定是否应执行构建。随着时间的推移，让系统验证并做出决定或最终完全移除审批要求可能变得更可行。其目的是为组织提供一定的灵活性。

## 摘要

Cloud Build 不仅仅用于构建和编译服务和应用程序。该平台的灵活性，包括预构建的 cloud builders、自定义 cloud builders、用户指定的服务帐户和人工审批，可以帮助组织自动化任务。在本章中，利用 Terraform，我们能够使用 IaC 概念在 GCP 上配置资源。

在下一章中，我们将深入探讨在 **Google Kubernetes Engine (GKE)** 中保护您的服务交付。

## 使用 Cloud Build 保护向 GKE 的软件交付

近年来，软件供应链安全已成为行业关注的重点，众多入侵导致了对公司和用户造成严重后果。这种关注催生了更严格的软件交付安全实践，强调了验证所部署工件的信任以及应用最小权限原则（POLP）等实践的重要性。由于 Cloud Build 是在 Google Cloud 中自动化软件交付的推荐机制，了解其安全功能和最佳实践非常重要。

在本章中，我们将通过一个示例讲解如何利用 Cloud Build 将一组应用程序部署到私有 Google Kubernetes Engine（GKE）集群，同时使用 Cloud Build 功能实施多项安全最佳实践。

具体来说，本章将涵盖以下主题：

- 创建构建基础设施和部署目标
- 保护构建和部署基础设施
- 将 POLP 应用于构建
- 配置构建的发布管理
- 启用构建工件的可验证信任

### 创建构建基础设施和部署目标

本示例中的部署目标是一个 GKE 集群。Cloud Build 将我们的软件交付流水线作为构建来实现，同时执行容器镜像的构建以及将 Kubernetes 清单发布到 GKE 集群中运行容器镜像。

在深入了解如何从 Cloud Build 保护向 GKE 的软件交付之前，我们应回顾与软件交付流程相关的 Kubernetes 和 GKE 方面。

Kubernetes (https://kubernetes.io) 是一个流行的 **开源软件 (OSS)** 项目，由 **Cloud Native Computing Foundation (CNCF)** (https://www.cncf.io) 拥有，负责在众多机器上运行容器化应用程序。

在高层次上，它通过软件实现，该软件充当控制平面，协调在注册到控制平面的服务器集上容器化应用程序的生命周期，这些服务器称为 **节点**。这些节点可以由多个容器化应用程序共享，也可以为特定应用程序隔离。此外，Kubernetes 还允许通过 Kubernetes 资源 **命名空间** 对应用程序进行逻辑分离。

这些组件共同构成了一个 Kubernetes 集群，如下图所示：

![](img/2fe9576240c146c49e3a21d5e7f6af00_146_0.png)

Figure 8.1 – Kubernetes 集群的高级架构

控制平面对于本章的示例至关重要，因为它是用户和自动化与集群交互的基础点。它提供了一个网络端点，公开 Kubernetes **应用程序编程接口 (API)**，并运行执行诸如将容器化应用程序接收到集群中等功能的软件。

这意味着控制平面的交互必须安全，我们必须利用 Cloud Build 的安全功能及其准入控制能力。

如果您是 Kubernetes 新手并希望深入了解，建议访问官方文档 (https://kubernetes.io/docs) 了解其运行方式。

GKE 是一个平台，Google 以服务形式向用户提供托管 Kubernetes 集群。GKE 提供的管理级别取决于您使用的集群模式。您可以从以下集群模式中选择：

- **GKE Standard**，其中控制平面由 Google 完全管理，但您仍可控制节点配置和升级生命周期的某些方面
- **GKE Autopilot**，其中控制平面和节点生命周期均由 Google 完全管理

在本示例中，您将使用 GKE 创建一个 GKE Standard 集群，这意味着控制平面将在 Google 拥有的环境中运行并由 Google 完全管理：

![](img/2fe9576240c146c49e3a21d5e7f6af00_147_0.png)

Figure 8.2 – GKE Standard 集群的高级架构

具体来说，您将创建一个私有 GKE Standard 集群。这是对 GKE 集群的配置，其中控制平面只能通过 Google 管理的网络上的私有端点访问，该网络与您自己的 Google 虚拟私有云（VPC）对等。

这意味着我们可以通过在 Cloud Build 中使用私有池来构建和部署到私有 GKE 集群，从而使 Cloud Build 和 GKE 的安全最佳实践保持一致。这可确保 Cloud Build 对 Kubernetes 控制平面的调用不会通过公共互联网传输，而是通过私有网络连接。

### 启用基础 Google Cloud 服务

要开始此示例并创建安全的构建基础设施，您必须首先确保在 Google Cloud 项目中启用所有相关服务。
