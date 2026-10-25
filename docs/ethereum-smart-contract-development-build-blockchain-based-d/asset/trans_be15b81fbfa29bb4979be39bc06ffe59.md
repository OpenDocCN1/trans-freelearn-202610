
分配了特定的地址。在我们的示例输入中，我们使用了三位候选人 [Tom、Dick 和 Harry]。

这个 DApp 更有趣的地方在于，当我们将其部署到区块链上时，构造函数只能被调用一次。与普通的 Web 应用程序不同——在普通 Web 应用中，每次同名的新部署都会覆盖旧代码——区块链上部署的代码是不可变的。也就是说，如果我们修改合约并重新部署，旧合约仍将原封不动地存留在区块链上，连同所有存储数据一起。新的部署只会创建一个独立的新合约实例。

代码档案中还提供了一个企业版的投票应用，名为 `corporateVotingDapp.sol`，其中包含对每个函数的详细注释。该版本将 DAO 的购币能力与投票功能结合在一起。在这个智能合约中，除了候选人名称数组之外，我们还传递了两个额外的参数。这两个参数分别是可供选民使用的代币总量，以及每个代币对应的以太币单位。

例如，输入 `10000,1, [Tom、Dick 和 Harry]` 意味着我们有一个投票合约，其中最多有 10,000 个代币可供选民使用，每个代币价值 1 个以太币，选民可以为三位候选人投票。如果某位选民使用 100 个代币投给了 Tom，那么其他选民可以使用剩余的 9,900 个代币投票给这三位候选人中的任意一位。

*图 5.8* 展示了带有必要注释的投票 DApp 的 Solidity 智能合约。*图 5.9、图 5.10、图 5.11、图 5.12 和图 5.13* 展示了私有区块链上的投票 DApp。*图 5.14 和图 5.15* 展示了 Remix 浏览器中简单版和企业版投票 DApp 的候选人得票数输出：

```solidity
pragma solidity ^0.4.11;
contract simpleVotingDapp {
    /*
    The key of the mapping is candidate name stored as type bytes32 and value is
    an unsigned integer to store the vote count
    */
    mapping (bytes32 => uint8) public votesReceived;
    /*
    We use an array of bytes32 instead to store the list of candidates
    */
    bytes32[] public candidateList;
    /* This is the constructor which will be called once when we
    deploy the contract to the blockchain. When we deploy the contract,
    we will pass an array of candidates who will be contesting in the election
    e.g.["Tom","Dick","Harry"]
    */

    function simpleVotingDapp(bytes32[] candidateNames) {
        candidateList = candidateNames;
    }

    // This function returns the total votes a candidate has received so far
    function totalVotesFor(bytes32 candidate) returns (uint8) {
        if (validCandidate(candidate) == false) revert();
        return votesReceived[candidate];
    }

    // This function increments the vote count for the specified candidate. This
    // is equivalent to casting a vote
    function voteForCandidate(bytes32 candidate) {
        if (validCandidate(candidate) == false) revert();
        votesReceived[candidate] += 1;
    }

    function validCandidate(bytes32 candidate) returns (bool) {
        for(uint i = 0; i < candidateList.length; i++) {
            if (candidateList[i] == candidate) {
                return true;
            }
        }
        return false;
    }
}
```

图 5.8：简单的 votingDapp.sol

![](img/8c91673d39176d2e910ac52c4bc35d8b_187_0.png)

图 5.9：私有区块链上以候选人数组作为构造函数参数的 VotingDapp

![](img/8c91673d39176d2e910ac52c4bc35d8b_188_0.png)

图 5.10：矿工在私有区块链上部署合约

![](img/8c91673d39176d2e910ac52c4bc35d8b_188_1.png)

图 5.11：为候选人 2 投票

![](img/8c91673d39176d2e910ac52c4bc35d8b_189_0.png)

图 5.12：候选人 2 的投票交易已完成

![](img/8c91673d39176d2e910ac52c4bc35d8b_189_1.png)

图 5.13：候选人 2 投票交易成功后，得票数从 0 变为 1

