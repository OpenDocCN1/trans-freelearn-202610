## 架构——它是如何工作的？

现在，我们已经从管理层面了解了 Consul 是什么，以及它能为我们做什么，接下来我们将更深入地探讨 Consul 系统的架构，了解它是如何工作的。在本章中，我们将自行搭建一个 Consul 集群，以便更好地理解集群中分发哪些信息、谁负责这些信息，以及它们是如何共享的。本章涵盖的主题包括：

- **Consul 集群运行** —— 我们已经从理论上了解了 Consul 服务器如何作为集群的大脑运作，以及客户端如何监控服务。现在我们将把这一理论付诸实践。
- **建立共识** —— 我们将研究 Consul 服务器如何建立共识以选举领导者并维护服务目录。
- **集群通信协议** —— 集群服务器和客户端之间会发生大量活动。我们将更深入地探讨所有这些信息是如何共享和更新的。
- **地理分布** —— Consul 内部管理着许多决策，而服务位置不仅仅意味着其背后的**互联网协议（IP）**地址。我们将了解 Consul 还能报告哪些其他服务标准。

作为我们学习之旅的一部分，我们将使用 Terraform 在 **Amazon Web Services（AWS）** 中创建自己的 Consul 集群。如果你以前从未使用过 Terraform，也不用担心。我们只需要执行几条基本指令，而且我已经为你写好了所有代码！如果你以前用过 Terraform，请别笑话我的代码；我从来没说过自己是软件开发人员。

## 技术要求

由于你将在此部分中构建自己的集群，因此有一些要求需要满足。不过，我们将在 AWS 中构建所有内容，所以你真正需要的只是能够在命令行环境中运行 Terraform。我将使用 macOS，但 Terraform 可以在大多数桌面操作系统中运行。在接下来的几个小节中，我们将在 AWS 中设置所需内容，并下载必要的 HashiCorp 工具。为了能够跟着我们一起操作，如果你还没有 AWS 账户，就需要创建一个。我们将全程使用 AWS 来创建和管理基础设施。我们已经尽力确保搭建这套基础设施的成本尽可能低，但仍有一些事项需要注意，例如：

- 每天工作结束后，务必执行一次 `terraform destroy` 操作。这确实意味着你第二天继续工作时必须重新构建，但这是关于不可变基础设施的绝佳一课！
- 请务必小心保管你的 AWS 凭证。切勿分享你的**访问密钥 ID**，绝对不要分享你的**秘密访问密钥**。我们会将它们设置为环境变量，这样它们就不会出现在代码中，也绝对不应该出现在你的**版本控制系统（VCS）**中！

## 获取 AWS 凭证

要获取 AWS 凭证，请登录 AWS `https://console.aws.amazon.com/iam/home?#`，然后进入**身份和访问管理（IAM）**。

在页面左侧，你应该会看到一个 **Users（用户）** 区域，你可以在其中使用现有用户或添加新用户。我建议专门为此项目创建一个新用户。以下截图展示了该页面的概览：

![](img/52479dafaefba00517c48a137e037b7e_35_0.png)

图 2.1 – AWS 用户管理

创建一个你满意的用户名，并在访问类型中选择**编程访问**，如*图 2.2* 所示。点击**下一步**按钮，继续该流程：

添加用户

设置用户详细信息

你可以一次性添加多个具有相同访问类型和权限的用户。了解更多

用户名*  h.abiff

+ 添加另一个用户

选择 AWS 访问类型

选择这些用户将如何访问 AWS。访问密钥和自动生成的密码将在最后一步提供。了解更多

访问类型* ☑ 编程访问
为 AWS API、CLI、SDK 和其他开发工具启用访问密钥 ID 和秘密访问密钥。

☐ AWS 管理控制台访问
启用允许用户登录 AWS 管理控制台的密码。

![](img/52479dafaefba00517c48a137e037b7e_35_1.png)

图 2.2 – 创建用户（第 1 步）





下一步，我们将为新创建的用户分配适当的权限。我们假设系统中没有任何组或现有用户，因此我们将附加一个现有策略作为权限集。我们在系统中所做的一切都将利用 **弹性计算云（EC2）** 基础设施，因此请搜索 `AmazonEC2FullAccess` 并为用户选择该选项，如下面的屏幕截图所示：

![](img/52479dafaefba00517c48a137e037b7e_36_0.png)

图 2.3 – 为用户设置权限

既然我们已经分配了权限，接下来要做的是为账户添加一个标签，以便于识别。为我们的基础设施或账户添加标签通常是可选的。然而，为了便于识别，这始终是一个好习惯，所以添加一个标签（让它有趣一点！），如下面的屏幕截图所示，然后再次点击 **下一步** 按钮：

![](img/52479dafaefba00517c48a137e037b7e_36_1.png)

图 2.4 – 为用户添加标签

请继续跟着我们……我们快完成了！你接下来看到的屏幕将是对新账户信息的审核。如果你像乖孩子一样一路跟着做，你会看到与此非常相似的内容：

## 添加用户

## 审核

审核你的选择。创建用户后，你可以查看并下载自动生成的密码和访问密钥。

### 用户详细信息

| 用户名 | h.abiff |
| AWS 访问类型 | 编程访问 - 带访问密钥 |
| 权限边界 | 未设置权限边界 |

### 权限摘要

以下策略将附加到上面显示的用户。

| 类型 | 名称 |
| 托管策略 | AmazonEC2FullAccess |

### 标签

新用户将收到以下标签

| 键 | 值 |
| 用途 | Up and Running |

图 2.5 – 用户创建审核

到目前为止，我希望你已经点击了最后一个 **下一步** 按钮（好吧，至少是暂时的），并且可以为一个全新用户的创建而欢欣鼓舞！我不知道你怎么样，但当我看到一个大大的 **成功** 标志和绿色复选框时，我总是感觉更好。不过，这里更重要的是已创建并分配给你的新用户的访问凭证，如下面的屏幕截图所示：

![](img/52479dafaefba00517c48a137e037b7e_38_0.png)

图 2.6 – 用户访问凭证

