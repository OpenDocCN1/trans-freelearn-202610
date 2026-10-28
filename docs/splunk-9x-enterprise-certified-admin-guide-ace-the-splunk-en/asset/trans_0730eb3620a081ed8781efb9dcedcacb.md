

在下一节中，我们将学习 Splunk Enterprise 组件。

## 了解 Splunk 组件

Splunk Enterprise 拥有多个相互协作的核心组件，主要按其功能划分。列表非常全面。独立的 Splunk 部署不需要所有组件；然而，分布式和高可用部署几乎需要所有组件。

关于独立部署与分布式部署的详细理解将在本章的下一节 *Splunk Validated Architectures (SVAs)* 中介绍。到本节结束时，你将熟悉两种类型的 Splunk 组件——即处理组件和管理组件。

## 处理组件

以下是处理组件：

- Forwarder
- SH
- Indexer

让我们详细了解这些组件的作用以及它们与管理组件的关联。

### Forwarder

顾名思义，它主要将数据从源转发到目标 indexer。有两种类型的 forwarder：

- **Universal Forwarder (UF)**
- **Heavy Forwarder (HF)**

UF 通常是安装在数据产生源系统上的软件代理。它包含一个输入配置（即 `inputs.conf` 文件），其中列出了绝对文件路径以及诸如 index 和 sourcetype 之类的元数据字段。UF 是监控和将文件内容转发到指定 indexers 的首选方法。默认情况下，UF 使用 fishbucket 进程来确保数据仅被索引一次，并通过 **cyclic redundancy checks (CRCs)** 和 seek 指针避免数据重复。你将在 *Chapter 9, Configuring Splunk Data Inputs.* 中找到有关额外支持的数据输入的更多信息以及关于 fishbucket 概念的详细解释。

下图说明了 UF 安装在 Web 服务器上，配置为监控 Web 服务器日志并在日志更新时持续将其转发到 indexer：

![](img/5319cbd6f078c6cda7fac6347f77a07d_28_0.png)

Figure 1.1: UF forwarding web server logs to indexer

让我们现在来看 SH，它是分布式部署中一个关键的用户[content]

在下一节中，我们将学习 Splunk Enterprise 组件。

## 了解 Splunk 组件

Splunk Enterprise 拥有多个相互协作的核心组件，主要按其功能划分。列表非常全面。独立的 Splunk 部署不需要所有组件；然而，分布式和高可用部署几乎需要所有组件。

关于独立部署与分布式部署的详细理解将在本章的下一节 *Splunk Validated Architectures (SVAs)* 中介绍。到本节结束时，你将熟悉两种类型的 Splunk 组件——即处理组件和管理组件。

## 处理组件

以下是处理组件：

- Forwarder
- SH
- Indexer

让我们详细了解这些组件的作用以及它们与管理组件的关联。

### Forwarder

顾名思义，它主要将数据从源转发到目标 indexer。有两种类型的 forwarder：

- **Universal Forwarder (UF)**
- **Heavy Forwarder (HF)**

UF 通常是安装在数据产生源系统上的软件代理。它包含一个输入配置（即 `inputs.conf` 文件），其中列出了绝对文件路径以及诸如 index 和 sourcetype 之类的元数据字段。UF 是监控和将文件内容转发到指定 indexers 的首选方法。默认情况下，UF 使用 fishbucket 进程来确保数据仅被索引一次，并通过 **cyclic redundancy checks (CRCs)** 和 seek 指针避免数据重复。你将在 *Chapter 9, Configuring Splunk Data Inputs.* 中找到有关额外支持的数据输入的更多信息以及关于 fishbucket 概念的详细解释。

下图说明了 UF 安装在 Web 服务器上，配置为监控 Web 服务器日志并在日志更新时持续将其转发到 indexer：

![](img/5319cbd6f078c6cda7fac6347f77a07d_28_0.png)

Figure 1.1: UF forwarding web server logs to indexer

让我们现在来看 SH，它是分布式部署中一个关键的用户[content]

在下一节中，我们将学习 Splunk Enterprise 组件。

## 了解 Splunk 组件

Splunk Enterprise 拥有多个相互协作的核心组件，主要按其功能划分。列表非常全面。独立的 Splunk 部署不需要所有组件；然而，分布式和高可用部署几乎需要所有组件。

关于独立部署与分布式部署的详细理解将在本章的下一节 *Splunk Validated Architectures (SVAs)* 中介绍。到本节结束时，你将熟悉两种类型的 Splunk 组件——即处理组件和管理组件。

## 处理组件

以下是处理组件：

- Forwarder
- SH
- Indexer

让我们详细了解这些组件的作用以及它们与管理组件的关联。

### Forwarder

顾名思义，它主要将数据从源转发到目标 indexer。有两种类型的 forwarder：

- **Universal Forwarder (UF)**
- **Heavy Forwarder (HF)**

UF 通常是安装在数据产生源系统上的软件代理。它包含一个输入配置（即 `inputs.conf` 文件），其中列出了绝对文件路径以及诸如 index 和 sourcetype 之类的元数据字段。UF 是监控和将文件内容转发到指定 indexers 的首选方法。默认情况下，UF 使用 fishbucket 进程来确保数据仅被索引一次，并通过 **cyclic redundancy checks (CRCs)** 和 seek 指针避免数据重复。你将在 *Chapter 9, Configuring Splunk Data Inputs.* 中找到有关额外支持的数据输入的更多信息以及关于 fishbucket 概念的详细解释。

下图说明了 UF 安装在 Web 服务器上，配置为监控 Web 服务器日志并在日志更新时持续将其转发到 indexer：

![](img/5319cbd6f078c6cda7fac6347f77a07d_28_0.png)