图 5.14：Remix 浏览器上的 VotingDapp

![](img/8c91673d39176d2e910ac52c4bc35d8b_191_0.png)

图 5.15：Remix 浏览器上的企业版 votingDapp

至此，本章即将结束。话虽如此，智能合约仍然是一个快速发展的主题。在第 6 章《Solidity 深入》中，我们将讨论 Solidity 编程语言，以及它如何帮助创建复杂的智能合约。

# 总结

在本章开始时，我们破除了关于智能合约的迷思。我们意识到，智能合约无法自行做出决策，需要算法触发才能实现这样的功能。我们还研究了智能合约的原始定义，并认识到智能合约模型位于一个从编码自然语言到完全自动化的频谱上。我们了解到，代码在智能合约中的作用包括优化、审计和遵守合规要求的明智组合。我们讨论了 Solidity 编译器未能优化的高 Gas 成本代码模式，并将其归类为无用代码和昂贵循环。然后，我们简要了解了如何使用预言机化来提供真实数据，以及 GitHub 归档上的一个问题如何催生了 ERC20 标准。最后，我们在私有区块链上部署了一个去中心化投票应用，以此结束本章。在第 6 章《Solidity 深入》中，我们将深入探讨图灵完备的 Solidity 编程语言。

# Solidity 深入

本章将全面介绍以太坊区块链上图灵完备的 Solidity 语言。我们将首先一窥其设计决策，以及我们究竟为什么需要一门像 Solidity 这样的新语言。接下来的部分将涵盖 Solidity 语言的细微差别及其使用的基本语法。具体来说，我们将涵盖面向合约的特性、函数、事件、继承、库、表达式、控制结构、单位和变量。然后，我们将一窥优化器和调试选项。最后，我们将分析导致近期以太坊区块链 Parity 钱包被黑客攻击的代码缺陷，该攻击盗取了价值约 3000 万美元的以太币。

学习完本章后，你将能够：

+   - 认识到 Solidity 相对于其他高级语言的必要性
+   - 理解 Solidity 的细微差别、语法和特性
+   - 一窥 Solidity 优化器和调试选项
+   - 分析 3000 万美元 Parity 钱包攻击事件背后的缺陷

# 对 Solidity 的需求

让我告诉你们一个关于程序员的肮脏秘密。在整个工程界中，我们是最懒惰的一群技术人员。我们非常努力地工作，就是为了保持懒惰。我们恪守这样一句誓言：

> “如果说需要是发明之母，那么懒惰就是发明之父。”

难怪我们如此热爱开源运动。重用另一个懒惰程序员的代码正是我们所梦寐以求的。而为了增添一点虚伪的色彩，我们也从不忘记抱怨或批评原始代码有多么混乱，以及我们是多么英勇地驱赶了那些 bug，并将其整理成文档，使之成为 2.0 或更高版本。这就是我们的天性。

但是，有时在这片懒惰极客的土地上，会出现一个异类，他完全从头开始编写一些全新的东西。整个懒惰社区随后会嘲笑这些粗糙的代码。渐渐地，这个异类消除了新东西中的 bug，并开始获得关注。尽管如此，我们仍然无视它。此时，这个异类已经继续去做另一件新事物了，而他在获得关注期间的一些狂热追随者正在核心代码之上进行构建。然后，我们懒惰的社区开始与之对抗。批评者发起了著名的 X 与 Y 之争，在在线论坛上提问：“作为一个刚辍学、没有工作的新鲜人，我应该选择哪种语言？是应该选 X——我祖母在大学时用过的那种，还是应该选 Y——我误以为我的孙子会用的那种？”到这个时候，五年已经过去了，这个新事物在 IEEE 最受欢迎编程语言排行榜上高歌猛进，世界一流大学鼓励我们学习它，业界也在尝试围绕它的使用制定事实标准和框架。而我们这些懒惰的领主们，却在疯狂地寻找它的免费副本，以便安装并“掌握”它。

而且，不，它不是 Python（尽管你的猜测已经足够接近了），我指的是……