好的——现在你已经创建了新用户，你拥有了一组非常具体且非常重要的凭证，任何凡人要记住这些凭证即使不是不可能，也是非常困难的。为了方便保存这些信息，我建议你下载 **逗号分隔值（CSV）** 文件并将其保存在一个 *非常* 安全的位置。这不是你想与你最喜欢的表情包伙伴分享的东西，尤其是如果他们像我朋友那样的话！

在该 CSV 文件中，你会找到你的 **用户名**、**访问密钥 ID** 和 **秘密访问密钥**。是的——里面还有其他一些内容，但那些不那么有用。我在这里提供了一个 CSV 文件的示例视图：

| 用户名 | 密码 | 访问密钥 ID | 秘密访问密钥 | 控制台登录链接 |
| h.abiff |  | AKIA26CFBA32D6JFVJJB | NYaSFhAzZqQq4bqSGIda/Y22Guc3pB2WCA8R9jVj | https://751764047604.signin.aws.amazon.com/console |

图 2.7 – 访问凭证 CSV 文件内容

是的——我明白我刚刚通过分享我的凭证把自己暴露给了阅读这本书的任何人，而我说过你绝不应该这样做。不过，我刚刚吊销了那些凭证，所以连编辑都没有机会！

现在，我们可以选择如何处理这些数据，即使我们不把它发布到 InstaFace 上。为了在 Terraform 中使用这些凭证，我们需要用它们的内容创建环境变量。我们现在将看到一些示例，以图 2.7 中显示的凭证为例。

> **注意**
> 这些凭证也可以保存在 Terraform 直接使用的文件中；然而，这有时会导致凭证被共享或无意中分发。然后，在你意识到之前，你就收到了一张巨额云计算账单，却没有任何比特币可以展示！

一种选择是手动将凭证值复制并粘贴到 export 语句中，如下所示：

```
$ export AWS_ACCESS_KEY_ID=AKIA26CFBA32D6JFVJJB
```

```
$ export AWS_SECRET_ACCESS_KEY=NYaSFhAzZqQq4bqSGJda/Y22Guc...
```

或者，我们可以使用一些 Bash 工具创建环境变量（假设你与下载的文件在同一目录中），如下所示：

```
$ export AWS_ACCESS_KEY_ID=$(tail -n 1 new_user_credentials.csv | awk -F ',' '{print $3}')
```

```
$ export AWS_SECRET_ACCESS_KEY=$(tail -n 1 new_user_credentials.csv | awk -F ',' '{print $4}')
```

如果你能够验证环境中的值与凭证文件中的值相同，你就知道做对了。你可以使用以下命令进行验证：

```
$ env | grep AWS
```

```
AWS_ACCESS_KEY_ID=AKIA26CFBA32D6JFVJJB
```

```
AWS_SECRET_ACCESS_KEY=NYaSFhAzZqOq4bqSGJda/Y22Guc3p...
```

另外，如果你使用的是 Terraform Cloud，你可以将它们作为工作区的环境变量输入，如下面的屏幕截图所示。但是，请记住，我们将主要使用 **命令行界面（CLI）** 进行工作，所以除非你熟悉 Terraform 远程后端，否则建议直接使用 Terraform CLI：

![](img/52479dafaefba00517c48a137e037b7e_40_0.png)

图 2.8 – Terraform Cloud 环境变量

是的！所有这些工作只是为了确保你有足够的凭证，并且它们位于正确的位置。

## 安装 Terraform 和 Packer

我们快到了……你能感觉到吗？请务必在这里仔细跟着做。我们将使用 Packer 为我们的项目创建基础镜像，并使用 Terraform 创建基础设施。

要安装 Terraform，首先从 Terraform 下载页面 `https://www.terraform.io/downloads.html` 下载适合你操作系统的 ZIP 文件。

要安装 Packer，首先从 Packer 下载页面 `https://www.packer.io/downloads` 下载适合你操作系统的 ZIP 文件。

作为参考，本书中的材料是使用 Terraform 0.14.8 版本和 Packer 1.7 版本创建的。

记住文件保存的位置，下载完成后，解压缩这些文件。你将得到一个 `terraform` 可执行文件和一个 `packer` 可执行文件，你需要将它们保存到一个合适的“家”中，并确保这个“家”在你的命令行路径中。

就是这样！现在，你可能会问：Rob，*如果下载和使用 Terraform 这么简单，为什么不是每个人都在用呢？* 这是一个非常好的问题，我亲爱的读者……*一个非常好的问题。*

## 什么是集群！

如果你刚刚设置了 AWS 账户并安装了 Terraform，我可以想象你正迫不及待地想要启动一个集群。别担心——正如 Robert Plant 常说的，*你的时机终将到来。* 首先，我们将更深入地研究 Consul 集群操作本身所采用的机制。我之前可能提到过，Consul 的核心应用之一是促进并实现网络中所有服务之间更简单、更安全的通信机制。如果 Consul 无法掌控其内部通信，它永远无法实现外部通信。因此，让我们来看看整个 Consul 集群（包括服务器和客户端）中的通信机制和流程。

从服务器开始，它们需要共享和通信的基本信息是什么？以下是必需的内容：

- 1. 领导者选举——毕竟，没有领导者，就会陷入无政府状态，服务将在街头燃烧。
- 2. 客户端注册——服务器显然需要知道客户端何时加入集群。
- 3. 服务状态和位置——服务器需要知道集群中部署的每个服务的状态及其位置。

为了选举领导者，服务器节点必须达成共识。它们使用一种称为——嗯——**共识**的协议来做到这一点。我们将在接下来的章节中更详细地研究这个协议，但本质上发生的情况是，当服务器节点启动时，会进行一次选举。如果服务器没有收到有人站出来成为领导者的通知，它就会选举自己。如果其他人先选举了自己，那么该服务器就会投票给最初的领导者。最终，所有服务器将达成 *共识*，并产生一个无可争议的领导者。同样，我们将在后面的章节中更详细地了解这是如何工作的，并讨论潜在的问题。




