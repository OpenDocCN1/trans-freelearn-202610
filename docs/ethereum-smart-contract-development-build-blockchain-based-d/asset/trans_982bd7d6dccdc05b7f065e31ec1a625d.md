

```solidity
pragma solidity ^0.4.11;
// define new contract
contract ArithValue{
    uint number;
    function ArithValue() public { //constructor function with default value
        number = 100;
    }
    // constructor function to set new value
    function setNumber(uint theValue) public {
        number = theValue;
    }
    // constructor function to fetch the new value
    function fetchNumber() public constant returns (uint) {
        return number;
    }
    // constructor function to increment by one
    function incrementNumber() public {
        number=number + 1;
    }
    // constructor function to decrement by one
    function decrementNumber() public {
        number=number - 1;
    }
}
```

图 3.12：部署并创建 ArithValue 合约

## 图 3.13：incrementNumber 函数的工作过程

## 图 3.14：setNumber 函数的工作过程

## 图 3.15：decrementNumber 函数的工作过程

## 编写循环

当我们需要重复执行某段代码一定次数时，循环非常有用。与其复制粘贴相同的代码行、让代码块变得更长，不如使用这种编程结构来让代码更简洁。循环可以是有限次的，也可以是无限次的。对于无限循环，我们通常会提供一条 break 语句来跳出循环。

*图 3.16* 中的智能合约使用有限循环来计算有限个整数的总和：

```solidity
pragma solidity ^0.4.11;

contract numberLoop {
    uint number; // unsigned integer is positive integer
    //constructor function with default value
    function numberLoop() public {
        number = 100;
    }
    function myFirstLoop() public returns (uint) {
        for (uint i = 1; i < 10; i++) {
            number = number + i;
        }
        return number;
    }
}
```

图 3.16：Solidity 智能合约中的循环

remix IDE 中的输出如*图 3.17* 所示。如前所述，请注意每次调用 `myFirstLoop` 函数时，Account（账户）标签页中以太币数量的减少：

```
pragma solidity ^0.4.11;

contract numberLoop {
    uint number; // unsigned integer is positive integer
    //constructor function with default value
    function numberLoop() public {
        number = 100;
    }
    function myFirstLoop() public returns (uint) {
        for (uint i = 1; i < 10; i++) {
            number = number + i;
        }
        return number;
    }
}
```

图 3.18：调试模式以识别执行状态

至此，我希望你已经非常习惯于在浏览器中使用 remix IDE 来操作智能合约。然而，我所展示的只是一个顺利路径的场景。以太坊的开发社区非常敏捷，正在非常迅速地推出新的变更。当你开始阅读本书时，IDE 可能已经有了细微或重大的修改。一旦你开始编写代码，你就会体会到其中的细微差别，甚至会在后续过程中捕获许多奇怪的 bug。下一节讲的是当你遇到问题或代码 bug 时可以做些什么。

## 在 GitHub 上提交 issue

当你遇到代码 bug 时，在 GitHub 上提交任何 issue 之前，请先查阅以太坊的文档，这些文档可以通过任何搜索引擎免费获取。然后，在以太坊的 Stack Exchange 上提出相同的问题。在大多数情况下，你不会是唯一遇到该 bug 的人。因此，你的问题的解决方案可能已经在网上存在了。最后的手段才是去 GitHub 上提交 issue。GitHub 是一个免费向所有人提供、带有版本控制的代码仓库。大多数与区块链相关的源代码都可以在 GitHub 上找到。我将向你展示我是如何为 LETH 安卓钱包应用提交一个 issue 的，这个应用在第 2 章“Grokking Ethereum”中讨论过：

- **识别问题**：在向聊天窗口上传图片文件时（该聊天窗口使用 whisper 通信协议），没有选项可以提供消息或说明文字，就像我们在 WhatsApp 这类聊天服务中可以做的那样。
- **记录问题**：创建一个免费的 GitHub 账户，并登录 LETH 的 GitHub 仓库页面，如图 3.19 所示。点击“New issue”按钮来记录问题。在描述完问题并附上必要的图片和说明后，点击“Submit new issue”按钮，如图 3.20 所示，就可以生成新 issue，就像我在图 3.19 中生成的那样。你可以在链接 https://github.com/inzhoop-co/LETH/issues/31 中查看该 issue 的详情：

