## Keep It Safe, Stupid, and Secure Your Cluster!

此时，你可能会对自己说，*哇，两章就完成了。* 好吧，很抱歉打破你的幻想，但你目前非常暴露。是的，我们构建了一个基本的 Consul 集群，由三台服务器和四台客户端组成，但该系统没有任何安全措施。基本上，我们建造了一座没有门的玻璃房子，任何人都可以走进去，任何人都可以看到里面发生了什么。有点 creepy，不是吗？

为了了解我们将如何保护我们的系统，更重要的是保护我们的服务，让我们看看我们暴露的所有区域：

- **System communication** – 我们已经看到所有 Consul 组件如何自由地相互通信。任何人都可以未经验证地加入该集群，任何人都可以窃听。
- **Who can do what** – 即使你在集群中经过验证和身份验证，我们也需要确保可用操作的范围受到严格控制。毕竟，我们不希望任何人带着对配置的完全访问权限跳入集群。
- Extending Consul – 在理解到多个组将访问 Consul 的前提下，我们希望为每个组提供他们管理自己的服务所需的访问权限，而不会干扰他人。

除了这些区域之外，还有保护操作系统环境的基本程序。这包括对操作系统和文件结构的受保护访问。这些被认为是基本的管理程序，不在本書中涵盖。

有了这些，让我们直接跳进去，开始看看如何开始保护我们的玻璃房子！

## Technical requirements

随着通过保护任何基础设施而引入的额外复杂性，我们将使用一个特定于本章的新存储库：

```
https://github.com/PacktPublishing/Simplifying-Service-Management-with-Consul/tree/main/ch3
```

所有文件和代码都可用，我们将再次使用 Packer 和 Terraform 来创建我们的工作基础设施，这意味着我们也需要我们在第 2 章 Architecture – How Does It Work? 中建立的 AWS 凭证。

本章中的练习是在 macOS Terminal 中使用原生 SSH 客户端执行的。熟悉基于命令行的文件编辑器对于在必要时查看和编辑文件也总是很有帮助的。

最后，让我们重申第 2 章 Architecture – How Does It Work? 中所述的内容：

- 当你一天的工作完成后，务必执行 terraform destroy。它意味着你继续第二天的工作时必须重建，但这对不可变基础设施来说是一个很好的教训！
- 对你的 AWS 凭证要非常小心。永远不要分享你的访问密钥 ID，更绝对不要分享你的秘密访问密钥。我们将把它们设置为环境变量，这样它们永远不会出现在代码中，也绝对不会出现在你的版本控制系统中！





## 保护通信路径

不知道你怎么样，但每当我开始阅读有关 AES、TLS、GCM 等文档时，我的眼睛就开始发花。大量的缩写词让我们像追松鼠一样到处乱跑，但这并不能帮助我们保护 Consul 集群的安全。所以，我的目标是用最少的缩写词来解释我们需要做些什么来保护我们的系统通信。这让我想起高中时我不得不写一篇论文，而且不能用任何形式的动词“to be”。谢谢您，法利先生。重要的是要记住，目前我们只关注保护我们的客户端和服务器。保护我们服务的流量并对其进行加密稍后再说，所以请耐心等待。

在本书的第二章中，我们了解了 Consul 组件之间两种主要的通信模式：**远程过程调用（RPC）** 和八卦协议。但是等等——我们读到的 Raft 和共识机制是怎么回事呢？嗯，所有这些通信都利用 RPC 通信，并且通过 TCP 传输。八卦协议是代理之间用来相互通信的，如果你还记得的话，它利用的是 UDP。我之所以提到这些，是因为将有两种不同的方法来保护这些通信路径。让我们先谈谈保护八卦协议，仅仅因为那更容易理解一些。

### 保护我们的八卦协议

如果你见过一群人八卦，你就知道他们通常会紧密地聚在一起，以便没有其他人能加入并偷听他们的谈话。当你试图靠近这群人时，突然间，他们的谈话声变安静了，一种不信任感立刻产生。这是因为你可能不是他们认识的实体，或者他们可能不清楚你的立场。如果那些人因为社交距离而在更大的区域里分散开来，那就更容易偷听了。这就是我们在 Consul 中工作的环境。尤其是在云环境中，要让八卦群体紧密地聚在一起并远离外人和窃听者就比较困难了。

既然我们明白了八卦者不可能作为一个孤立的群体存在，那么确保他们谈论的内容不被其他人听到，或者至少不被理解的最佳方法是什么呢？让我们给我们的故事加个转折（不是给饮料里加片柠檬）。假设你发现自己身处一群说着你不熟悉语言的人群中。我想我们大多数人都经历过这种情况——即使他们说同一种核心语言，你可能也不明白他们说的某些词。我的妻子在印第安纳州长大，我们搬到波士顿地区时就经历过这种情况，但那是另一个故事了。在这种环境下，你可能会听到人们在说话，但你无法理解他们在说什么。这是一种保持通信开放，但对那些无权接收信息的人保密的好方法。这正是我们在保护我们的八卦代理时所做的。

Consul 提供了一种生成特殊语言的方法，该语言由一个*加密密钥*定义。这个信息与我们创建的相同，但使用的语言不同。这个密钥被分发到 Consul 集群中的所有代理。即使你有多个集群相互通信，它们也都需要使用相同的语言，因此使用相同的密钥。

![](img/52479dafaefba00517c48a137e037b7e_78_0.png)

图 3.1 – 谁懂苏美尔语？

此时，你可能会想*但是 Rob，最终人们可以学会一种新语言并破译消息*。你说得对，虽然我们中有些人可能比其他人花更多时间学习不同的语言，但最终，只要我们足够努力，我们就能弄明白。这对于每一个加密密钥、密码和证书都是一样的，这也是为什么定期更换和更新这些安全项目很重要的主要原因。本质上，当八卦者周围的人学会了新语言，我们就要更换语言！当我们开始谈论群体中有几十、几百或几千个八卦者时，要让每个人都学会新语言、同时又不遗漏消息，这可能非常具有挑战性。幸运的是，Consul 可以为你管理很多这方面的事情，当我们部署新的安全系统时，我们将会看到如何做到这一点。

### 保护我们的通话

与八卦协议不同，当我们给人打电话时（当然，除非是电话会议），通信不会广播给分散的群体。那个对话通常是点对点的，并有一定程度的身份验证，以确认你在和谁说话。这可以是电话号码、来源号码、熟悉的声音等等。当我们研究 Consul 代理之间的通信时，这个过程非常相似。然而，我们永远不知道是否有人在窃听那个对话。因此，保护我们的通话需要三个步骤：验证谁在呼叫、验证谁被呼叫，以及加密他们之间的消息。

![](img/52479dafaefba00517c48a137e037b7e_79_0.png)

图 3.2 – 安全与不安全的通信

我们使用 Consul 实现这一点的方式，与你几乎每天打开网页浏览器、登录银行账户并给我汇钱时的做法并没有太大不同。当你这样做时，你的网页浏览器会验证它是否已连接到银行的正确网站。这种验证来自于一个由某个官方权威实体签名的证书，确保你的浏览器相信该网站是有效的。如果你在浏览器中看到过弹出窗口，提示无法验证证书，那意味着该证书不是由已知且被接受的权威机构签名的。当该证书被交换时，浏览器就知道了如何加密流量，以便银行网站能够解密，同样，你的浏览器也知道如何解密从银行传回的消息。然而，我们使用 Consul 的做法有一点小小的不同。

让我们再次假设你正在从浏览器访问银行网站的场景。银行网站并不真正关心你在哪里（至少在大多数情况下是这样）。对于 Consul，发起呼叫的代理不仅验证它在呼叫一个有效的实体，被呼叫的一方也会确保呼叫者是有效的。让我们深入探讨一下这个问题，但别担心——我们深入探讨时不会碰到水。