已部署服务的状态及其位置不仅是服务器需要的信息，也是分布在整个网络中的客户端需要的信息。现在，这种级别的通信并不是什么新鲜事，对吧？在几乎任何分布式系统中，服务器需要了解客户端的状况，以及客户端在几乎任何分布式系统中的作用。然而，随着这些系统的扩展，最终会有成百上千甚至上万个客户端同时尝试与服务器通信。最终，服务器领导者将敲响法槌，要求法庭保持秩序！不过，在 Consul 中，服务器和客户端都使用一种名为——嗯——**gossip** 的协议相互传播信息。这些协议名称背后的想象力真是令人惊叹。利用 gossip 协议，客户端与相邻客户端共享关于自身服务和状态的信息，而服务器则在一旁监听。你可以在*图 2.9*中看到此过程的概览：

![](img/52479dafaefba00517c48a137e037b7e_42_0.png)

那么，既然我们让所有人都开始交谈，他们到底在谈些什么？我也想知道！别把我排除在外！为了勉强保持某种重点，我们将先讨论服务器的角色，然后再讨论客户端。

好了——既然我们基本了解了大型集群内部发生的情况，让我们更深入地研究一下促成这一切的协议！

## Raft 共识

我们已经看到 Consul 服务器如何利用**共识**协议选举领导者，但这到底意味着什么？这就是它们的全部工作吗？嗯，当然不止；否则这一节就会非常简短。Consul 使用的共识协议基于 Raft 算法（https://raft.github.io/raft.pdf），而 Raft 又基于 Paxos（https://en.wikipedia.org/wiki/Paxos_%28computer_science%29）。如果你觉得我们正在迅速陷入兔子洞，别担心——我们不会一直挖到水为止。

## 深入探讨领导者选举

你很快就会看到，一个 Consul 服务器会与若干其他服务器一起配置，并与它们组成 Raft。这在配置中被称为 `bootstrap_expect` 值。记住这个服务器数量很重要，因为它稍后还会出现。当然，如果一个 Consul 服务器期望与若干服务器组成 Raft，它需要知道它们，因此这些信息也会被配置。为了成为领导者，服务器需要获得 Raft 中大多数服务器的选举——例如，如果你期望 Raft 中有五台服务器，你至少需要三台投票给你。这被称为法定人数，对于服务器集群的可用性来说是一个关键概念。就像我们参加的许多会议一样，如果没有法定人数的成员，你就无法真正做出任何决定。在某些会议上，即使有法定人数，如果没有领导，你也无法做出任何决定！如果你不确定需要多少台服务器才能达到法定人数，请参考以下公式：

(服务器数量)/2 + 1 = 法定人数

如果服务器数量等于法定人数规模，那么集群就没有保护机制。当然，你不可能有半台服务器，所以如果结果有小数，直接向下取整。图 2.11 中的表格也许也有帮助：

| 服务器数量 | 法定人数 | 故障容忍度 |
|---|---|---|
| 1 | 1 | 0 |
| 2 | 2 | 0 |
| 3 | 2 | 1 |
| 4 | 3 | 1 |
| 5 | 3 | 2 |
| 6 | 3 | 2 |
| 7 | 4 | 3 |

图 2.11 – 法定人数不容有失

那么，让我们来看看选举是如何进行的。如果你想看这个过程非常酷的动画演示，下面这个网站很棒：http://thesecretlivesofdata.com/raft/。

我们以三台服务器为例来走一遍选举：**Bart**、**Lisa** 和 **Maggie**。**Bart** 是第一台“觉醒”的服务器，因此他提名自己为*候选人*，并向 **Lisa** 和 **Maggie** 发送请求，希望能成为*领导者*。然而，在 **Lisa** 听到 **Bart** 的请求之前，她已经向 **Bart** 和 **Maggie** 发送了自己的候选人提名及领导者请求，因为我们都知道 **Lisa** 更聪明。

你可以在图 2.12 中看到该过程的概览：

![](img/52479dafaefba00517c48a137e037b7e_45_0.png)

图 2.12 – Lisa 的选举提名

太棒了——我们的集群现在有了领导者！然而，服务器领导者的选举只是 Consul 实际使用共识协议的目的之一——毕竟，如果一个领导者不分享它学到的信息，那它还算什么领导者呢？

> **注意**
> 
> 虽然这个过程只有三台服务器时可以非常迅速，但它只能容忍一台 Consul 服务器的故障。你可能会想：*哇，那我就塞进 12 台服务器，总该没问题了吧！* 但凡事有取舍——随着 Raft 中服务器数量的增加，达到法定人数所需的时间也会增加。这不仅影响领导者选举，还影响数据一致性。通常，建议将服务器对等集保持在五到七台之间。

## 服务器对等集中的数据共享

现在 **Lisa** 已成为无可争议的领导者，她负责维护与 **Bart** 和 **Maggie** 共享的信息。这些信息通过 **Lisa** 写入的日志条目进行记录和共享。这些日志条目包含了服务目录和领导者选举过程的状态，以及服务器对等集运作所需的一切。是时候开始剥洋葱了，但我希望它不会让你流泪。

在对等集内，任何服务器都可以接收来自客户端的信息，并将这些信息共享给领导者。这些信息可能包含直接节点上服务状态的变更，甚至对集群内其他节点的更新。只有领导者才能更新日志条目；然而，仅仅因为领导者 **Lisa** 知道了服务目录或其他组件的某些调整，并不意味着 **Maggie** 或 **Bart** 也拥有相同的信息。这时我们的法定人数概念再次派上用场。正如*群体思维*一样，对等集中最少数量的服务器需要就当前时间点的现实情况达成一致。因此，当 **Lisa** 对日志文件写入更新时，她会将更新同时发送给 **Bart** 和 **Maggie**。直到 **Bart** 或 **Maggie** 确认他们已应用相应的日志条目且日志已被正确*复制*，该条目才被视为已*提交*。然而，一旦 **Lisa** 获得确认，并且服务器对等集中至少有一个其他成员（记住——共有三个成员，所以我们需要两个才能达到法定人数），我们就可以认为该日志条目已提交，并且我们对现实有了新的视角。

所有这些交换都利用了远程过程调用（RPC）和共识机制，如图 2.15 所示。

![](img/52479dafaefba00517c48a137e037b7e_48_0.png)

图 2.15 – 对等集中的共识与 RPC 连接