图 3.19：在 GitHub 上创建新 issue

图 3.20：在 GitHub 上描述并提交新 issue

## 在私有区块链上部署智能合约

现在让我们做一些更专业的事情吧。是的，我们仍然不打算花法定货币购买以太币来在区块链上编写代码。相反，我们将做以下事情：

- 1. 为私有区块链编写一个创世区块文件。
- 2. 使用 geth 命令创建私有区块链。
- 3. 将私有区块链与 MIST 浏览器连接。
- 4. 挖矿生成我们自己的以太币作为奖励。
- 5. 使用这些以太币在我们自己的私有区块链上部署智能合约。

需要预先安装的工具如下：

- Windows 7（64 位）（说明针对 Windows 用户，Ubuntu/Mac 用户请参考附录）
- Cygwin setup-x86_64（用于 Windows 7 上的 Bash-Shell）（https://cygwin.com/install.html）
- geth-windows-amd64-1.6.7-ab5646c5（https://geth.ethereum.org/downloads/）
- Mist-win64-0-9-0（https://github.com/ethereum/mist/releases）
- Node.js（node-v6.11.2-x64）（https://nodejs.org/en/download/）
- .NET framework 4.5（https://www.microsoft.com/en-in/download/details.）

我不打算详细介绍安装过程。安装过程非常直接，包括条款和条件页面（我们从不阅读），然后盲目地点击“Next”和“I Agree”按钮，在安装进行的同时，看一些随机的 YouTube 视频或单口相声来娱乐自己。

至于故障排除，Google 和 Stack Exchange 永远是你最好的朋友。*图 3.21*、*图 3.22*、*图 3.23*、*图 3.24*、*图 3.25* 和 *图 3.26* 供你参考，用来检查安装过程中（尤其是 MIST、geth、Cygwin 和 Node.js）你是否在正确的轨道上。唯一的建议是，尽量将所有安装（.NET framework 除外）放在另一个驱动器的一个单独文件夹中，这样它们就不会弄乱你的主程序文件。我的下载和安装都在 `F:Ethereum_Environment path` 中：

![](img/8c91673d39176d2e910ac52c4bc35d8b_122_0.png)

图 3.21：Mist 安装程序包

![](img/8c91673d39176d2e910ac52c4bc35d8b_123_0.png)

图 3.22：MIST 在 RINKEBY 公共区块链上的默认启动（避免这一步）

![](img/8c91673d39176d2e910ac52c4bc35d8b_123_1.png)

## 具体版本

如果你在寻找特定的版本、操作系统或架构，下面你会找到：

- Geth 和工具的所有稳定版和开发版构建
- 非主要处理器架构的归档
- Android 库归档和 iOS XCode 框架

请从下面的列表中选择你想要的平台，并下载你选择的软件包。请注意，MD5 校验和是由我们的二进制托管平台（Azure Blobstore）提供的，以帮助检查下载错误。有关安全保证，请通过附带的 PGP 签名文件验证任何下载（参见 OpenPGP）。

图 3.23：Geth 下载页面

图 3.24：Geth 成功安装页面

图 3.25：Cygwin 安装程序

图 3.26：Node.js 安装向导

## 编写创世区块

创建私有区块链的第一步是创建一个创世区块。这基本上就是提交一个包含以下详细信息的 JavaScript Object Notation（JSON）文件。在本章中，我们只是展示结构，而不是深入探讨区块内容的含义。需要记住的一点是，对于 chainId 参数，要使用一个不是 1 的随机整数，如图 3.27 所示：

```
{
  "coinbase" : "0x0000000000000000000000000000000000000001",
  "difficulty" : "0x20000",
  "extraData" : "",
  "gasLimit" : "0x2fef d8",
  "nonce" : "0x0000000000000042",
  "mixhash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  "timestamp" : "0x00",
  "alloc": {},
  "config": {
    "chainId": 15,
    "homesteadBlock": 0,
    "eip155Block": 0,
    "eip158Block": 0
  }
}
```

图 3.27：genesis.json

