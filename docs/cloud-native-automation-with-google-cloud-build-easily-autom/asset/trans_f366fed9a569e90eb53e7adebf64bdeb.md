
## 使用 Cloud Build 自动化无服务器

无服务器平台通常专注于开发者体验，力求在尽可能少的运维开销下让开发者专注于代码。在 Google Cloud 中，Cloud Functions 和 Cloud Run 等平台力求提供无缝的开发者体验，将部署生产就绪代码所需的运维工作降至最低。

尽管如此，这些平台并未忽视某些运维任务的需求，而是提供了简单而强大的抽象，供开发者在生产环境中管理代码使用。这些任务可能包括将代码构建为制品、推出代码新版本、水平扩展代码、通过私有网络连接服务等。

Cloud Build 通常用于编排此类活动。凭借与 Google 无服务器产品原生的集成，Cloud Build 帮助开发者完成这些任务，同时仍忠于无服务器的愿景——专注于代码，而非运维。

在本章中，我们将涵盖以下主题：
- 了解 Cloud Functions 和 Cloud Run
- 无需配置即可构建容器
- 为 Cloud Run 和 Cloud Functions 自动化任务

### 了解 Cloud Functions 和 Cloud Run

我们将首先回顾本章范围内的“无服务器”平台 Cloud Functions 和 Cloud Run。在了解开发者在各自平台上的职责后，我们再介绍如何使用 Cloud Build 来管理这些职责。

### Cloud Functions

当希望以尽可能少的运维开销运行代码时，**Cloud Functions** 是大多数开发者首先选择的起点。在该平台上，开发者可以选择 Cloud Functions 支持的语言运行时并编写小块代码。开发者随后可以将代码部署到 Cloud Functions，并在生产环境中运行，而无需执行以下操作：
- 将代码构建为制品
- 构建运行代码的容器
- 创建运行代码的基础设施
- 管理运行和扩展代码的基础设施
- 设置代码实例的连接性
- 为运行中的代码配置日志记录和监控

相反，开发者可以专注于编写代码，并使用单个 `gcloud` 命令部署到 Cloud Functions：

```
$ gcloud functions deploy function-name --source .
```

Cloud Functions 支持来自包括 **Python**、**Node.js**、**Go** 和 **Java** 在内的语言的特定版本运行时。你可以在此处找到当前支持版本的完整列表：https://cloud.google.com/functions/docs/writing.

使用 Cloud Functions 最常见的模式是编写事件驱动的函数，即它们响应特定事件的发生而执行。这可能是数据库的更改、云资源的创建，或对 Webhook 的响应。

### Cloud Run

**Cloud Run** 旨在为那些已经或将对在容器中运行代码感兴趣，但仍希望开销尽可能少的开发者提供友好的平台。

一旦开发者拥有容器，他们就可以使用单个 `gcloud` 命令将其部署到 Cloud Run：

```
$ gcloud run deploy my-backend --image=us-docker.pkg.dev/project-name/container-image
```

在容器中运行代码的需求或必要性可能来自多项要求：
- 开发者希望确保从开发到生产的多個执行环境中行为一致。
- 企业中的开发者可能需要为代码轻松迁移到不同平台（如 Kubernetes）做好准备。
- 开发者可能将代码部署到多个位置，需要统一的打包标准来部署该代码。

对于这些开发者，他们可能首先需要自行构建容器，然后将其带到运维最少的“无服务器”平台。Cloud Run 提供了这样的平台，允许开发者使用容器作为部署机制，同时仍受益于 Cloud Run 管理以下任务：
- 创建运行代码的基础设施
- 管理运行和扩展代码的基础设施
- 设置代码实例的连接性
- 为运行中的代码配置日志记录和监控

Cloud Run 还允许开发者在“服务”和“作业”之间做出区分。Cloud Run 服务允许通过 HTTPS 端点访问的长期运行服务，而 Cloud Run 作业允许运行至完成的服务，可并行运行实例执行工作，并为每个实例分配一个索引，开发者可利用该索引分配工作。

最后，Cloud Run 允许开发者使服务遵循 Knative API。

Knative 是一个开源项目，使运营者能够在 Kubernetes 集群之上托管自己的自管理无服务器平台。由于 Cloud Run 遵循 Knative API 规范，当用户部署 Cloud Run 服务时，他们还将获得可用于将同一容器部署到任何地方运行的 Kubernetes 集群的配置。

这使得 Cloud Run 不仅是一个无服务器容器平台，而且可以扩展到 Google Cloud 之外的环境，且几乎无需或不需要配置更改。

### Cloud Functions 第二代

有一个较新的 Cloud Functions 版本，名为 Cloud Functions 2nd gen（https://cloud.google.com/functions/docs/2nd-gen/overview），于 2022 年发布。在本章剩余部分，我们将主要与此版本交互。Cloud Functions 2nd gen 的关键变化之一是它构建于 Cloud Run 之上。

这意味着 Cloud Functions 在 Cloud Run 之上提供了一个抽象层，满足构建容器并部署到 Cloud Run 的要求，使不熟悉容器的用户仍可使用该平台。它保持了前一版 Cloud Functions 建立的开发者契约：编写代码，让平台处理其余部分。

与第一个版本一致，部署 Cloud Functions 2nd gen 也可以通过单个命令完成：

```
$ gcloud functions deploy function-name --gen2 --source .
```

此次重平台化在内部带来了多项改进：
- 更长的代码执行超时时间
- 支持对单个代码实例的并发请求
- 支持在代码修订版本之间拆分流量
- 支持更大类型的实例

Cloud Functions 2nd gen 还与 Google Cloud 的 Eventarc（https://cloud.google.com/eventarc/docs）集成，后者是一种托管事件传递服务，提供了数十种开箱即用的来自 Google Cloud 资源的事件支持。

### 比较 Cloud Functions 和 Cloud Run

由于 Cloud Functions 2nd gen 构建为 Cloud Run 之上的抽象层，应注意使用场景可能存在重叠。虽然本章不讨论何时使用其中一个的完整讨论，但应注意一般而言，Cloud Functions 通常执行单一任务，对事件做出某种处理响应。此外，虽然 Cloud Functions 2nd gen 在内部使用 Cloud Run，但你无法自定义其创建和部署的容器。相反，你受限于提供的运行时，并在这些约束内工作。

另一方面，Cloud Run 是一个功能完善的容器即服务平台。这意味着你可以将任何内容构建到容器镜像中并在 Cloud Run 上运行——从 Web 服务到单体应用程序！

接下来，让我们回顾开发者如何简化标准包——容器——的生产，该容器由 Cloud Run 和 Cloud Functions 2nd gen 使用。这种简化有助于在认知上最小化开销，减少个人工作站资源的占用，并避免导致信息孤岛的本地优化。

### 无需构建配置即可构建容器

如果在无服务器上构建的使命是让开发者尽可能轻松地运行软件，那么考虑可以减少认知负担的领域就很重要——尤其是那些不独特或不特定于为其业务提供独特价值的领域。