我们将在本节中大量讨论密钥和证书。如果你从未深入研究过传输层安全（TLS），这可能会有点令人困惑，所以我将使用一些符号来标识服务器、客户端以及公钥和私钥，这些符号基于长寿动画系列片《辛普森一家》中的角色。例如，我们将有三台服务器：Bart、Lisa 和 Maggie。对于这三台服务器，需要某种受信任的权威机构来标识 Bart、Lisa 和 Maggie；我们将称我们的权威机构为 Marge。

我们的三台服务器都需要某种方法来传输和接收只有它们自己能理解的消息。如果你还记得我们的电话类比，它们都需要验证谁在呼叫、谁被呼叫，并加密它们之间的消息。所有这些都通过专门为它们制作的证书来完成。现在，如果 Bart 创建了自己的证书，并将这些证书分发给 Lisa 和 Maggie，它们就可以共享秘密消息。然而，什么能阻止 Homer——那个本不应该接触这些消息的家伙——加入这个群体，分享他自己的证书，并查看这些秘密消息呢？为此，我们需要 Marge，某种中央权威机构，以确保每个人都使用她授权的证书。我们可以称她为证书颁发机构（CA）。

Marge 利用她的强大力量，为 Bart 制作了两份证书，为 Lisa 制作了两份，为 Maggie 制作了两份。这些证书有特殊的名称：公钥和私钥。公钥确认了他们的身份，并使他们能够向其他人加密消息，而私钥使他们能够解密他们收到的消息。可选地，Marge 可以创建特定的限制，例如证书的持续时间，以及允许哪些姓氏使用该证书。例如，如果有个人走进来，其姓氏不是 Simpson，那么他们将无法使用 Marge 的证书。

Marge 使用她自己的两份证书来制作这些证书；一份标识她的权威机构，我们称之为 chop-chop，另一个秘密成分我们称之为 salt。没有其他人知道 Marge 的私密秘密，因为如果他们知道了，他们就可以在自己的证书中使用 salt，从而破坏 Marge 的权威性。总而言之，让我们看看我们拥有的证书：

- porkchop：标识 Marge 具有权威性的证书
- salt：Marge 用来制作证书的秘密成分
- bart.public_key：Bart 用来标识自己并向 Lisa 和 Maggie 加密秘密消息的证书
- bart.private_key：只有 Bart 拥有，并且可以用来解密来自 Lisa 和 Maggie 的消息的证书





- `lisa.public_key`: Lisa 用于识别自己并向 Bart 和 Maggie 加密秘密消息的证书
- `lisa.private_key`: 只有 Lisa 拥有的证书，她可以用它来解密来自 Bart 和 Maggie 的消息
- `maggie.public_key`: Maggie 用于识别自己并向 Bart 和 Lisa 加密秘密消息的证书
- `maggie.private_key`: 只有 Maggie 拥有的证书，她可以用它来解密来自 Bart 和 Lisa 的消息

如果 Lisa 想给 Maggie 发送消息，她会使用自己的特殊公钥对消息进行加密。Lisa 将该消息发送给 Maggie，Maggie 可以用自己的私钥打开消息，并使用 Marge 提供的猪排来验证该消息。现在，假设 Homer 可以创建自己的公钥并向 Maggie 加密自己的消息。好吧，该密钥没有使用 Marge 的授权，也没有她的秘密配料。因此，Maggie 知道要拒绝该消息，而伤心的 Homer 则前往 Moe's。

![](img/52479dafaefba00517c48a137e037b7e_81_0.png)

图 3.3 – 证书和密钥分发

这正是我们保护 Consul 机器之间调用的方式。集群中的每个节点都有自己的公钥、私钥，以及由某个集中式 CA 提供的权威密钥。对于我们的练习，我们将使用 Consul 作为我们的 CA。这只是为了帮助我们尽可能独立，但这不应该在生产环境中这样做。在现实世界中，你的公司将拥有一个 CA，安全团队中的某个人会将这些密钥提供给你（以及在证书轮换时提供新密钥）。如果你的公司已经拥抱了自动化，那么在获得授权的情况下，可以通过利用 HashiCorp 的 Vault 来生成这些密钥。

现在我们已经了解了如何保护节点之间的 gossip 以及它们的直接消息，让我们看看还能做些什么来保护我们的系统。

## 使用访问控制进行访问控制

Consul 功能的核心是一个分布式键/值存储。通过 gossip 协议，任何一个节点学到的东西都可以与集群中的其他节点共享。这不仅适用于我们三个主要用例，而且通常，操作员会利用 Consul 来分发相关组件可以利用的配置参数。如果您想快速更改所有机器上的日志级别，这将非常有帮助。这也是为什么在任何生产部署中，保护 Consul 的重要性怎么强调都不为过。利用我们上一节讨论的 TLS 密钥，我们可以验证和保护客户端与服务器之间的通信。如果我们不这样做，那么就有可能有人假装成为集群的一部分。

Consul 中的访问控制系统提供了关于每个节点在网络上可以做什么的额外控制级别——因此得名 *访问控制*。如果你曾经处理过路由设备上的 **访问控制列表 (ACLs)** 或网络防火墙规则，你可能已经熟悉访问控制的概念。然而，对于 Consul，规则可以非常精细和详细。当然，这意味着它们可能会变得非常复杂，从而增加了对某种程度的基础设施即代码（如 Terraform）或其他自动化工具的需求。在深入了解之前，让我们先了解一下 Consul 访问控制系统包含的内容。

当我们在 Consul 的领域讨论访问控制时，有三个主要部分需要我们理解：规则、策略和令牌。可选地，我们还可以为策略组创建角色，针对应应用策略的节点、服务或数据中心组。我们在本章中大量谈论确保身份识别。毕竟，本章是关于安全的，而安全的一个重要部分是确保身份。这就是为什么秘密握手和暗号被发明的原因。对于 Consul 访问控制，该身份以令牌的形式提供，该令牌由某个权威实体生成。我们将使用 Consul 作为该权威；然而，在 HashiCorp 的 Vault 等系统中集中管理这些令牌会更受欢迎，不仅仅是因为我目前受雇于 HashiCorp，而是主要因为利用 Vault 为操作员提供了一种集中且安全的方法来维护和轮换这些令牌。好了，关于令牌就说到这里……令牌能为我做什么？

当某人使用令牌访问 Consul 时，令牌会与策略关联，该策略包含一个或多个规则。由于策略只是规则的集合，我们将重点关注可以应用的规则。

![](img/52479dafaefba00517c48a137e037b7e_83_0.png)

图 3.4 – 令牌、策略和规则

几乎每条规则都包含三个部分：规则类型、定义规则应用位置的变量，以及规则的控制级别。有些特殊情况变量不是必需的，但我们稍后会讨论。还有一个始终存在的默认策略，其为 *deny* 或 *allow*，取决于你的安全态势。现在我提到了 *posture*，我坐直了……你呢？在定义默认策略时必须格外小心。如果默认是 *deny*，那么每个操作、服务、节点等都需要明确的权限才能执行某些功能。如果默认策略是 *allow*，那么你需要指定要阻止的内容。当然，后者看起来实现起来要简单得多。然而，从安全角度来看，只识别你想要允许的少数事情要容易得多，而不是试图识别和阻止所有需要阻止的事情（这些事情 constantly 变化）。因此，继续前进，我们将假设我们的默认策略是拒绝。你现在可能恨我，但你以后会感激的。

你可以猜出 Consul 规则的控制级别，但以防你猜不到，它们如下：

