

## 私有区块链上的服务设置

在我们的私有区块链上设置推文服务非常简单。首先，我们需要在项目文件夹中使用 geth 命令行启动我们的 **进程间通信 (IPC)**，如 _第 3 章《智能合约的 Hello World》_ 中所述。

```bash
$ geth --datadir=./chaindata/
```

然后我们打开 Mist 浏览器并进入新合约选项。_图 9.15_ 表示在我们的私有区块链上部署智能合约账户：

![](img/8c91673d39176d2e910ac52c4bc35d8b_309_0.png)

图 9.15： 在 Mist 中编译 TwtAccount

我们以图 9.16 所示的类似方式编译注册表智能合约。在点击部署按钮之前，我们需要在 geth 命令行中使用 `$ miner.start(1)` 命令启动私有矿工。我们可以使用 `$ miner.stop(0)` 命令停止矿工：

![](img/8c91673d39176d2e910ac52c4bc35d8b_310_0.png)

图 9.16： 在 Mist 中编译 TwtRegistry

要部署这些合约，我们需要指定 gas 数量并提供密码 new123456，如第 3 章《智能合约的 Hello World》中所做。然后这些合约在矿工确认后在私有区块链中设置，如图 9.17 所示：

![](img/8c91673d39176d2e910ac52c4bc35d8b_311_0.png)

图 9.17： 推文合约正在被挖矿

请注意，我们需要将 Mist 浏览器升级到 0.9.2 或更高版本，否则源代码将无法编译，这是由于旧编译器中的某些错误所致。此外，该编译器中存在一个正在进行的错误，会隐藏现有的覆盖声明。一种变通方法是不要对函数和变量使用相同的名称。在这里，我们使用变量名 isAdminTrue 代替 isAdmin。

### 读取推文

通过对智能合约进行后挖矿，函数可以从智能合约账户中使用，以读取用户在私有区块链上发布的推文。在这里，我们在图 9.18 中通过使用 Remix 浏览器展示了一个不同的变体。我们作为测试在区块链上部署了三条推文：

- Tweet0: "Hello Blockchain."
- Tweet1: "Hello Ethereum."
- Tweet2: "This is a junk tweet which contains spurious characters in order to exceed the 160 character limit which is being used as a negative test case and check the correctness of the smart contract operation."

我们可以在图 9.18 中看到推文数量仅返回两条，因为第三条推文被拒绝了：

![](img/8c91673d39176d2e910ac52c4bc35d8b_312_0.png)

图 9.18： 使用 Remix 浏览器进行 Tweeter 智能合约测试

### 总结

我们从了解互联网货币背后的概念开始了本章。我们看到可编程货币就像建造一栋混凝土房屋，需要大量的前期规划。然后我们讨论了智能合约和区块链的各种用例。我们列出了社会和经济几乎每个方面都可能被这项革命性技术颠覆。我们通过设计一个去中心化的微型博客应用程序并将其部署到我们的私有区块链中结束了本章。

在第 10 章 _BaaS 和暗网市场_ 中，我们将讨论某些企业级框架，它们使用以太坊区块链来设置开发平台。我们还将一窥暗网市场以及如何安全地探索它。

## BaaS 和暗网市场

本章向您介绍各种特定于智能合约和区块链开发的平台。我们将从关于许可光谱上的私有、联盟和公共区块链的讨论开始。然后我们将深入探讨 **区块链即服务 (BaaS)** 的概念并介绍企业平台，例如 Hyperledger、Azure 和 AWS。我们通过探索神秘的暗网市场并附带洗钱说明来结束本章。

学习本章后，您将能够：

- 识别不同类型的区块链
- 理解区块链即服务的概念
- 了解 Hyperledger、Azure 和 AWS
- 探索暗网市场

###  playful privacy

您可能已经注意到，在之前的章节中，所有示例都是在私有区块链上编写的，我们在第 3 章 _Hello World of Smart Contracts_ 中使用创世区块创建了它。这样做是因为私有区块链是行业所偏好的；除了让事情变得更容易玩之外，无需等待区块或花费真钱。加密货币如 ETH 和 ETC 位于以太坊区块链之上，与我们的私有链相比是完全不同的链类型。困惑吗？

让我们深入研究不同类型的区块链以了解这种分类。

### 区块链类型

在深入研究区块链类型之前，我们必须熟悉分布式账本技术 aka DLT 的许可光谱概念。区块链是一种 DLT，并在许可光谱中存在。

要理解许可光谱，我们需要确定该光谱的两个极端。在一端，我们必须寻求实体完全许可才能参与共识，而在另一极端，我们无需向任何实体寻求任何许可即可参与共识。

许可极端的有趣之处在于，我们没有义务向参与共识的任何人透露我们的身份。随着我们向完全许可移动，在许可的中部光谱某处，需要透露此身份方面才能参与共识，只是为了验证我们在共识中的权益。当我们到达完全许可时，我们需要识别自己并获得当局的许可才能参与共识。

任何实体或当局寻求许可的这种意识形态危及去中心化的理念，并且是区块链社区和传统企业中严重且经常激烈辩论的话题。

回到 DLT，区块链被称为许可区块链，如果：

- 它限制了可以为系统状态共识做出贡献的参与者
- 只有一组受限的已识别用户有权验证区块交易
- 当局可以限制访问已批准的可以创建智能合约的参与者
- 需要当局许可才能发送/接收交易

许可区块链最终使用 RAFT 或 Paxos 等共识算法（参见第 1 章《区块链基础》），因为其所有参与者的身份都被揭示。为验证区块所花费的精力不会由系统奖励，参与者最终通过充当中介向终端用户收取费用。

企业级金融机构喜欢这种许可区块链的概念，因为它们可以享受比特币或其他加密货币提供的无边界交易设施，但在其私有高速网络中具有精细的访问控制、参与机构批准组内的隐私以及在其内网上的可扩展性。

另一方面，许可区块链是：

- 任何人都可以加入网络
- 任何人都可以参与区块验证过程
- 任何人都可以创建智能合约

以太坊和比特币区块链是许可区块链的绝佳示例。区块验证过程类似于挖矿，并且由系统高度激励。这些区块链使用 PoW 等共识算法，未来可能是 PoS，其中参与者可能需要透露其所有权以申领其权益。但参与本质上仍然是无许可的，与所有权揭示无关。

图 10.1 总结了区分许可光谱两个极端的关键特征：

![](img/8c91673d39176d2e910ac52c4bc35d8b_319_0.png)

图 10.1： 许可型与无许可型

比特币和以太坊区块链是经过强化的区块链平台。它们经受住了时间的考验，即使在多次黑客攻击和偶尔的市场崩盘后也是如此。Andreas Antonopoulos 写了以下关于比特币的内容，这几乎完全适用于以太坊：

> "Bitcoin is a sewer rat. It's missing a leg. Its snout was badly mangled in an accident in the last year. It's not allergic to anything. In fact, it's probably got a couple of strains of bubonic plague on it which it treats like a common cold. You have a system that is antifragile and dynamic and robust.
>
> Let's take it, cut off its beard, take away its piercings, put it in a suit, call it blockchain, and present it to the board. It's safe. It's got borders. We can apply the same regulations. We can put barriers to entry and create an anti-competitive environment to control who has access. It will be more efficient than our existing banking system."

此时，我们可以介绍三种广泛的区块链类型，即公共、私有和联盟。联盟区块链有时被称为联邦区块链。图 10.2 沿许可光谱说明了这三种类型：

