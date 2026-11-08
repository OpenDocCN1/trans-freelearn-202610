
## 设置 Cloud Build

借助 Cloud Build 提供的持续部署，对源代码仓库的更改会自动构建为 Container Registry 中的容器镜像并部署到 Cloud Run。

您的代码应监听 $PORT 上的 HTTP 请求。您的仓库必须包含 Dockerfile 或 Go、Node.js、Python、Java、.NET Core 或 Ruby 的源代码，以便构建为容器镜像。

### 源代码仓库

### 构建配置

分支 *
^main$
使用正则表达式匹配特定分支 了解更多

匹配分支：main

构建类型
Dockerfile
Go, Node.js, Python, Java, .NET Core 或 Ruby via Google Cloud Buildpacks
构建上下文目录 *
/
该目录将用作 Buildpack 构建上下文。
入口点
启动服务器的命令，留空以使用默认行为？
保存

图 9.7 – 在 Cloud 控制台中为 Cloud Run 服务启用使用 buildpacks 的构建

通过此设置，您源代码仓库中指定分支的更改现在将触发 Cloud Run 设置的相同构建。虽然不是必须的，但如果您想查看构建或构建日志，可以导航到 Cloud Build 控制台。

![](img/2fe9576240c146c49e3a21d5e7f6af00_196_0.png)

图 9.8 – Cloud Run 在 Cloud Build 中自动创建的三步构建

话虽如此，Cloud Run 还会在 Cloud Run 控制台中直接展示整个服务的构建历史以及每个修订版本的构建日志。

![](img/2fe9576240c146c49e3a21d5e7f6af00_196_1.png)

图 9.9 – Cloud Run 控制台中可用的构建历史

### 主容器（端口 8080，1 CPU，512MiB 内存）

| 属性 | 值 |
|---|---|
| 镜像 URL | us.gcr.io/agmsb-lab/source-to-cloudrun/new-new@sh... |
| 端口 | 8080 |
| 构建 | Cloud Build (logs) |
| 源 | https://github.com/agmsb/source-to-cloudrun/comm... |
| 命令和参数 | (container entrypoint) |
| CPU 限制 | 1 |
| 内存限制 | 512MiB |

图 9.10 – Cloud Run 控制台中可用的构建日志

Cloud Run 在其即用型源到生产功能之下使用 Cloud Build。通过将 Cloud Build 的功能原生地编织到 Cloud Run 资源中，开发人员能够实现无服务器平台所承诺的易用性。

话虽如此，刚刚回顾的方法提供了一种有主见的部署模型，可自动化蓝绿部署的逐步过程。

蓝绿部署是一种方法，其中部署了应用程序的两个版本，并且 1-100% 的流量从旧的“蓝色”版本切换到新的“绿色”版本：

### 修订版本 管理流量

| 名称 | 流量 | 已部署 | 修订版本 URL（标签） | 操作 |
|---|---|---|---|---|
| new-new-00006-pim | 100% (to latest) | 38 分钟前 | green | |
| new-new-00005-rig | 0% | 41 分钟前 | blue | |

然而，Cloud Run 为开发人员带来的亮点之一是对声明式更复杂发布模式的支持。

让我们现在讨论如何使用 Cloud Build 来自动化这些内置于 Cloud Run API 中的发布模式。

## Cloud Run 服务修订版本的渐进式发布

Cloud Run 使开发人员能够直接从其 API 自动化其容器各种修订版本之间的流量整形。使用 gcloud 时，您可以使用特定标志的集合来控制修订版本之间的流量转移。

使用 gcloud run deploy 部署 Cloud Run 服务的新修订版本时，您可以指定以下标志：

- `--no-traffic`
  这确保您刚刚部署的修订版本在部署时不会接收任何流量。

部署后，您可以运行 gcloud run services update-traffic 命令来精细控制服务：

- `--to-revisions`
  使用此标志允许您明确指定发送到服务命名修订版本的流量百分比。