- **read**: 对特定资源的读取访问授予使用该令牌的人读取与资源关联的值的能力。
- **write**: 对特定资源的写入访问授予使用该令牌的人更新或创建与资源关联的值的能力。如果你的令牌授予对资源的写入访问，你也拥有读取访问。
- **deny**: Smackdown – 你没有权限读取或写入与所标识资源关联的值。由于我们将使用默认拒绝策略，因此我们不需要此级别。
- **list**: 这是一个特殊级别，授予使用该令牌的人列出所有相关参数的能力。唯一适用的地方是在 Consul 键/值存储中，相关令牌的持有者可以列出特定路径前缀下的所有键/值组合。

正如我在数学课上残酷学到的那样，每当你查看此类区域时，优先级顺序至关重要。如果有任何重叠规则，deny 优先于所有规则，写入能力优先于只读。这应该是理所当然的，但 deny 优先于一切这一事实应该被充分理解。此外，如果你在特定规则中具有 *write* 访问权限，你会自动获得 list 访问权限。如果你具有 *list* 访问权限，你会自动具有 *read* 访问权限，因此控制级别是某种程度上嵌套的。我发现通过以下图表很容易记住这一点：

![](img/52479dafaefba00517c48a137e037b7e_85_0.png)

现在我们知道了 *什么* 可以用单个规则做，让我们看看 *在哪里* 可以（或不可以）执行这些操作……这就引出了规则类型。以下是当前支持的所有规则类型的列表。虽然我们在本次练习中只会使用其中几个，但我只是为了完整性而列出它们全部：

- **ACL resource:** 控制 ACL 规则的管理。
- **Agent:** 此规则类型控制与管理 Consul 代理相关的方面，例如加入集群、列出成员和代理配置。代理确实应该对其自己的节点具有写入访问权限；否则，它们将无法将其自己的服务信息注册到目录中。





- **事件（Event）**：这专门与读取和写入 Consul 事件的能力相关。本书不会涵盖事件相关的内容，但可以利用事件根据 Consul 系统中的变化（包括服务可用性）来执行自定义操作。
- **键/值（Key/Value）**：控制对 Consul 内分布式键/值存储的访问。规则可应用于读取、写入或列出键及其关联值的权限。
- **哨兵（Sentinel）**：与犹大牧师乐队那首绝妙的歌曲无关，Sentinel 规则允许我们根据 Consul 结构编写几乎任何可能的规则。Sentinel 是企业版功能，且相当高级，因此本书不会涉及。
- **密钥环（Keyring）**：这是一种无需应用变量的规则类型，因为它特定于密钥环功能。由于密钥环控制着 gossip 协议的加密密钥，因此对此区域的访问应严格管控。
- **节点（Node）**：这是一个重要的规则类型。它控制节点级数据，例如目录中的可用服务和节点健康状态。它还可以作为过滤器，将代理级操作限制在允许的集群成员子集内。
- **操作员（Operator）**：这是另一种无需应用变量的规则类型，用于处理 Consul 中的操作员级操作，例如 auto-pilot 或 Raft 特定功能。与密钥环一样，对此区域的访问应严格管控。
- **预置查询（Prepared query）**：Consul 提供了创建模板的能力，这些模板对于系统中更复杂的服务查询非常有用，包括节点位置、元数据、标签等。本书不会涵盖预置查询。
- **服务（Service）**：这是另一个需要理解的重要规则类型。该规则类型管理服务级信息，包括对服务目录和服务健康状态的访问。
- **会话（Session）**：Consul 允许你在键/值存储中分配和释放值的锁。这可以在已执行的会话中完成，本书不会涉及这部分内容。
- **命名空间（Namespace）**：命名空间是另一个企业级功能，支持对 Consul 集群进行细分。此规则类型允许你为整个命名空间创建由多种规则类型组成的更全面的规则。虽然本书不会过多涉及命名空间，但非常重要的是要理解，包括操作员和密钥环规则在内的几种规则类型不能应用于命名空间。这些功能是全局性的，只在默认命名空间上运行。

正如你所看到的，有很多领域可以应用 ACL 来管理人类、机器或半机械人访问我们服务的权限。这也为控制各个节点提供了大量途径。

当我们谈论安全性时，我们经常使用术语*最小权限*。简单地说（或写），这意味着不是给一个孩子一堆糖果并告诉他们什么不能吃，而是从孩子没有糖果开始，只提供他们能吃的东西。这就是为什么在本节前面，我们讨论了在 Consul 中实施默认拒绝策略。如果我们将其作为标准安全规则，尤其是结合我们在上一节中讨论的证书，我们就可以进一步加强对各个节点的控制。这大大减少了任何懦夫或窃听者访问或监听我们集群操作的可能性——因为如果你没有秘密握手暗号，你就不会通过。

既然我们已经解决了所有这些问题，让我们看看规则长什么样。我们将在这里查看两条规则，当我们开始实际操作系统时，这两条规则都会用到。

第一条规则为 Consul 客户端应用了一个最小策略，使其能够在自身执行操作，并将基础设施的任何变化通知 Consul 服务器：

```hcl
node "ip-192-168-100-11" {
  policy = "write"
}
service "consul" {
  policy = "write"
}
agent_prefix "ip" {
  policy = "write"
}
```

在此策略中，我们有三个主要规则，看起来非常简单，但在最小权限模型中，它们的作用至关重要。

策略中的第一条规则使用了节点（node）规则类型。如果没有这条规则，节点就无法向服务器集群更新它看到的服务，更重要的是，无法更新节点的健康状况和位置。如果客户端尝试加入集群，但没有与此规则关联的令牌，服务器集群甚至不会将其识别为客户端。请注意，由于节点规则仅适用于 ip-192-168-100-11，因此该规则仅适用于此特定节点的操作，因为那是它的主机名。任何其他使用与此策略关联的令牌的节点都不会被 Consul 服务器集群识别。

下一条规则属于 `service` 类型，允许我们更新节点本身上的服务目录。这特定于节点本身上的服务，而不是由 Consul 服务器集群托管的整体服务目录。要更新集中式服务目录，我们需要 `agent` 规则，我们接下来会讨论。请注意，使用此规则，我们可以 `write` 到名为 `consul` 的服务来管理服务更改。如果我们只想读取服务的状态或可用性，我们可以使用 `read` 指令。

最后一条规则使用了 `agent` 规则类型。这与节点规则类型的重要性类似，它控制着节点功能的非常基本的方面（尽管在这种情况下，它与代理相关）。此规则允许关联令牌的持有者执行代理级功能，例如更新服务目录……这有点重要，因为 Consul 的一切都围绕着服务。你可能已经注意到这个特定规则定义的一个不同之处：它不是指向某个特定的代理，而是使用了一个微小的变体，称为 `agent_prefix`。正如你可能已经猜到的，这适用于任何主机名以 `ip` 前缀开头的代理。这提供了一些灵活性，允许你将规则应用于代理组，而不是为每个单独的代理编写规则。然而，这确实让我们暴露了一些，因为该规则并未锁定到某一个特定的代理。可以帮助解决这个问题的一种方法是拥有两条代理规则——一条特定于其所在的主机，使其能够写入代理操作，另一条用于从其他节点读取代理操作：

```hcl
node "ip-192-168-100-11" {
    policy = "write"
}
agent "ip-192-168-100-11" {
    policy = "write"
}
agent_prefix "ip" {
    policy = "read"
}
```

这是适用于该特定代理的一组非常有限的策略规则。以下策略稍微开放了一些，更适用于服务器集群，使其能够执行诸如写入 ACL、事件等操作：

```hcl
acl = "write"
agent_prefix "" {
    policy = "write"
}
event_prefix "" {
```

在这里，与此策略关联的令牌的持有者将能够写入任何以 `ip-192` 开头的主机名，但只能对任何以 `ip` 开头的主机名进行只读访问。