我知道你可能会想：好吧，Rob，但是如果日志变得太大怎么办？这是一个合理的担忧，可以通过服务器对等集中的公共有限状态机（FSM）来管理。当日志在服务器之间复制和提交时，FSM 会更新为当前状态。当对 Consul 服务器对等集发起查询时，领导者会根据 FSM 的状态响应查询。Consul 的结构使得任何一组日志的重放都会产生完全相同的状态。哇——如果人类也能如此可预测，那该多好啊！然而，正是这种状态的创建和维护，使 Consul 能够定期对日志条目进行快照，以管理复制日志的大小。这种快照不会中断 Consul 服务，并确保对等集的长期稳定。

## 传播流言的 Serf

既然我们对服务器对等集的建立和运作有了更多了解，让我们更深入地看看大家都在传播什么。再次，如果我们开始解析 gossip 协议的来源，它使用 Serf（https://www.serf.io/）库，该库基于可扩展的弱一致感染式进程组成员协议，幸运的是它被缩写为 SWIM（http://www.cs.cornell.edu/info/projects/pringlass/public_pdfs/swim.pdf）。我们从 Raft 上跳下来，去游泳了！



请看以下示意图：

![](img/52479dafaefba00517c48a137e037b7e_49_0.png)

图 2.16 – Gossip 连接

Consul 集群中的所有成员，无论是服务器还是客户端，都处于同一个“池子”中。这样可以实现对池中所有成员的分布式发现、它们的状态，当然，还包括注册到 Consul 客户端的任何服务的状态。虽然 gossip 是让所有代理了解集群成员和服务变更的方式，但所有信息仍然是通过直接 RPC 从代理发送到服务器节点的。请记住，peer 集合中的任何服务器都可以接收更新，并将其转发给领导者，以便进行法定人数提交和对状态机的更新。

## 纬度的变化

我们已经了解了 Consul 如何在集群内共享有关领导者选举、服务可用性变化和节点可用性变化的信息。除了这些项目之外，Consul 始终知道集群中每个节点相对于其他节点的地理位置。这有助于做出智能决策，例如定位离请求实体最近的可用的服务实例。发现机制利用了 Serf 库提供的网络断层扫描技术。没错——它们不仅互相 gossip 彼此的状态，还知道彼此的位置！奇点即将来临！

## 建立我们的第一个 Consul 集群

关键时刻到了！在本章开头，有创建 AWS 账户凭据以及下载 Terraform 的说明。现在，你终于可以把这些工作派上用场了！

> **注意**
> 
> 本书涵盖了 Consul 在服务器和客户端操作方面的内容。HashiCorp 还提供了托管式 Consul 服务，即 **HashiCorp 云平台（HCP）**，在您自己的生产环境中部署时，这可能是理想的选择。然而，为了理解服务器操作，我们将自行操作。有关 HashiCorp 托管 Consul 服务的更多信息，请访问 `https://portal.cloud.hashicorp.com/`。

本节的所有代码都在以下 GitHub 仓库中：

https://github.com/PacktPublishing/Simplifying-Service-Management-with-Consul

您可以 fork 该仓库，或者直接下载它并跟着操作。我们将使用该仓库中的代码为每一章构建我们的系统，相关代码位于相应的章节文件夹中。

以下各节包括对我们用来创建 Consul 集群的 Packer 和 Terraform 代码的回顾。没有必要理解代码的每个方面，但我还是想包含一些解释，以求完整。当您继续操作时，请仔细注意以 `$` 符号表示的命令。

## 构建镜像

我们将从构建基础镜像开始。为了让我们所有人——毫不夸张地说——在使用镜像和操作 Consul 方面保持一致，我们将一起使用 Packer 构建一个镜像。但别担心——你需要的所有代码都在仓库里。在 ch2 文件夹中，您会找到另一个名为 Image-Creation 的文件夹。我知道——这个名字很有创意。以下是进入该文件夹所需的代码：

```
```sh
$ cd $github_root/Consul-Up-and-Running/ch2/Image-Creation
```
```

这里有几个关键项需要注意。您的构建变量都在 `variables.pkr.hcl` 中定义，如下面的代码片段所示：