Figure 1.1: UF forwarding web server logs to indexer

让我们现在来看 SH，它是分布式部署中一个关键的用户面向的处理组件。

HF 是一个 Splunk Enterprise 实例，不需要单独的二进制文件进行安装。与 UF 相比，它提供了扩展的功能集。它不仅收集和转发数据，还包括用于配置和管理的 Splunk 用户界面。要运行 HF，需要 forwarder 许可证。通常，HF 配置为转发模式，通过禁用本地数据存储来实现。可在 Splunkbase 上获得的 Splunk Add-ons 可以安装在 HF 上以便于从各种来源收集数据。这种功能组合使 HF 成为在受益于用户友好界面的同时进行数据预处理和转发数据的多功能选择。

### SH

SH 组件是一个专用于搜索管理的 Splunk Enterprise 实例，并为用户交互提供多种接口。它向用户提供的流行接口是 web、CLI 和 RESTful API。

多个 SH 可以组合在一起形成一个集群，称为 SH 集群（SHC）。SHC 的成员共享相同的基线配置，作业由 SH captain 分配给可用成员。

在独立部署中，单个 Splunk Enterprise 实例（即同一实例）同时充当 SH 和 indexer。在分布式部署模型中，SH 或 SHC 可以向多个 indexers 提交搜索并汇总返回的结果。结果存储在本地的 dispatch 目录中，位于 $SPLUNK_HOME/var/run/splunk/dispatch 以便稍后检索，作业过期后结果将被删除。$SPLUNK_HOME 指的是 Splunk 软件安装的安装目录。例如，临时搜索结果（即搜索作业结果）在 dispatch 目录中保留 10 分钟，作业过期后将被一个名为 dispatch reaper 的进程删除，该进程每 30 秒运行一次。

![](img/5319cbd6f078c6cda7fac6347f77a07d_30_0.png)

Figure 1.3: Indexers receiving data from forwarders and storing it in indexes

到目前为止，我们已经了解了处理组件及其在 Splunk Enterprise 部署中的作用。让我们在下一节中了解管理组件。

## 管理组件

这些是支持处理组件的管理组件：

- Deployment Server (DS)
- SHC Deployer (SHC-D)
- Indexer CM
- License Manager (LM)
- MC

我们将在以下小节中讨论它们。

### DS

使用独立的 Splunk Enterprise 实例来管理 forwarders。位于数据源（通常是 UF）的 forwarders 通常需要新的配置来监控新文件或对现有配置进行更改，然后可选地重启。手动更改它们在大规模基础设施中是一项非常耗时的任务。这就是 DS 发挥作用的地方，它以 apps 的形式维护配置的中央存储库。除了 UF 之外，HFs 也可以使用 DS 进行集中管理。

## 分布式非集群部署

分布式非集群部署比单服务器部署更适合额外的负载和索引容量。SH 和索引职责的分离增加了总拥有成本（TCO）。

下图显示了具有独立 SH 和索引层的非集群部署架构：

![](img/5319cbd6f078c6cda7fac6347f77a07d_34_0.png)

Figure 1.5: Distributed non-clustered architecture

在所示架构中，独立的搜索层包含一个 SHC，索引层包含多个独立的 indexers。SHC-D 是一个强制性的管理组件，负责使用 apps 将配置部署到 SHC。它通过从 SHC-D 向 SHC 推送配置更新的 apps 来促进部署过程。DS 用于管理 forwarders，而 LM 存储许可证信息。DS 确保有效的 forwarder 管理，而 LM 作为许可证详细信息的中央存储库，所有其他实例都连接到它以获取许可证信息。让我们看看这种部署相对于单服务器部署的优势：

- 用户数量将高于单服务器部署，并有额外的 indexer 支持
- 独立的 indexers 将每日索引容量提高到超过 300 GB

现在，让我们看看这种部署的局限性：

- 无 HA 和 DR
- 每次添加新的搜索 peer/indexer 时都需要重新配置 SH
- 当其中一个 indexer 停机时，搜索结果可能不完整，数据摄取也可能受到影响
- 在独立的单 SH 部署场景中，存在 **single point of failure (SPOF)**

让我们看看分布式集群部署，这是一种比分布式非集群部署更先进的设置。

## 分布式集群部署和 SHC – 单站点

单站点分布式集群 SH 和 indexer 部署是一种高可用、 resilient 架构。站点是一个特定区域/地理位置的经典数据中心。

下图显示了具有独立 SHC 和在单站点上运行的集群索引层的集群部署架构：

![](img/5319cbd6f078c6cda7fac6347f77a07d_35_0.png)

Figure 1.6: Distributed clustered deployment and SHC – single-site

![](img/5319cbd6f078c6cda7fac6347f77a07d_37_0.png)

Figure 1.7: Distributed clustered deployment and SHC – multi-site

如 Figure 1.6 所示，每个站点的组件保持相同。然而，收集层在两个站点之间是通用的。每个站点都有一个专用的 SHC。

让我们了解局限性：

- Indexer 集群在站点之间复制数据，这称为跨站点复制，需要较低的网络延迟。100 毫秒或更少是首选。
- SHC 独立工作，不共享工件和通用配置。
- SHC 每个站点有 100 个节点的限制。
- 每个站点都需要一个专用的 SHC-D。
- 一个 CM 节点足以用于跨站点的整个 indexer 集群。

我们已经了解了非常基本的单服务器架构（最好用于测试或开发）和先进的多站点集群部署架构。每种都有其优势、局限性和成本影响。在此阶段，你已经相当熟悉 Splunk 组件和架构。在下一节中，我们将安装一个独立/单服务器部署，我们在这一节的开头就谈到了它。