我希望你能看到，Consul 中 ACL 的灵活性允许我们在系统上定义和执行极其具体的规则。我们可以对 Consul 功能的具体领域应用各种控制。其中一些规则对于任何功能正常的系统都至关重要（例如节点或代理类型），但即使是这些规则也应该受到控制，以便它们只应用于特定的分段（主机名、域等）。接下来，我们将看看如何管理那些可以访问我们 Consul 集群的人类，并跟踪他们在做什么，就像老大哥一样。

正如我们已经看到的，使用 Terraform 来创建和管理基础设施简化了很多这种复杂性。当你开始处理包含数百、数千和数万个节点的 Consul 集群时（是的，这种规模的集群确实存在），将基础设施作为代码使用的便利性就变得更加必要了。如果你已经厌倦了阅读，想看看所有这些安全功能是如何结合在一起的，请稍安勿躁——我们很快就会讲到。首先，我们需要快速了解一下 Consul 系统中的审计日志记录，这是任何企业都至关重要的组成部分。

## 打造你的身份

作为服务生产启用的一部分，我们需要处理谁将访问系统以及他们能做什么。这通常被称为**基于角色的访问控制**，或 **RBAC**。我们还希望能够维护系统的审计跟踪。随着我们走出那个受信任的有围墙花园网络进入云环境，最后这一点变得越来越重要。我无法告诉你，即使在受控环境中，我有多少次不得不回去弄清楚是什么改变以及何时改变破坏了我的系统！为充分披露起见，要实现这一级别的功能，需要 Consul 的企业版许可证。

实际登录 Consul 系统的过程当然可以通过使用 ACL 进行管理，然后像对待机器一样对待任何人类，给他们自己特定的访问令牌。然而，这些令牌并不像冉·阿让的 24601 那么简单。所以，不可避免地会有人把他们的令牌写在便利贴上贴在笔记本电脑上，或者更甚者，把它和最新的猫咪表情包一起通过电子邮件发送给朋友和家人。此外，这只有在某个工程师（会是你吗？）站在公司面前高呼*我不是一个数字，我是一个自由的人！*之前才有效。这时，高层管理办公室里传来癫狂的笑声。一个更好的解决方案是将 Consul 连接到某种集中的身份验证和授权方法。为此，Consul Enterprise 利用了一种日益增长的集中式身份验证和授权标准——**OpenID Connect (OIDC)**。





利用 OIDC，Consul 可以通过查询众多 OIDC 提供商之一来验证尝试访问 Consul 的用户（大多数企业环境至少使用一个）。如果你曾经登录某个服务并被重定向到另一个用于 Google、Yahoo、Microsoft、PayPal 或 Amazon 凭证的身份验证页面或弹出窗口，那么很可能你已为该身份验证使用了 OIDC。企业环境中其他流行的 OIDC 提供商包括 OKTA 和 Auth0。此过程不仅验证并对用户进行身份验证，还可以将该用户映射到一组授权策略以控制对系统的访问。毕竟，如果你曾经需要给经理提供 Consul 集群的访问权限，你肯定不希望他们拥有管理权限！那么，让我们用另一个美妙的例子来逐步了解。

作为这个故事的一部分，我们需要假设我们的更大雇主已实施 OIDC，并且我们每个人都有自己的凭证来使用该 OIDC 提供商进行身份验证。除了我们的凭证外，我们所有人都有与我们在公司中的职位相关联的特定元数据：

| Credentials | Metadata |
| Username | Password | First Name | Last Name | Position |
| sharris | ********** | Steve | Harris | Bassist |
| bdickinson | ********** | Bruce | Dickinson | Singer |
| dmurray | ********** | Dave | Murray | Guitarist |
| asmith | ********** | Adrian | Smith | Guitarist |
| nmcbrain | ********** | Nicko | McBrain | Drummer |

Figure 3.6 – OIDC accounts and metadata

在这个示例中，每个人都有自己独特的职位，除了 Adrian 和 Dave。他们都属于吉他手组，我们将把它映射到 Consul 中的一个角色。回想一下，角色由 Consul 中的一组 ACL 策略组成。当我们配置 Consul 来利用此信息时，我们创建了所谓的声明映射。这将 OIDC 提供商中配置的元数据映射到 Consul 中的变量中，然后可用于将该元数据与特定角色关联。例如，以下配置将名字和姓氏的特定映射关联起来，并且职位元数据与 Consul 中名为 band_positions 的列表相关联：

```
{
"OIDCDiscoveryURL": "https://<OIDC_URL>/",
"OIDCClientID": "<OIDC_CLIENT_ID>",
```

```
"OIDCClientSecret": "<OIDC_CLIENT_SECRET>",
"BoundAudiences": ["<OIDC_CLIENT_ID>"],
"AllowedRedirectURIs": [
    "http://localhost:8550/oidc/callback",
    "http://localhost:8500/ui/oidc/callback"
],
"ClaimMappings": {
    "user.metadata.first_name": "first_name",
    "user.metadata.last_name": "last_name"
},
"ListClaimMappings": {
    "user.metadata.position ": "band_positions"
}
}
```

此配置只是允许 Consul 与 OIDC 提供商通信并提取特定的账户信息（名字、姓氏和职位）。然而，除非我们配置 Consul 将该元数据与特定角色关联，否则这些信息毫无意义。让我们假设 Consul 中有一个吉他手的角色，由两个策略组成。我们可以使用以下示例命令创建该角色：

```
$ consul acl role create -name guitarists -policy-name rhythm -policy-name lead
```

太棒了！我们已在 OIDC 提供商中创建了我们的账户，并且在 Consul 中定义了一个角色，该角色将账户的特定属性与一组策略相关联。但是，我们如何链接这两条信息？这可以通过所谓的绑定规则来完成。现在，我们之前没有谈论过这些，但绑定规则将一种身份验证方法与特定角色关联。绑定规则可以应用于应用程序身份验证（例如，如果你想使用 Java Web Token 验证身份验证），但我们这里的重点是验证人类。特别是，我们希望具有吉他手职位的用户能够访问吉他手 Consul 角色。请注意拼写上的细微差别（guitarist 与 guitarists）。这是为了在整个过程中区分术语。

够谈论我们的绑定规则了。如果我们想创建一个，我们可以使用以下示例：

```
$ consul acl binding-rule create -method=<oidc_name> -bind-type=role -bind-name=guitarists -selector='guitarist in list.band_positions'
```

在这里，我们使用在 Consul 中配置 OIDC 提供商时给出的 OIDC 提供商的名称。我们声明任何使用该方法登录 Consul 的人都需要使用关联的 OIDC 提供商进行身份验证。当我们收到尝试身份验证的用户元数据时，我们会在 band_positions 列表中查找，如果我们找到 guitarist，我们就可以将 guitarists 角色（以及因此关联的所有策略）分配给这些用户。现在，为什么我们要经历所有这些工作，而我只需要创建令牌并将这些令牌交给用户呢？

让我们假设我们已经完成了为吉他手创建角色的过程，并且该角色拥有自己的令牌。我们可以将同一个令牌交给 Adrian 和 Dave，他们就可以使用该令牌通过 CLI 或 Web 界面使用 Consul 进行身份验证。这里出现问题一；当我们跟踪审计日志时，如何区分 Adrian 和 Dave？当我们只有一个用户时可能没那么糟糕，但当 Jannick（另一名吉他手）加入乐队时，区分用户变得更加困难（尽管他们的吉他风格截然不同）。

现在，我承认所有这些都可能令人困惑，所以让我列出顺序：

## Consul OIDC Authentication

![](img/52479dafaefba00517c48a137e037b7e_93_0.png)

Figure 3.7 – OIDC authentication process

我提到过需要区分 Adrian、Dave，现在是 Jannick，因为他们登录 Consul 系统。这是为了区分每个个人在审计日志中执行的操作。这并不是为了站在后面指责，而是为了确保流程得到遵循并且系统中的更改得到跟踪。