```
```hcl
variable "owner" {
  type        = string
```
```

```
description = "Owner tag to which the artifacts belong"
default     = "packt-consul"
}
variable "consul_version" {
    type = string
    description = "Three digit Consul version to work with"
    default = "1.9.0"
}
variable "aws_region" {
    type        = string
    description = "AWS Region for image"
    default     = "us-east-2"
}
variable "aws_instance_type" {
    type        = string
    description = "Instance Type for Image"
    default     = "t2.small"
}
```

对于这些变量中的大多数，默认值就可以了。但是，如果您想更改任何默认值，请在 variables.pkrvars.hcl 中进行修改。例如，请看下面的片段：

```
owner = "rjackson"
```

是的——自定义变量就是这么简单！定义我们要构建内容的 Packer 文件名为 AWS_linux_image.pkr.hcl。让我们快速看一下该文件，看看我们要构建什么。

第 1 到 19 行定义了我们将用来构建 Consul 镜像的基础镜像。我们利用上述变量文件为名称和区域设置了变量。我们还在 AWS 中筛选合适的 Ubuntu 镜像，如下面的代码片段所示：

# AWS_linux_image.pkr.hcl

```
source "amazon-ebs" "ubuntu-image" {
    ami_name = "${var.owner}-consul-{{timestamp}}"
    region   = "${var.aws_region}"
```

```
instance_type = var.aws_instance_type
tags = {
    Name = "${var.owner}-consul"
}
source_ami_filter {
    filters = {
        virtualization-type = "hvm"
        name = "ubuntu/images/*ubuntu-bionic-18.04-amd64-server-*"
        root-device-type = "ebs"
    }
    owners = ["099720109477"]
    most_recent = true
}
communicator = "ssh"
ssh_username = "ubuntu"
}
```

第 21-58 行实际构建镜像，包括我们需要的所有配置。本质上，我们是获取搜索到的初始镜像，并将我们需要的内容打包进去——因此得名 Packer。在此过程中，我们会移动几个文件供 Consul 使用：一个定义如何在 Ubuntu 环境中管理 Consul 的服务文件，以及一个将应用于所有 Consul 节点的通用配置文件。我们还执行一些内联配置，指示 Packer 通过 ssh 连接到节点，并在设置过程中执行一组命令。代码如下面的片段所示：

```
build {
    sources = [
        "source.amazon-ebs.ubuntu-image"
    ]
    provisioner "file" {
        source      = "../files/consul.service"
        destination = "/tmp/consul.service"
    }
    provisioner "file" {
        source      = "../files/consul-common.hcl"
        destination = "/tmp/consul-common.hcl"
    }
```

```
```hcl
}
provisioner "shell" {
  inline = [
    "sleep 30",
    "sudo apt-get update",
    "sudo apt install unzip -y",
    "sudo apt install default-jre -y",
    "curl -k -O \"https://releases.hashicorp.com/consul/${var.consul_version}/consul_${var.consul_version}_linux_amd64.zip\"",
    "unzip consul_${var.consul_version}_linux_amd64.zip",
    "sudo mv consul /usr/local/bin"
  ]
}
provisioner "shell"{
  inline = [
    "sudo /usr/local/bin/consul -autocomplete-install",
    "sudo useradd --system --home /etc/consul/consul.d --shell /bin/false consul",
    "sudo mkdir /etc/consul /etc/consul/consul.d /etc/consul/logs /var/lib/consul/ /var/run/consul/",
    "sudo chown -R consul:consul /etc/consul /var/lib/consul/ /var/run/consul/",
    "sudo chmod -R a+r /etc/consul/logs/",
    "sudo mv /tmp/consul.service /etc/systemd/system/consul.service",
    "sudo mv /tmp/consul-common.hcl /etc/consul/consul.d/consul-common.hcl"
  ]
}
}
```

其中一个配置文件包含我们希望在所有 Consul 节点上使用的通用设置。就我们而言，我们只将其设置为日志设置。我们将这些设置为 DEBUG，以便我们能深入了解 Consul 正在做什么！请看下面的代码片段：

# consul-common.hcl

```
log_file = "/etc/consul/logs/"
log_level = "DEBUG"
```

此时，您可能需要再次确认您的 AWS 环境变量已设置。您可以使用以下命令进行检查：

```
$ env |grep AWS
AWS_ACCESS_KEY_ID=AFAIKJENNY
AWS_SECRET_ACCESS_KEY=8675309...
```

现在，我们准备构建！执行下面代码片段中显示的那个简单的 Packer 命令，然后花几分钟为自己打包了一个镜像而祝贺吧！选择您喜欢的饮料庆祝一下，放松一下，因为该过程大约需要 5 分钟：

```
$ packer build -var-file="variables.pkrvars.hcl" .
```

查看输出，它看起来就像有人把圣诞灯扔满了您的终端。Packer 的输出全是红色的，并且有各种深浅不一的绿色，只是为了营造节日气氛。您会看到文件被移动，操作系统也在更新，以确保我们拥有所有最新的 Ubuntu 软件包。您还会看到一些关于使用 shell 脚本进行配置的行。Packer 会根据我们在 Packer 构建定义中看到的那些 shell 配置块来创建 shell 脚本。

在过程结束时，您会看到新镜像的位置和 Amazon 系统映像（AMI）ID！如下面的片段所示。现在，您可以在自己的镜像之上构建您的 Consul 知识：

```
==> Wait completed after 5 minutes 52 seconds
==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs.ubuntu-image: AMIs were created:
us-east-2: ami-079a52bd81f91b201
```

同样，无需记住该镜像 ID，因为在下一步中，Terraform 会为我们找到它！




## 构建 Consul 集群

首先，切换到 ch2 的根目录。在这里，我们将找到用于构建集群的 Terraform 脚本。你可以通过运行以下命令来完成此操作：

```
$ cd $github_root/Consul-Up-and-Running/ch2/
```

与构建 Packer 镜像时类似，此目录中有几个文件需要我们注意。暂时不用关心 files 目录，等我们评估用于构建集群的 Terraform 脚本时再深入探讨。

如果你查看我们的 variables.tf 文件，这里定义了 Terraform 所需的变量。我总是分不清变量在哪里定义、在哪里赋值。总之，对于这五个变量，每个都有描述（这总是一个好习惯）和一个合理的默认值（好吧，从我的角度来看是合理的）。请看下面的代码片段：

### variables.tf

```
variable "aws_region" {
  type        = string
  default     = "us-east-1"
  description = "Region for AWS Components"
}
variable "owner" {
  type        = string
  default     = "consul-demo"
}
variable "consul_region" {
  type        = string
  description = "Region of consul server (not AWS Region)"
  default     = "global"
}
variable "vpc_cidr" {
  type        = string
  description = "CIDR of the VPC"
  default     = "192.168.100.0/24"
}
variable "instance_type" {
```

```
type          = string
description   = "machine instance type"
default       = "t3.micro"
}
```

如果你更愿意为必要的变量使用自己的值，别担心——我不会介意的。这些变量在 terraform.tfvars 文件中设置。请看下面的代码片段：

### Terraform.tfvars

```
owner         = "rjackson"
aws_region    = "us-east-2"
instance_type = "t2.small"
```

好了——现在我们已经按照自己的喜好设置好了变量，是时候启动我们的基础设施了！如果你以前从未使用过 Terraform，别担心——它非常简单，而且所有代码都已构建完成。再说一次，我从不自称是软件开发人员，所以请不要嘲笑我的代码！首先，我们要启动基础设施的构建，然后剖析我们构建的内容。要开始，Terraform 需要下载任何必要的特定于提供程序的数据。这通过以下命令执行：

```
$ terraform init
```

当然，在不检查将要构建的内容的情况下就进行构建从来都不是好事，所以接下来，我们要规划我们的构建。此命令的所有输出都应该是绿色的加号（+），表示将要创建哪些基础设施。请注意，其中许多项目在 apply 操作之后才能确定，因为它们需要从 AWS 分配，但浏览一下你将要构建的内容可能会很有趣。如果不感兴趣，别担心——我们会在启动构建后审查细节。

现在，运行以下命令：

```
$ terraform plan
```

接下来，我们要应用我们的基础设施构建。这通过你猜对了的 terraform apply 来完成。这会再次触发一个 plan，并显示你将要构建的内容，但既然我们已经做过这件事了，那就来个危险的操作吧，直接自动批准。请看下面的代码片段：

```
$ terraform apply --auto-approve
```

> 注意
> 对于你在本练习中产生的任何费用，作者概不负责。请务必在白天或夜晚结束时销毁你的基础设施。如果你想设置一个 m3.xlarge 系统并让它连续运行几天，那随你便！

好的——让我们来看看我们在构建什么。以下所有代码块（本节中的）都在 main.tf 文件中。Terraform 的最佳实践是将所有代码片段放在一个文件中，所以我们要做的就是这样！

第一部分相当简单，只是设置了我们将要使用的 Terraform 提供程序。Terraform 提供程序有点像提供特定于该提供程序的功能的模块——例如，我们正在安装 AWS 提供程序，使我们能够管理 AWS 中的资源。我们指定了该提供程序的版本，以及我们将要工作的区域。provider 块也是调皮的人会插入其 AWS 凭据的地方，但我们永远不会那样做——对吧？？？最后，我们使用 AWS 数据源来查找我们刚刚使用 Packer 构建的镜像。以下是您需要的代码：

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 2.5"
    }
  }
}
provider "aws" {
  region = var.aws_region
}
data "aws_ami" "an_image" {
  most_recent = true
  owners      = ["self"]
  filter {
    name   = "name"
    values = ["${var.owner}-consul-*"]
  }
}
```