- `--to-latest`
  使用此标志将把所有流量转移到服务的最新修订版本。

虽然您也可以手动运行这些命令，但让我们回顾一下如何将此功能与 Cloud Build 中的源代码管理和触发器集成。

使用 --no-traffic 标志指定您的新修订版本将可用并与您的 Cloud Run 服务关联，但默认情况下不会向其发送流量。

以下示例构建步骤使用此标志：

```
# For demonstration purposes only, to run this build would
require setup beyond the scope of its inclusion
steps:
- id: Rollout version with no traffic
  name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  args:
  - 'gcloud'
  - 'run'
  - 'deploy'
  - 'SERVICE_NAME'
  - '--image'
  - 'gcr.io/PROJECT_ID/IMAGE_NAME:IMAGE_TAG'
  - '--region'
```
```
- 'REGION'
- '--platform'
- 'managed'
- '--no-traffic'
```

这有效地允许蓝绿部署模型，您可以控制切换发生的时间——因为两个修订版本同时与 Cloud Run 服务关联。

这还有额外的好处，即不会产生 VM 或 Kubernetes 集群中蓝绿部署模型通常会产生的额外成本，因为 Cloud Run 仅按计算分钟计费——如果我们的新修订版本不提供流量，则不产生费用。

新修订版本现在已部署，您可以开始实施流量的渐进式发布。--to-revisions 标志允许您明确命名修订版本或指定 LATEST 并定义要定向到该命名修订版本的流量百分比。

以下示例构建步骤使用此标志：

```
# For demonstration purposes only, to run this build would require setup beyond the scope of its inclusion
steps:
- id: Send 20% of traffic to latest version
  name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  args:
  - 'gcloud'
  - 'run'
  - 'services'
  - 'update-traffic'
  - 'SERVICE_NAME'
  - '--to-revisions'
  - 'LATEST=20'
  - '--region'
  - 'us-central1'
```

将此付诸实践被认为是一种金丝雀部署方法，其中您将生产流量的子集发送到新的或“金丝雀”修订版本。在本例中，为生产流量的 20%。

在此期间，您的金丝雀和之前的“稳定”版本共存，通常您会利用这段时间进行金丝雀分析。在金丝雀分析期间，您可以将金丝雀修订版本与稳定修订版本进行比较，观察诸如延迟等指标，以判断金丝雀修订版本是否可以提升以接收所有流量。

当您准备做出此判断时，--to-latest 标志允许您明确将所有生产流量发送到最新修订版本，从而有效地将该修订版本提升到生产环境。

以下示例构建步骤使用此标志：

```
# For demonstration purposes only, to run this build would
require setup beyond the scope of its inclusion
steps:
- id: Send 100% of traffic to latest version
  name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  args:
  - 'gcloud'
  - 'run'
  - 'services'
  - 'update-traffic'
  - 'SERVICE_NAME'
  - '--to-latest'
  - '--region'
  - 'us-central1'
```

此模型也扩展到 Cloud Functions 第二代。虽然 Cloud Functions 第二代抽象掉了用户与 Cloud Run 需要做的许多工作，但流量管理仍然通过 Cloud Run API 进行。

使用 Cloud Build 来编排此操作可以使您手动触发这些相应流程，或根据源代码仓库的更改触发。

流量管理并不是 Cloud Run 和 Cloud Build 可以利用的唯一复杂部署模式。现在让我们回顾一下在部署到 Cloud Run 时如何实施安全最佳实践。

## 使用 Binary Authorization 保护生产环境

鉴于 Cloud Run 允许开发人员理论上部署他们想要的任何容器镜像，管理员如何防止开发人员部署任意镜像，更糟糕的是防止恶意行为者部署有害或恶意镜像？

与我们在第 8 章中为 GKE 回顾的内容类似，使用 Cloud Build Binary Authorization 保护软件交付到 GKE 的功能也适用于 Cloud Run 开发人员。

这是执行容器镜像部署时准入控制的功能，拒绝违反云或安全管理员预定义策略的容器镜像。