在 Consul 服务器中启用审计日志非常简单。启用后，Consul 将输出条目以跟踪谁做了什么以及何时做，所有内容都写入文件。别担心——不会包含机密信息，这使得启用 OIDC 等身份验证方法以唯一区分用户变得更加重要。日志以 JSON 格式写入文件，可以被大多数日志分析实用程序（如 DataDog、Splunk 和 Elasticsearch）摄取。但是，在配置日志记录时，请务必设置日志文件数量和轮换频率的限制……没有人喜欢承认自己因日志记录而耗尽了空间！

```
```hcl
audit {
  enabled = true
  sink "consul_audit" {
    type   = "file"
    format = "json"
    path   = "data/audit/audit.json"
    delivery_guarantee = "best-effort"
    rotate_duration = "24h"
    rotate_max_files = 15
    rotate_bytes = 25165824
  }
}
```
```

日志文件的输出标识了几个重要的信息，特别是时间戳和令牌访问者。此访问者可以链接回访问系统的个人令牌和角色（因此是用户）。我个人发现审计日志中的源 IP 地址非常有趣，尤其是当我可以进行地理关联并确定谁从世界各地访问系统时。当然，条目还包括他们对系统做了什么！

```
```json
{
  "created_at": "2020-08-25T10:35:23.258765383Z",
  "event_type": "audit",
  "payload": {
    "id": "3a880e2b-3f43-5d4d-97a6-26aec5eacc0c",
```

```
```json
"version": "1",
"type": "HTTPEvent",
"timestamp": "2020-08-25T10:35:23.258548744Z",
"auth": {
  "accessor_id": "48162132-6b28-de21-aee9-6903d2024f2e",
  "description": "token created via OIDC login",
  "create_time": "2020-08-25T10:35:23.158336091Z"
},
"request": {
  "operation": "GET",
  "endpoint": "/v1/catalog/datacenters",
  "remote_addr": "172.17.0.1:48516",
  "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.125 Safari/537.36",
  "host": "192.168.64.243:31394"
},
"stage": "OperationStart"
}
```
```

正如你所见，启用这些审计日志是操作和管理任何企业中的 Consul 系统的一个关键方面。因为我们只是正在学习系统的从业者，所以它并不那么有趣（我们应该知道我们做了什么以及何时做，尽管我承认我可能比应该更频繁地使用 Bash 历史记录）。然而，当 Consul 推广到企业环境时，跟踪这些操作与确保系统实际运行一样关键！说到运行，我希望你准备好了。我强烈建议此时你起身走走或伸展双腿，因为接下来会变得相当复杂。

## Securing your Consul cluster

在开始保护你的集群之前，我需要提出一个警告。当你处理安全执行时，事情可能会变得有点棘手，尤其是在你不小心将自己锁在自己的集群之外时。幸运的是，我们只是在这里使用一个简单的开发系统。然而，在生产环境中，必须格外小心。我已尽可能自动化，但在此过程中的任何时候，你都可以销毁整个基础设施集并重建它。啊，Terraform 和临时基础设施的奇迹和美丽！





All the code for this section can be found in the following GitHub repository:
https://github.com/PacktPublishing/Simplifying-Service-Management-with-Consul/tree/main/ch3

## Creating the image

If you've already forked or downloaded the repository, you should have the corresponding folder for this chapter. This is where we'll be doing all our work. We are going to be building a new image for this chapter that includes all the certificates that have been generated for you.

As we build the system and look at the code, we're going to focus on the differences between what we did in Chapter 2, Architecture – How Does It Work?, and what we are doing here, simply for brevity. Unless you actually shut down your machine or closed your Terminal, you should still have your AWS keys: AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY. To check whether you still have these set properly, run the following command:

```
$ env | grep AWS
AWS_ACCESS_KEY_ID=AKIA26CFBA32D6JFVJJB
AWS_SECRET_ACCESS_KEY=NYaSFhAzZqOq4bqSGJda/Y22Guc3p...
```

Since you are all Packer experts by now, we're going to kick off the build so that we can review what we are doing while the image is created. To do that, you need to be in the proper folder:

```
$ cd $github_root/Consul-Up-and-Running/ch3/Image-Creation
```

Now, kick off your Packer building using the following command:

```
packer build -var-file="variables.pkrvars.hcl" .
```

Now, while the Christmas lights are spewing across your screen, let's take a look at the differences between what you are building now and what you built in Chapter 2, Architecture – How Does It Work?.

The first thing you might recognize is that in the source definition, we have slightly changed the name of the image to be used. This is just because we only want to be using this image to understand the security aspects of Consul. As we move forward, we will be using the insecure image. *But Rob, why would it be insecure?* I've been asking this myself nearly all my life! As we will see, once we secure our cluster, it becomes more much difficult to utilize and navigate. This is by design because, in a production environment, you don't want to make it easier for those nefarious hackers:

```
```hcl
source "amazon-ebs" "ubuntu-image" {
  ami_name      = "${var.owner}-secure-consul-${timestamp}"
  region        = "${var.aws_region}"
  instance_type = var.aws_instance_type
  tags = {
    Name = "${var.owner}-secure-consul"
  }
}
```
```

We aren't really changing things that drastically, only slightly modifying the name to identify it as the secure image base. One of the small changes for the secure image is the presence of the CA file, `consul-agent-ca.pem`. Recall that the CA confirms that the certificate being used is valid. Marge needs to be sure that her kids are all using valid certificates! We could use a provisioner to move this file to the machines when we run the Terraform script; however, every machine needs the CA file, so we might as well pack that into the image:

```
```hcl
provisioner "file" {
  source      = "../files/consul-agent-ca.pem"
  destination = "/tmp/consul-agent-ca.pem"
}
```
```

We're also going to be moving that file to the proper location within Consul as part of our shell provisioner:

```
```hcl
provisioner "shell"{
  inline = [
    "sudo /usr/local/bin/consul -autocomplete-install",
    "sudo useradd --system --home /etc/consul/consul.d --shell /bin/false consul",
    "sudo mkdir /etc/consul /etc/consul/consul.d /etc/consul/logs /var/lib/consul/ /var/run/consul/",
    "sudo chown -R consul:consul /etc/consul /var/lib/consul/"
```

```
/var/run/consul/",
        "sudo chmod -R a+r /etc/consul/logs/",
        "sudo mv /tmp/consul.service /etc/systemd/system/consul.service",
        "sudo mv /tmp/consul-common.hcl /etc/consul/consul.d/consul-common.hcl",
        "sudo mv /tmp/consul-agent-ca.pem /etc/consul/consul.d/consul-agent-ca.pem"
    ]
```
```

And that's it! These are seemingly small changes, but we should get used to packing our commonalities into the image files to manage how we build out our systems in that crazy place called the *real world*.

Next, we're going to build our Consul system. Again, this process is very similar to what we did previously. We're going to kick off `terraform apply`, and while that is running, we'll take a look at the differences.

## Building the Consul cluster

Now that we've built the image, we can use Terraform to build the cluster itself. All the Terraform code exists in this chapter's root folder, so let's go there:

```
```bash
$ cd $github_root/Consul-Up-and-Running/ch3
```
```

As you may recall, the `terraform.tfvars` file doesn't get copied over from what you configured in *Chapter 2, Architecture – How Does It Work?*. So, you'll have to make sure you edit it so that it matches your Chapter 2 file (or just copy the Chapter 2 `terraform.tfvars` file to the Chapter 3 directory):

# Terraform.tfvars

```
```hcl
owner         = "rjackson"
aws_region    = "us-east-2"
instance_type = "t2.small"
```
```

Now that we've confirmed our variables are to our liking, let's start firing up our infrastructure. Remember that to get started, Terraform needs to download any provider-specific data necessary. This can be done via the following command:

```
```bash
$ terraform init
```
```

Next, we're going to run our plan so that we can see what changes Terraform is going to make. This will be all new infrastructure, so you should just see a bunch of green plus signs:

```
```shell
$ terraform plan
```
```

Let's start building our infrastructure now! Once again, by living dangerously (not recommended in production), we can auto-approve our `apply` command:

```
```shell
$ terraform apply --auto-approve
```
```

OK; let's now look at what we are building. Within the definition of the security groups within the `main.tf` file, we did make a slight modification to include access to port 8501. This is the port that Consul is using for secure communication:

```
```hcl
ingress {
    from_port   = 8501
    to_port     = 8501
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
}
```
```

The majority of the changes, however, are where we are creating our AWS server instances. The big difference here is that we are pushing our private and public keys for each server out to the system. Please note that I'm using very generic certificates that were created by Consul. In most instances, you'll want to use specific certificates for your environment. As the process of creating and distributing the certificates may cause even the most stable person to lose their mind, I've done a lot of that for you. If you wish to follow the process of creating the certificates, the following command must be run for every server. These commands are simply for informative purposes as certificates have already been embedded in the infrastructure:

```
```shell
consul tls cert create
```
```

You can use this command to create server and client certificates, and even a certificate for the CLI to access the Consul system. They would all use the same CA file that is also produced by the command. The output would be as follows:

```
```text
consul tls cert create -server
==> WARNING: Server Certificates grants authority to become a
    server and access all state in the cluster including root
keys
```

```
and all ACL tokens. Do not distribute them to production hosts
that are not server nodes. Store them as securely as CA keys.
==> Using consul-ca.pem and consul-ca-key.pem
==> Saved dc1-server-consul-0.pem
==> Saved dc1-server-consul-0-key.pem
```

This command would need to be executed for each of the three servers in the server cluster, and the files would need to be transferred over. For simplicity's sake, I've created the certificates ahead of time and placed them in their proper positions. A big part of the changes that have been made to the Terraform script in the `main.tf` file is due to the placement of these files. It can't be stressed enough that in a production environment, you should be using certificates provided by your security team (or through automation with HashiCorp's Vault):

```
main.tfresource aws_instance "consul-server" {
  count                       = 3
  ami                         = data.aws_ami.an_image.id
  instance_type               = var.instance_type
  key_name                    = aws_key_pair.consul_key.key_name
  associate_public_ip_address = true
  subnet_id                   = aws_subnet.consul-demo.id
  vpc_security_group_ids      = [aws_security_group.consul-demo.id]
  iam_instance_profile        = aws_iam_instance_profile.instance_profile.name
  user_data = templatefile("files/server_template.tpl", {
    server_name   = "${var.owner}-consul-server-instance",
    server_number = count.index})
  tags = {
    Name     = "${var.owner}-consul-server-instance"
    Owner    = var.owner
    Instance = "${var.owner}-consul-server-instance-${count.index}"
  }
  provisioner "file" {
    source      = "files/dc1-server-consul-${count.index}.pem"
    destination = "/tmp/dc1-server-consul-${count.index}.pem"
```





```
```hcl
}
provisioner "file" {
  source      = "files/dc1-server-consul-${count.index}-key.pem"
  destination = "/tmp/dc1-server-consul-${count.index}-key.pem"
}
provisioner "file" {
  source      = "files/server_acl.hcl"
  destination = "/tmp/server_acl.hcl"
}
provisioner "remote-exec" {
  inline = [
    "sudo mv /tmp/*.pem /etc/consul/consul.d/"
  ]
}
connection {
  type        = "ssh"
  user        = "ubuntu"
  private_key = local_file.private_key.content
  host        = self.public_ip
}
}
```
```