下一个部分创建了一个私钥，供我们用来 SSH 登录到我们的机器。一个组织通常会维护自己的私钥用于机器访问，但这些私钥确实不应该随意流传。更安全的方法是使用 HashiCorp Vault Secure Shell (SSH) 助手来生成一次性 SSH 密钥，但那是另一个故事了。密钥将构建在我们的 Consul 机器内部，但我们也会有一个以 -consul-key.pem 结尾的本地 Privacy Enhanced Mail (PEM) 文件。您需要的代码如下面片段所示：

```
resource "tls_private_key" "my_private_key" {
  algorithm = "RSA"
  rsa_bits = 4096
}
resource "local_file" "private_key" {
  content = tls_private_key.my_private_key.private_key_pem
  filename = "${var.owner}-consul-key.pem"
  file_permission = 0400
}
resource "aws_key_pair" "consul_key" {
  key_name = "${var.owner}-consul-key"
  public_key = tls_private_key.my_private_key.public_key_openssh
}
```

本部分提供了我们系统所需的大部分网络信息。由于我们不是在阅读关于 AWS 网络复杂细节的书，你就得相信我，下面片段中的所有代码都很酷：

```
resource aws_vpc "consul-demo" {
  cidr_block           = var.vpc_cidr
  enable_dns_hostnames = true
  tags = {
    Name = "${var.owner}-vpc"
  }
}
resource aws_subnet "consul-demo" {
  vpc_id     = aws_vpc.consul-demo.id
  cidr_block = var.vpc_cidr
  tags = {
    name = "${var.owner}-subnet"
```
```
}
}
resource aws_internet_gateway "consul-demo" {
  vpc_id = aws_vpc.consul-demo.id

  tags = {
    Name = "${var.owner}-internet-gateway"
  }
}
resource aws_route_table "consul-demo" {
  vpc_id = aws_vpc.consul-demo.id
  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.consul-demo.id
  }
}
resource aws_route_table_association "consul-demo" {
  subnet_id     = aws_subnet.consul-demo.id
  route_table_id = aws_route_table.consul-demo.id
}
```

好的——这部分网络内容可不是能轻易略过的。AWS 安全组确定了哪些设备之间开放哪些端口。以下是已定义端口及其用途的列表：

-   端口 22—SSH 访问。
-   端口 8300-8302—这些涵盖了节点之间所有的共识、RPC 和 gossip 通信。
-   端口 8500—用于 Consul 的 Web 用户界面（UI）。
-   端口 8600—Consul 利用端口 8600 进行域名系统（DNS）查询，作为服务发现功能的一部分。

这些端口都已在安全组配置中设置并配置完毕。以下是说明这一点的代码：

```
resource aws_security_group "consul-demo" {
  name   = "${var.owner}-security-group"
  vpc_id = aws_vpc.consul-demo.id
```

```
ingress {
    from_port    = 22
    to_port      = 22
    protocol     = "tcp"
    cidr_blocks  = ["0.0.0.0/0"]
}
ingress {
    from_port    = 8300
    to_port      = 8302
    protocol     = "tcp"
    cidr_blocks  = ["0.0.0.0/0"]
}
ingress {
    from_port    = 8500
    to_port      = 8500
    protocol     = "tcp"
    cidr_blocks  = ["0.0.0.0/0"]
}
ingress {
    from_port    = 8600
    to_port      = 8600
    protocol     = "tcp"
    cidr_blocks  = ["0.0.0.0/0"]
}
egress {
    from_port        = 0
    to_port          = 0
    protocol         = "-1"
    cidr_blocks      = ["0.0.0.0/0"]
    prefix_list_ids  = []
}
tags = {
    Name = "${var.owner}-security-group"
}
}
```


好了，现在进入正题！在本节中，我们将创建三个 Consul 服务器，使用我们通过数据源找到的 AMI（即我们用 Packer 构建的 AMI）。我们使用 Terraform 脚本中创建的 SSH 密钥，并分配了一个公网 IP 地址，以便能够访问服务器。在脚本底部，我们为服务器关联了标签，以便轻松找到它们。帮你自己和你的 IT 运维团队一个忙：永远不要在未关联逻辑标签的情况下部署基础设施！其中一个标签还能唯一标识实例，我们可以用它来唯一识别服务器。在标签正上方，有一个 `user_data` 字段。这非常重要，因为它调用模板文件 `server_template.tpl` 来配置 Consul 服务器。我们还将 `server_name_tag` 属性传递给模板文件，以帮助识别服务器。代码如下所示：

```
resource aws_instance "consul-server" {
  count                         = 3
  ami                           = data.aws_ami.an_image.id
  instance_type                 = var.instance_type
  key_name                      = aws_key_pair.consul_key.key_name
  associate_public_ip_address   = true
  subnet_id                     = aws_subnet.consul-demo.id
  vpc_security_group_ids        = [aws_security_group.consul-demo.id]
  iam_instance_profile          = aws_iam_instance_profile.instance_profile.name
  user_data = templatefile("files/server_template.tpl", {
    server_name_tag = "${var.owner}-consul-server-instance"})
  tags = {
    Name     = "${var.owner}-consul-server-instance"
    Owner    = var.owner
    Instance = "${var.owner}-consul-server-instance-${count.index}"
  }
}
```

