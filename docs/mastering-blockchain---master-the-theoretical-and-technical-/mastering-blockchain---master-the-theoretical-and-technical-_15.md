## 值得注意的项目列表

以下是区块链领域目前正在进行的知名项目列表。除了这些项目之外，还有大量初创公司和公司在区块链领域工作并提供区块链相关产品。

### Zcash on Ethereum

以太坊研发团队最近的一个项目是在以太坊上实现 Zcash。这是一个有趣的项目，开发人员正试图使用 Zcash 项目中已有的 zk-SNARKs 为以太坊创建隐私层。通过在以太坊上实现 Zcash，目标是创建一个平台，允许创建投票等隐私至关重要的应用程序。它还将允许在以太坊上创建匿名代币，可用于多种应用。

### CollCo

这是由 Deutsche Börse 开发的项目，基于 Hyperledger 代码库，用于管理商业银行现金转账。**抵押币（Collateralized Coin，CollCo）**提供了一个基于区块链的平台，允许实时转账商业银行资金，同时仍依赖于 Eurex Clearing CCP 提供的传统功能。这是一个可用于解决交割后结算流程低效问题的重要项目。

### Cello

截至 2017 年 2 月，这是 Hyperledger 项目中最新的补充。该项目的目标是提供按需 BaaS，这将使部署和管理多个区块链对用户来说方便且简单。预计 Cello 将支持所有未来和当前的 Hyperledger 区块链，例如 Fabric 和 Sawtooth Lake。

### Qtum

该项目基于将比特币和以太坊区块链功能结合起来的想法。Qtum 使用比特币代码库，但使用以太坊的 EVM 进行智能合约执行。以太坊智能合约可以使用比特币的 UTXO（未花费交易输出）模型运行。它可在 https://qtum.org/ 获取。

### Bitcoin-NG

这是解决比特币区块链可扩展性、吞吐量和速度问题的另一个提案。下一代（NG）协议基于领导者选举机制，该机制在交易发生时立即验证交易，而与比特币协议相比，后者区块之间的时间和区块大小是与可扩展性相关的关键限制。

### Solidus

这是一种新的加密货币，它提供了解决自私挖矿的方案，同时解决扩展和性能问题。它还解决了保密性问题。它基于无许可拜占庭共识。其当前状态的协议相对复杂，是一个开放的研究领域。

### Hawk

这是一个旨在解决区块链中智能合约隐私问题的项目。它是一个智能合约系统，允许对区块链上的交易进行加密。Hawk 能够自动生成与区块链交互的安全协议，而无需手动编程密码学协议。

### Town-Crier

这是一个旨在为智能合约提供真实世界可信数据馈送的项目。该系统基于英特尔 SGX 可信硬件技术。这是预言机设计的进一步发展，智能合约可以在保护机密性的同时从在线来源请求数据。





## SETLCoin

这是一个由 *Goldman Sachs* 构建的系统，并已在 *Cryptographic currency for Securities settlement* 申请下申请专利。顾名思义，这种加密货币 *coin* 可用于快速高效的结算。该技术利用虚拟钱包在网络上的点对点之间交换资产，并允许通过拥有 SETLCoin 实现即时结算。

## TEEChan

这是一种利用 **trusted execution environments (TEEs)** 提供可扩展且高效的比特币区块链扩容解决方案的新颖想法。这与支付通道的概念类似，其中链下通道用于更快地转移交易。该想法的关键吸引力在于，它可以在比特币区块链上实现，而无需对比特币网络进行任何更改，因为它是一种链外解决方案。然而，这里有一个小小的[content]

## SETLCoin

这是一个由 *Goldman Sachs* 构建的系统，并已在 *Cryptographic currency for Securities settlement* 申请下申请专利。顾名思义，这种加密货币 *coin* 可用于快速高效的结算。该技术利用虚拟钱包在网络上的点对点之间交换资产，并允许通过拥有 SETLCoin 实现即时结算。

## TEEChan