需要关注的主要点是 `user_data` 脚本中的细微变化。与其简单地通过配置传递服务器名称，我们还包含了索引。这是因为我们用于证书的文件都遵循相同的命名结构，并且索引嵌入在文件名中。这些文件都可见于 `files` 文件夹（`$github_root/ch3/files`）：

- `files/consul-agent-ca-key.pem`
- `files/consul-agent-ca.pem`
- `files/dc1-server-consul-0-key.pem`
- `files/dc1-server-consul-0.pem`
- `files/dc1-server-consul-1-key.pem`
- `files/dc1-server-consul-1.pem`
- `files/dc1-server-consul-2-key.pem`
- `files/dc1-server-consul-2.pem`

`consul-agent-ca-key.pem` 文件是 CA 的私钥（Consul 是我们的 CA）。公共 CA 文件 `consul-agent-ca.pem` 已作为镜像构建过程的一部分推送到所有机器。通过在 `aws_instance` 资源中使用 provisioner，我们将剩余文件传输到三个服务器（0、1 和 2）中的每一个，并将它们放置在 `/etc/consul/consul.d/` 目录中。

利用文件 provisioner，我们还在服务器上添加了一个 ACL 配置文件。这只是为了帮助我们在开始使用 ACL 时使用。当我们配置客户端时，我们必须做一些小的更改。首先，我们必须将一个新的配置文件移动到客户端以帮助 ACL 配置。我们还将 CA 文件添加到配置中：

```
```hcl
resource null_resource "provisioning-clients" {
  for_each = { for client in aws_instance.consul-client :
  client.tags.Name => client }
  provisioner "file" {
    source      = "files/httpd.json"
    destination = "/tmp/httpd.json"
  }
  provisioner "file" {
    source      = "files/client_acl.hcl"
    destination = "/tmp/client_acl.hcl"
  }
  provisioner "remote-exec" {
    inline = [
      "sudo cat << EOF > /tmp/consul-client.hcl",
      "advertise_addr = \"${each.value.public_ip}\"",
      "server = false",
      "enable_script_checks = true",
      "bind_addr = \"${each.value.private_ip}\"",
      "retry_join = [\"${aws_instance.consul-server[0].public_ip}\",\"${aws_instance.consul-server[1].public_ip}\",\"${aws_instance.consul-server[2].public_ip}\"]",
      "client_addr = \"${each.value.private_ip}\"",
      "ca_file = \"/etc/consul/consul.d/consul-agent-ca.pem\"",
      "verify_incoming = true",
      "verify_outgoing = true",
      "verify_server_hostname = true",
      "auto_encrypt = {"
```
```

```
```hcl
  " tls = true",
  "}",
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
  // private_key = file("~/hashicorp/aws/rjackson-east2.pem")
  host        = each.value.public_ip
}
}
```

```

除了将 CA 文件添加到配置中之外，我们还添加了一些其他参数：

```
```hcl
"verify_incoming = true",
"verify_outgoing = true",
"verify_server_hostname = true",
"auto_encrypt = {",
"  tls = true",
"}",
```
```

```
```
}
connect {
  enabled = true
}
EOF
sudo systemctl start consul
```
```

与客户端类似，我们将验证参数设置为确保使用证书文件验证接收的消息以及发送的消息。那些文件在接下来的几行中配置，但请注意，与客户端不同，我们为每个服务器配置了私钥和公钥。服务器集群必须以安全的方式配置和创建，然后才能使用 auto_encrypt 配置启用向客户端分发证书。我想指出配置的端口。在此配置中，我们将端口 8500 配置为 HTTP，这就是我们在 *第 2 章，架构 – 它是如何工作的？* 中使用的方法。然而，我们还具有 HTTPS 配置，端口 8501 专门用于与 Consul 集群的 TLS 通信。因此，如果你禁用 HTTP（将 `http = -1` 设置），你将通过阻止任何未加密的 HTTP 流量来保护你的 Consul 系统。如果你想看看未认证的网页浏览器尝试与 Consul 通信时会是什么样子，假设 Consul 系统正在运行，将你的网页浏览器指向 Terraform 输出中提供的 `CONSUL_HTTPS_ADDR` 参数之一。你会看到类似以下内容：