在下一个代码片段中，我们正在构建 Consul 客户端——准确地说，是四个，分别代表地、风、火、水四大核心元素。其中许多参数与服务器使用的参数相同，但请注意，这里没有 `user_data` 部分。为了多样化，我们使用 Terraform provisioner 来配置客户端，尽管在生产环境中，像 Ansible 这样的配置管理器会更受欢迎：

```
resource aws_instance "consul-client" {
  count = 4
  ami = data.aws_ami.an_image.id
  instance_type = var.instance_type
  key_name = aws_key_pair.consul_key.key_name
  associate_public_ip_address = true
  subnet_id = aws_subnet.consul-demo.id
  vpc_security_group_ids = [aws_security_group.consul-demo.id]
  iam_instance_profile = aws_iam_instance_profile.instance_profile.name
  tags = {
    Name = "${var.owner}-consul-client-instance-${count.index}"
    Owner = var.owner
  }
}
```

需要注意的是，远程 provisioner 在 Terraform 领域中并不是首选；不过，我偶尔发现它在定制所需资源时非常有用。作为此配置过程的一部分，我们遍历四个客户端中的每一个并执行一些功能，如下所示：

- 将名为 `httpd.json` 的服务定义文件传输到客户端。稍后当我们开始使用集群时，会进一步讨论这一点。
- 创建一个 `consul-client.hcl` 配置文件。我想更深入地探讨一下这个文件：
  - 我们宣告客户端的公网 IP 地址。这告诉 Consul 服务器以及任何请求访问此客户端上服务的设备，可以通过哪个地址访问它。我们选择客户端的公网地址，以便内部网络之外的应用程序可以访问它。
- 我们告诉 agent 不要以服务器模式运行，因为——嗯——我们是客户端。
- 我们启用脚本检查。这允许使用自定义脚本来检查服务健康状态，默认情况下是不启用的。
- 我们告诉 Consul 将其服务绑定到机器的私有地址。请注意绑定地址和宣告地址之间的区别。私有地址是机器自身知道的，并标识外部接口。而我们宣告的公网地址甚至不是客户端机器本身所知道的，它由 AWS 管理。
- 我们告诉客户端服务器在哪里。请记住，客户端需要向服务器注册，以便传达自身及其所有受管服务的状态。
- 我们指定与 Consul 服务器通信时使用的客户端地址。由于服务器和客户端位于同一个 AWS 虚拟网络中，我们可以使用私有 IP 地址。
- 将客户端配置和服务定义文件移动到配置目录（`/etc/consul/consul.d/`）。
- 使用 Python 启动一个简单的超文本传输协议（HTTP）服务器。毕竟，创建一个 Consul 集群却不在上面运行任何服务，何必这么麻烦呢？！
- 接下来，我们使用一个 provisioner 在客户端机器上远程启动 Consul 服务。
- 最后一块告诉 Terraform 它将如何连接到客户端机器。请注意，我们使用的是之前创建的 SSH 密钥的内容。

请注意，虽然客户端的配置全部都在这个 Terraform 文件中，但服务器的应用程序配置却不在这里。这是因为我们在 Terraform 中使用了 `user_data` 功能，而且我们很快就会看到模板文件。在 Terraform 中，当我们使用 provisioner 来执行脚本或移动文件时，我们会声明一个 `null_resource` 属性，如下所示：

```
resource null_resource "provisioning-clients" {
  for_each = { for client in aws_instance.consul-client : client.tags.Name => client }
  provisioner "file" {
    source      = "files/httpd.json"
    destination = "/tmp/httpd.json"
  }
}
```

```
```hcl
provisioner "remote-exec" {
  inline = [
    "sudo cat << EOF > /tmp/consul-client.hcl",
    "advertise_addr = \"${each.value.public_ip}\"",
    "server = false",
    "enable_script_checks = true",
    "bind_addr = \"${each.value.private_ip}\"",
    "retry_join = [\"${aws_instance.consul-server[0].public_ip}\",\"${aws_instance.consul-server[1].public_ip}\",\"${aws_instance.consul-server[2].public_ip}\"]",
    "client_addr = \"${each.value.private_ip}\"",
    "EOF",
    "sudo mv /tmp/consul-client.hcl /etc/consul/consul.d/consul-client.hcl",
    "sudo mv /tmp/httpd.json /etc/consul/consul.d/httpd.json",
    "nohup python3 -m http.server 8080 &",
    "sleep 60"
  ]
}
provisioner "remote-exec" {
  inline = [
    "sudo systemctl start consul",
  ]
}
connection {
  type        = "ssh"
  user        = "ubuntu"
  private_key = local_file.private_key.content
  host        = each.value.public_ip
}
```
```

接下来，我们有实例身份信息和相关角色。这与 Consul 没有直接关联，但它是启用我们在 AWS 内所需通信路径所必需的。代码如下所示：

```
```hcl
resource "aws_iam_instance_profile" "instance_profile" {
  name_prefix = var.owner
  role        = aws_iam_role.instance_role.name
}

resource "aws_iam_role" "instance_role" {
  name_prefix        = var.owner
  assume_role_policy = data.aws_iam_policy_document.instance_role.json
}

data "aws_iam_policy_document" "instance_role" {
  statement {
    effect  = "Allow"
    actions = ["sts:AssumeRole"]

    principals {
      type        = "Service"
      identifiers = ["ec2.amazonaws.com"]
    }
  }
}

resource "aws_iam_role_policy" "metadata_access" {
  name   = "metadata_access"
  role   = aws_iam_role.instance_role.id
  policy = data.aws_iam_policy_document.metadata_access.json
}

data "aws_iam_policy_document" "metadata_access" {
  statement {
    effect = "Allow"
    actions = [
      "ec2:DescribeInstances",
    ]
    resources = ["*"]
  }
}
```
```