这是一种利用 **trusted execution environments (TEEs)** 提供可扩展且高效的比特币区块链扩容解决方案的新颖想法。这与支付通道的概念类似，其中链下通道用于更快地转移交易。该想法的关键吸引力在于，它可以在比特币区块链上实现，而无需对比特币网络进行任何更改，因为它是一种链外解决方案。然而，这里有一个小小的缺点，即该解决方案确实需要信任 Intel 进行远程证明，因为 Intel 的 SGX CPU 用于提供 TEEs。这在去中心化区块链中不是一个理想的属性；不过，需要注意的是，即使使用远程证明，交易的保密性仍然得到保留，因为远程证明者 Intel 无法看到用户之间通信的内容。这一限制使得它是否是一个完全去中心化和无信任的解决方案存在争议。

## Falcon

Falcon 是一个项目，通过提供快速中继网络来广播比特币区块，从而帮助比特币扩容。其核心思想围绕一种减少孤块的技术，从而有助于比特币网络整体的可扩展性。为此目的使用的技术被称为应用级穿透路由。

## Bletchley

该项目由 Microsoft 推出，表明了 Microsoft 对区块链技术的承诺。Bletchley 允许使用 Azure 云服务以用户友好的方式构建区块链。Bletchley 引入的一个主要概念称为 *cryptlets*，可以将其视为一种高级版本的 Oracles，驻留在区块链之外，可以通过安全通道由智能合约调用。它们可以用任何语言编写，并在安全容器内执行。

## Solidity extension for Microsoft Visual studio

此扩展为去中心化应用提供 Intellisense、自动完成功能和模板，并在熟悉的 Visual Studio IDE 中运行，使开发人员更容易熟悉以太坊开发。

## MetaMask

这是一个 DAPP 浏览器，从 DAPP 浏览的角度来看类似于 Mist，但允许用户在浏览器内运行以太坊去中心化应用，而无需运行完整的以太坊节点。这可从 https://metamask.io/ 获取，可作为 Chrome 插件安装。

## Stratis

这是一个区块链开发平台，允许创建定制的私有区块链，并与主 Stratis 区块链 Stratis chain 协同工作以确保安全。它允许轻松部署比特币、以太坊和 Lisk 等主要区块链。此外，它还允许使用 C# .Net 技术进行开发。它还可通过 Microsoft Azure 作为 BaaS 使用。可在 https://stratisplatform.com/ 获取。

## Embark

这是一个用于以太坊的开发框架，允许与前几章中讨论的 truffle 类似的功能。它允许智能合约的自动部署，更轻松地与 JavaScript 集成，尤其是更轻松地与 IPFS 集成。这是一个功能非常丰富的框架，还有更多功能可用。可通过 npm 安装。该框架可在 GitHub 上获取：https://github.com/iurimatias/embark-framework。

## DAPPLE

这是在以太坊上用于更轻松开发和部署智能合约的另一个框架，通过处理更复杂的任务来实现。它可用于包管理、合约构建和部署脚本编写。它也可通过 npm 获取。也可在 GitHub 上获取：https://github.com/nexusdev/dapple。

## Meteor

这是一个用于单页应用的全栈开发框架。可用于以太坊 DAPP 开发。Meteor 中提供了一个开发环境，它允许更轻松、可管理的复杂 DAPP 开发。可在 `https://www.meteor.com/` 获取，以太坊特定 DAPP 构建信息可在 `https://github.com/ethereum/wiki/wiki/Dapp-using-Meteor` 获取。

## uPort

该平台建立在以太坊之上，提供去中心化身份管理系统。这使用户能够完全控制其身份和个人信息。这基于声誉系统的理念，使用户能够相互证明并建立信任。可在 `https://www.uport.me/` 获取。

## INFURA

该项目旨在提供企业级的以太坊和 IPFS 节点。INFURA 由以太坊节点、IPFS 节点和一个名为 Ferryman 的服务层组成，该服务层提供路由和负载均衡服务。