![](img/52479dafaefba00517c48a137e037b7e_105_0.png)

图 3.8 – 未授权的 HTTP 访问

## 保持安全、愚蠢，并保护你的集群！

如果你检查证书，你会看到它是由 Consul CA 签发的并且不受信任。这是因为证书没有由官方 CA 签名；它仅由 Consul 本身签名。

![](img/52479dafaefba00517c48a137e037b7e_106_0.png)

图 3.9 – 用于 HTTP 通信的证书

值得庆幸的是，我们仍然打开了端口 8500，以便无阻碍地与服务器集群通信。

但等等，还有更多！你还记得我们说过保护通信主要有两条路径吗？一条是通过相互 TLS，使用证书进行 RPC 通信。那我们所有服务器的 gossip 呢？没错，我们也必须保护它，但不用担心，这要容易得多，而且像证书一样，我已经提前为你设置好了。

如果你查看 `consul-common.hcl` 文件，你会看到一个额外的行，其中包含一个简单的参数 `encrypt`。该文件已作为使用 Packer 的镜像构建过程的一部分放置在所有服务器上：

```
log_file = "/etc/consul/logs/"
log_level = "DEBUG"
encrypt = "86753098Qr2LsXLcfD+Y+S6RUuzUZsY1ZxdAj5E9c="
encrypt_verify_incoming = true
encrypt_verify_outgoing = true
```

这个值并不是我凭空编造的——好吧，不完全是。通过使用 `consul keygen` 命令，你可以生成一个加密密钥，然后用于加密节点之间的 gossip。由于该参数位于 `common` 配置中，所有节点都使用相同的密钥。我们还确保每个代理都在加密其流量（`encrypt_verify_outgoing`）并且只接受已加密的流量（`encrypt_verify_incoming`）。如果你想更新此加密密钥（强烈建议），你可以使用 `consul keyring` 命令。

此时，你已经拥有一个完全设置好的 Consul 集群，所有节点之间运行受保护的消息，既用于 RPC 通信，也用于 gossip 通信。如我之前所述，将证书和加密参数作为构建的一部分包含在内的决定是为了简化流程。根据我的经验，创建和分发证书的过程具有挑战性，如果你没有机器的官方注册域名则更具挑战性。此过程应由你的安全团队完成，因此在设计 Consul 机器之间的安全通信时，请务必让他们参与其中。

## 让我们玩玩一些 ACL！

现在我们的集群已经启动并运行，让我们开始玩一些 ACL。我不想在构建过程中启用 ACL，仅仅因为没有令牌，我们将无法访问我们的 Consul 系统！我还想让你看到系统在默认拒绝策略下是如何被锁定的，然后我们再慢慢打开它。我们在这里将使用 SSH 和构建过程中创建的密钥在机器之间频繁跳转。在真实场景中，像我们的构建一样，其中很大一部分，甚至全部，都可以用 Terraform 作为代码执行，所以如果你觉得这个过程很痛苦，你可能想接下来读一本 Terraform 书籍。




我们需要做的第一件事是修改服务器配置以启用 ACL。文件的结构已经放置在服务器上，所以我们只需要把它放到正确的位置，然后就可以开始操作了。现在，我们需要在集群中的**每个**服务器上执行这些操作，因为如果你只在一台服务器上启用了 ACL，你可能会出现狗和猫和谐共处的局面，而我们绝对不能允许这种情况发生！

如果你仍然能看到你的 Terraform 输出，那太好了——请务必将它们复制到一个临时文本文件中以便使用。那里有很多有用的信息。如果你是。

*现在我们有了这个神级令牌，我们需要保存它！无论你把客户端和服务器的 IP 地址复制到哪里，也把这个 bootstrap 令牌复制到那里。* 你永远不知道什么时候需要这种级别的访问权限。然而，我们确实需要为我们的服务器创建一个更合理的令牌。再次强调，我们不希望那个 root 令牌到处乱飞，虽然我们可以做到非常精细的控制，但我们现在只是设置一下，让我们熟悉使用 ACL 并了解一些相关知识。

所以，我们的第一个目标是为我们的服务器创建一个包含两个简单规则的新策略。我们需要对节点和服务具有写访问权限。作为此过程的一部分，我们需要创建策略文件，这可以通过文本编辑器完成，也可以通过使用以下命令完成：

```sh
cat > first-policy.hcl << EOF
node_prefix "" {
    policy = "write"
}
service_prefix "" {
    policy = "read"
}
EOF
```

如果你能够正确地复制粘贴（或者如果你使用的是纸质书籍，输入正确），文件应该如下所示：

```hcl
node_prefix "" {
    policy = "write"
}
service_prefix "" {
    policy = "read"
}
```

根据我们的专业知识，我们知道这允许节点写入所有节点级操作，并且能够读取所有服务级信息。使用此文件，我们将在 Consul 中创建一个策略。这可以通过以下命令完成：

```sh
$ consul acl policy create -name "first-token" -description
"Server Token Policy" -rules @first-policy.hcl
```

此命令的输出应包含所有策略信息：

```
ID:            c034c84d-f4f6-6bb6-4e96-8fc9a4d7dbae
Name:          first-token
Description:   Server Token Policy
Datacenters:
Rules:
node_prefix "" {
    policy = "write"
```

请务必再次在三个服务器上重启 Consul，这样所有服务器节点就可以使用它们的新令牌了！

现在我们的服务器节点已经设置好了，让我们看看客户端节点会发生什么。我们网络中有五个客户端节点，当你运行 `consul members` 时，它们仍然都在。这只是因为服务器正在维护与这些机器的连接。一旦这些机器上的 Consul 服务被重启，该节点将不再能够在服务器集群上注册自己。如果你不相信我，试试看！

你还记得我让你保存的那份 IP 地址列表吗？我希望你能随手拿到，因为我们需要它来访问客户端。让我们打开一个新的终端标签页或窗口，并 SSH 到其中一个客户端。任何客户端都可以，所以从列表中随便挑一个。我喜欢挑一个有特殊数字的：

```bash
$ ssh -i rjackson-consul-key.pem ubuntu@3.142.52.65
```

查看服务器的主机名。它就在命令行里：

```bash
ubuntu@ip-192-168-100-77:~$
```

在你的服务器上，再次运行那个 `consul members` 命令并查找该主机。它应该在那里，但它即将被禁止进入你的 Consul 世界！重启该客户端代理：

```bash
$ sudo systemctl restart consul
```

等一分钟左右，然后在服务器上再次检查 `consul members` 输出。你会看到该代理已离开集群，但它仍然在那里！它可能在，但它不再被授权加入集群：

```text
ip-192-168-100-77   3.142.52.65:8301    left     client  1.9.0
2                  dc1  <default>
```

你觉得那个客户端节点上发生了什么？我们可以通过检查其状态来获得提示。在该客户端节点上执行以下命令：

```bash
$ sudo systemctl status consul
```





在输出中，你会看到代理正在运行，但你也会看到一些错误：

```
consul.service - Consul server agent
    Loaded: loaded (/etc/systemd/system/consul.service; disabled; vendor preset: enabled)
    Active: active (running) since Sat 2021-04-03 23:37:57 UTC; 3min 57s ago
    Process: 2990 ExecStartPre=/bin/chown -R consul:consul /var/run/consul (code=exited, status=0/SUCCESS)
    Process: 2979 ExecStartPre=/bin/mkdir -p /var/run/consul (code=exited, status=0/SUCCESS)
    Main PID: 2991 (consul)
    Tasks: 7 (limit: 2347)
    CGroup: /system.slice/consul.service
            └─2991 /usr/local/bin/consul agent -config-dir=/etc/consul/consul.d/ -data-dir=/var/lib/consul/ -pid-file=/var/run/consul/consu

Apr 03 23:39:04 ip-192-168-100-77 consul[2991]:
2021-04-03T23:39:04.935Z [DEBUG] agent.auto_config: making AutoEncrypt.Sign RPC: addr=
Apr 03 23:39:04 ip-192-168-100-77 consul[2991]:
2021-04-03T23:39:04.943Z [ERROR] agent.auto_config: AutoEncrypt.Sign RPC failed: addr=
Apr 03 23:39:04 ip-192-168-100-77 consul[2991]:
2021-04-03T23:39:04.944Z [ERROR] agent.auto_config: No servers successfully responded
```