好了——现在我们已经看完了整个 Terraform 文件，让我们仔细看看那个服务器模板文件。通过该模板，我们创建了一个 `consul-server.hcl` 服务器配置文件，并用所有必要的配置填充它，如下所示：

```
```bash
#!/usr/bin/env bash

cat << EOF > /etc/consul/consul.d/consul-server.hcl
server = true
bootstrap_expect = 3
retry_join = ["provider=aws tag_key=Name tag_value=${server_name_tag}"]
bind_addr = "$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)"
advertise_addr = "$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)"
client_addr = "$(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)"
ui = true
EOF

# Starting consul services
sudo systemctl start consul
```
```

顺着这个模板文件，让我们看看这个文件到底在做什么：

- 1. 指示 Consul agent 以服务器模式运行。
- 2. 接下来，我们指定希望在集群中看到的预期服务器数量。请记住，我们之前在讨论共识和法定人数时提到过这一点。我们创建了三个 Consul 服务器，这意味着我们的法定人数是……（我给你一个提示——它大于一，小于三）。
- 3. 好的——这部分相当酷。如果你还记得，在 Terraform 脚本中，我们有一个 `instance` 标签，用于标识各个服务器。这个 `retry_join` 配置标识了一个服务器列表，这些服务器组成了预期的三个服务器对等集。AWS 会用与该 `instance` 标签匹配的服务器来填充这个值。





+   4. 与客户端配置类似，我们正在配置要绑定的地址（`bind_addr`）、要通告的地址（`advertise_addr`）以及我们希望被识别为的地址（`client_addr`）。我们使用的是在配置过程中设置的主机元数据来配置这些值——这是 Terraform 和 AWS 的另一个不错的功能。
+   5. 最后，我们启用了 Consul 的 Web UI。默认情况下 Web 界面不可用，但我们在全书中偶尔会使用它。

至此，配置文件的内容就完成了。我们需要做的最后一件事是启动我们的 Consul 服务器！

Wahoo！恭喜你完成了所有这些疯狂的内容！我希望你阅读时的乐趣不亚于我写作时的乐趣。如果你感觉好一点，*Planet Caravan* 提供了一个很棒的小节拍来写作。

好了——到现在为止，你的 Consul 集群应该已经启动并运行了。嘿——*up and running*：这会是一个很棒的书系名称！那么，现在我们已经把它启动起来，让我们开始四处看看。

## 访问 Consul 系统

Terraform 运行的输出将为你提供访问 Consul 集群所需的关键信息。这是在 Terraform 运行中作为 *output* 产生的。另一个你会在目录中找到的输出是一个 SSH 密钥，可用于在需要时远程连接到机器，这里很快就会用到。请看下面的代码片段：

```
Apply complete! Resources: 25 added, 0 changed, 0 destroyed.

Outputs:

CONSUL_HTTP_ADDR = "http://18.191.150.155:8500"
Consul_Server_IPs = [
"3.129.42.175",
"18.191.150.155",
"3.139.81.1",
]
```

使用前面的示例，我们将通过两种不同的方式访问 Consul 系统：通过 CLI 和通过 Web 界面。

## 使用 CLI

让我们从 SSH 开始。Terraform 运行产生的工件之一是用于使用 Consul 机器进行身份验证的 SSH PEM 密钥。该文件将命名为 `<owner>-consul-key.pem`。使用命令行 shell，从你运行 Terraform 的同一位置，你可以 SSH 到任何 Consul 服务器——例如这个：

```
```bash
$ ssh -i rjackson-consul-key.pem ubuntu@18.191.150.155
```
```

显然，你需要将密钥名称替换为你自己的密钥，并将 IP 地址替换为其中一台 Consul 服务器的 IP 地址。它可以是三个中的任何一个，所以不用担心选对 *right* 一个。你会收到一个提示，里面有各种乱码（技术术语）关于 **Secure Hash Algorithm 256 (SHA256)** 指纹，并询问是否继续。是的——你确实想继续，然后你就会在你的第一台 Consul 服务器上了！

> **Note**
> 
> 如果你不想 SSH 到服务器，你不需要！但是，为了通过 CLI 与 Consul 集群通信，你需要在本地安装 Consul。别担心——它和下载 Packer 和 Terraform 一样简单；下载一个 ZIP 文件，解压，然后放到你的路径中。Consul 可以从 https://www.consul.io/downloads 下载。如果你决定使用远程 CLI，只需跟随操作即可，因为步骤是相同的。

现在你已经在机器上了，请不要删除任何东西！不要笑——这种情况会发生。如果你记得我们在讨论模板文件时，有一个 `bind_address` 属性指定了 Consul 将监听的地址。我们使用的是公共地址，所以我们希望将其设置为环境中的 Consul 地址。别担心——你有所有必要的信息来 *export* 这些信息到你的环境中。我不知道为什么他们在把信息放进去时称之为 *export*，但就这样吧。

在 Terraform 输出中，你会看到一个 `CONSUL_HTTP_ADDR` 的值。这正是你要设置到环境中的变量以及该地址。再次以 Terraform 输出为例，你可以看到这里：

```
```bash
$ export CONSUL_HTTP_ADDR=http://18.191.150.155:8500
```
```

好了——你已经准备就绪，让我们来看看那个集群！我们首先要做的是确保我们的所有机器、服务器和客户端都已启动并在 Consul 系统中注册。如果你不记得数量，我们有三台服务器和四台客户端。你可以通过以下命令查看我们集群的所有 *members*：

```
```bash
$ consul members
```
```

Figure 2.19 – Consul web interface

随意浏览这里。你已经可以看到可用服务的列表以及这些服务运行的位置。有趣的是，我们可以看到这里 consul 在三个实例上运行，而 httpd 在四个实例上运行。点击 consul 行，你可以获取运行该服务的节点信息，如以下屏幕截图所示。这些是 Consul 服务器节点：

Figure 2.20 – Consul service instances

如果我们点击窗口顶部 **Nodes** 选项卡，这将给出与我们运行 `consul members` 命令时类似的输出，如以下屏幕截图所示：

![](img/52479dafaefba00517c48a137e037b7e_72_0.png)

Figure 2.21 – Consul nodes listing

就这样：你已经创建了一个完整的 Consul 集群，由三台服务器和四台客户端组成。恭喜！
