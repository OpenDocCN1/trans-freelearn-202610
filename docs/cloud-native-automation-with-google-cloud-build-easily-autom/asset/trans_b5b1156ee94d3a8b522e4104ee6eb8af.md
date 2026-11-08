
虽然前面的文本是 CLI 的输出，但以下截图是可通过 GCP 控制台 UI 访问的审计日志的表示：

![](img/2fe9576240c146c49e3a21d5e7f6af00_60_0.png)

图 3.10 – 通过 GCP 控制台的示例输出审计日志

此审计日志可能具有信息价值，可为安全、运维和开发等不同团队提供谁（个人或自动化的服务账号）创建了构建的访问权限。您可以在以下链接查看 Cloud Build 可用的已审计操作列表，以更深入地了解 Cloud Build 的使用情况：https://cloud.google.com/build/docs/securing-builds/audit-logs#audited_operations。有关审计构建信息的构建来源将在 *第 6 章，管理环境安全* 中介绍。

The `gcloud logging read` command can also be used to view build logs as well as other GCP services.

启动构建对于使用 Cloud Build 至关重要，但了解还可以执行哪些其他操作或可获得何种类型的数据同样重要。

## 摘要

在提供有效的配置清单以及一些 Cloud Build 设置后，执行构建管道可以相当直接。托管服务负责幕后的配置验证、资源配置和构建本身的执行。由于它是一个托管服务，信息会被提供出来以通过各种方式向用户提供状态，无论是通过 GCP 控制台 UI 的视觉方式还是标准 API 集合。

请继续关注下一章，我们将深入探讨 Cloud Build 的清单配置。

## 第 2 部分：拆解构建

在本书的这一部分，我们将深入编写和配置功能齐全的构建。您将了解构成构建配置的核心构建块、这些构建可以执行的各种方式，以及如何设计这些构建以遵循安全最佳实践运行。

本部分包含以下章节：

- *第 4 章，构建配置和架构*
- *第 5 章，触发构建*
- *第 6 章，管理环境安全*

## 构建配置和架构

我们已经讨论了您可以在 Cloud Build 中运行的构建的多样性。作为用户，您能够定义构成构建中每个构建步骤的任务。

实现这一点的方式是根据特定于 Cloud Build 的声明式架构编写构建定义。此架构使您不仅可以定义各个构建步骤，还可以定义它们之间的顺序和关系，以及 Cloud Build 执行环境中的其他特定构建范围配置，例如超时或权限。

在本章中，我们将回顾以下主题：

- 定义构建步骤的最小配置
- 调整构建步骤的默认配置
- 定义各个构建步骤之间的关系
- 配置构建范围规范

### 定义构建步骤的最小配置

在 Cloud Build 中，构建是您作为用户将处理的首要资源。通常，构建是通过根据 Cloud Build 架构用 YAML 编写的配置文件定义的，尽管也支持 JSON。还有一些创建构建时不提供配置文件的例外情况，我们将在第 9 章《使用 Cloud Build 自动化无服务器》中回顾。

让我们回顾一下这些配置文件是什么样子的，从我们已经在一定程度上熟悉的构建开始。

### 设置环境

在上一章中，我们回顾了一个使用以下 `cloudbuild.yaml` 配置文件的简化版本的构建，该文件可在 https://github.com/GoogleCloudPlatform/cloud-build-samples/tree/main/basic-config 找到。

此构建配置文件定义了三个按顺序执行的独立构建步骤：

- 构建 Docker 容器镜像。
- 将 Docker 容器镜像推送到 **Artifact Registry** (https://cloud.google.com/artifact-registry)。
- 创建 **Compute Engine** (https://cloud.google.com/compute) VM 并将之前构建的容器镜像部署到新创建的 VM。

此外，我们可以在此处查看构建的可视化表示：

![](img/2fe9576240c146c49e3a21d5e7f6af00_66_0.png)

图 4.1 – 一个简单的构建，构建容器，将其推送到注册表，并在新 VM 上运行

Google Cloud Platform 的 GitHub 仓库中的内容可能会随时间变化，因此让我们创建自己的副本，以便运行本章中的构建练习所需的文件。让我们利用 **Cloud Shell** (`shell.cloud.google.com`)，这是 Google Cloud 提供的临时 Linux 工作站。

强烈建议您在沙箱或测试 Google Cloud 项目中运行这些练习，因为您不仅可以完全控制运行练习，还可以轻松清理创建的任何资源。

一旦在 Cloud Shell 中打开终端会话，请设置我们将传递给构建配置文件的环境变量：

```
$ PROJECT_ID=$(gcloud config get project)
```

```
$ SERVICE_ACCOUNT=packt-cloudbuild-chp4-sa
```

接下来，创建工作目录以存储本章构建练习的文件：

```
$ mkdir packt && cd packt && \
mkdir cloudbuild && cd cloudbuild && \
mkdir chapter4 && cd chapter4 && \
mkdir first-build && cd first-build
```

从上述构建创建第一个构建配置文件，该文件将构建容器镜像，将其推送到 Artifact Registry，并创建 VM 来运行容器镜像：

```
$ cat > cloudbuild.yaml <<EOF
steps:
  - name: 'gcr.io/cloud-builders/docker'
    args: ['build', '-t',
           'us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage',
           '.']
  - name: 'gcr.io/cloud-builders/docker'
    args: ['push',
           'us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage']
  - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
    entrypoint: 'gcloud'
    timeout: 240s
    args: ['compute', 'instances',
           'create-with-container', 'my-vm-name',
           '--container-image',
           'us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage']
    env:
      - 'CLOUDSDK_COMPUTE_REGION=us-central1'
      - 'CLOUDSDK_COMPUTE_ZONE=us-central1-a'
serviceAccount: 'projects/${PROJECT_ID}/serviceAccounts/${SERVICE_ACCOUNT}@${PROJECT_ID}.iam.gserviceaccount.com'
images: ['us-central1-docker.pkg.dev/${PROJECT_ID}/my-docker-repo/myimage']
options:
  logging: CLOUD_LOGGING_ONLY

EOF
```

> **注意**
> 在处理容器镜像时，最佳实践是使用 `images` 字段指定您正在创建的镜像。这可启用诸如构建来源等功能，我们将在 *第 6 章，管理环境安全* 中讨论。
> 
> 对于在构建运行结束时推送容器镜像即可的简单构建，您可以省略构建此镜像的独立构建步骤，而只指定 `images` 字段。这样将提示 Cloud Build 在构建运行结束时推送该字段中的镜像。
> 
> 然而，在本示例中，由于我们正在创建运行新构建的容器镜像的 VM，因此我们仍然保留独立的构建步骤。

创建 Dockerfile 以用于在此构建中构建的容器镜像：

```
$ cat > Dockerfile <<EOF
FROM alpine
CMD ["echo", "Hello World!"]
EOF
```

在本书前面，我们简要提到构建利用来自 **Identity and Access Management (IAM)** 的 **Service Account** 来执行访问 Google Cloud API 的某些操作。对于本章，我们将创建一个临时服务账号：

```
$ gcloud iam service-accounts create packt-cloudbuild-chp4-sa \
--description="Temporary SA for chp 4 exercises" \
--display-name="packt-cloudbuild-chp4-sa"
```