哦！RPC 失败了，而且没有服务器成功响应！你觉得这是为什么？如果你猜到了 ACL，那么你真的开始从这本书中学习了。我想我的英语也没那么糟糕！

现在我们已经排斥了我们的客户端节点，我们需要帮助它重新回到服务器家族中。你可以用全部五个客户端来做这件事，如果你愿意的话，但在这一点上，操作一个客户端就足够了。正如你可能还记得的那样，部分配置过程包括将一个 client_acl.hcl 文件移动到客户端，类似于我们对服务器所做的那样。让我们把那个客户端文件移动到正确的目录：

```
$ sudo mv /tmp/client_acl.hcl /etc/consul/consul.d/
```

如果我们查看那个文件，我们会看到它几乎与我们放置的服务器文件相同。只有一个东西缺失——合适的令牌：

```
acl = {
    enabled = true
    default_policy = "deny"
    enable_token_persistence = true
    tokens {
        "default" = ""
    }
}
```

所以，在我们再次重启那个客户端节点之前，为什么不为该节点创建一个新的策略和令牌呢？这个策略会更具体一些。要创建这些项目，我们需要回到我们之前使用的 Consul 服务器。如果你不再在环境中拥有 bootstrap 令牌，你将需要它再次。你可以通过查看环境变量来检查这一点：

```
$ env | grep CONSUL
```

输出应包含你正在操作的服务器的 Consul 地址，以及它的 HTTP 令牌：

```
CONSUL_HTTP_TOKEN=34e87e9d-2207-f2eb-7efc-d41674eca6f0
CONSUL_HTTP_ADDR=http://192.168.100.146:8500
```

如果你有这两个值，并且它们是正确的，那么我们可以继续。

我们将为此节点创建一个特定的策略。我正在操作的客户端节点的 hostname 为 ip-192-168-100-77。我要创建的策略规则将启用节点级和代理级的写操作，但仅限于该节点。让我们先创建该策略文件，类似于我们之前为服务器所做的那样：

```
$ cat > client-policy.hcl << EOF
node "ip-192-168-100-77" {
    policy = "write"
}
agent "ip-192-168-100-77" {
    policy = "write"
}
EOF
```

生成的文件应该看起来类似这样，但我这里的节点 `ip-192-168-100-77`，你应该用你的客户端节点的 hostname 代替。现在，我们必须用这个文件创建策略：

```
$ consul acl policy create -name "client-token" -description "Client ip-192-168-100-77 Token Policy" -rules @client-policy.hcl
```

此命令的输出应包含所有策略信息：

```
ID:                a1fb3a7c-2a42-613d-4586-5dadfb422022
Name:              client-token
Description:       Client ip-192-168-100-77 Token Policy
Datacenters:
Rules:
node "ip-192-168-100-77" {
    policy = "write"
}
agent "ip-192-168-100-77" {
    policy = "write"
}
```

正如我们之前所做的那样，我们现在必须为该策略创建一个新的令牌。要创建此令牌，我们将执行以下命令：

```
$ consul acl token create -description "ip-192-168-100-77 Token" -policy-name "client-token"
```

该命令的输出如下：

```
AccessorID:          b0b8cee4-8efd-62d2-e22c-fc84a0dc3c92
SecretID:            776f0c07-6352-8d2f-8fc7-23f94cbafd74
Description:         ip-192-168-100-77 Token
Local:               false
Create Time:         2021-04-04 00:02:08.863705384 +0000 UTC
Policies:
    a1fb3a7c-2a42-613d-4586-5dadfb422022 - client-token
```

我们现在有一个新的 Secret ID 值，可以将其放在 client_acl.hcl 中。如果我们使用 Terraform 和 Vault，这不就容易多了吗？

回到你登录正在操作的客户端的标签页，编辑 client_acl.hcl 文件以插入你的新令牌：

```
$ sudo vim /etc/consul/consul.d/client_acl.hcl
```

文件应该看起来像这样——当然是用你的令牌，而不是我的！

```
acl = {
    enabled = true
    default_policy = "deny"
    enable_token_persistence = true
    tokens {
        "default" = "776f0c07-6352-8d2f-8fc7-23f94cbafd74"
    }
}
```

好的；我们有配置和令牌。你准备好现在重启那个 Consul 服务了吗？我知道我准备好了！

```
$ sudo systemctl restart consul
```

你可以在客户端节点上再次检查状态，或者如果你更兴奋，可以回到你的服务器节点并运行另一个 consul members。你能看到你一直在操作的节点吗？我非常希望如此！

正如你可能还记得的那样，所有这些节点都同时运行着 Consul 服务以及在我们设置系统时部署的 HTTPD 服务。让我们检查一下这些服务是否已在该节点上注册。我们可以通过命令行在服务器节点上使用以下命令来完成此操作：

```
$ consul catalog services -node=ip-192-168-100-77
```

> Note
> 如果你运行这样的命令并最终出现 ACL 错误，你可能不在环境变量中拥有 bootstrap 令牌的服务器节点上。

这非常具体到我们的节点，以及在我们的客户端节点上运行的两个服务——正如应该的那样。

让我们用新的规则更新我们的 client-token 策略：

```
$ consul acl policy update -name "client-token" -rules=@client-policy.hcl
```

输出应确认你的新服务规则已写入策略：

```
ID:           a1fb3a7c-2a42-613d-4586-5dadfb422022
Name:         client-token
Description:  Client ip-192-168-100-77 Token Policy
Datacenters:
Rules:
node "ip-192-168-100-77" {
    policy = "write"
}
agent "ip-192-168-100-77" {
    policy = "write"
}
service "httpd" {
    policy = "write"
}
```

太好了！我们在服务器节点上更新了策略，而从未返回客户端。这里是最终测试；你的 httpd 服务现在是否显示在该节点的目录中？运行以下命令来找出答案：

```
$ consul catalog services -node=ip-192-168-100-77
```

你应该在输出中看到 httpd 服务列出。如果是这样，就站起来向世界大喊，我已经掌握了 Consul 访问控制列表！好吧，掌握可能有点夸张，但你应该为自己在这里取得的成就感到非常自豪！

## Summary

哇——又一个很长的章节！我不知道你怎么样，但我真的筋疲力尽了！虽然这是一本*入门*书，但使用 TLS 证书进行安全通信的概念并不是家庭晚餐的轻松话题。然而，由于 Consul 是网络的关键服务，它维护应用程序和服务信息，因此对节点进行身份验证并加密其通信路径的必要性不言而喻。此外，Consul ACLs 提供了如此多的灵活性，以至于它们可能非常令人不知所措。然而，它们的强大功能和控制应该是任何生产环境中的核心组件。我强烈建议你继续操作其他客户端节点，为它们创建一些新的策略和令牌，看看你能管理什么。如果你担心搞坏东西，不用担心。你始终可以使用 Terraform 来销毁和重建基础设施。说到这个，别忘了销毁你今天构建的东西……我们不想月底收到 AWS 账单而苦恼！一旦你销毁了基础设施，放松一下，喝点饮料。下一章会轻松一些，我保证。那里，我们将讨论如何将 Consul 扩展到多个数据中心并连接集群。我们快到一半了！




