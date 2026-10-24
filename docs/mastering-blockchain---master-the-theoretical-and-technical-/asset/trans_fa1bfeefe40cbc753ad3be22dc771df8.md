
而且，当键入命令时，可以通过按两次 tab 键自动补全。如果按两次 tab，还会显示可用方法的列表。如下面的截图所示：

![](img/62ab880d2302f959d34db648cb0bec5e_304_0.png)

除了前面提到的命令外，为了获取可用方法的列表，在键入任何命令后，输入 ';'（分号）。下一个截图显示了 net 可用的所有方法列表：

![](img/62ab880d2302f959d34db648cb0bec5e_304_1.png)

还有一些其他命令可用于查询私有网络。以下是一些示例：

- 获取当前 gas 价格：
  
  ```text
  > eth.gasPrice
  20000000000
  ```

- 获取最新区块号：
  
  ```text
  > eth.blockNumber
  587
  ```

- 获取编译器列表。注意，如果没有输出，则意味着没有安装 solidity 编译器；solidity 编译器的安装细节将在本章后面提供：

```
> web3.eth.getCompilers()
["Solidity"]
```

## 在私有网络上运行 Mist

可以通过发出以下命令在私有网络上运行 Mist。在安装 /opt/Ethereum 后，该二进制文件通常位于主文件夹中：

```
$ ./Ethereum\ Wallet --rpc ~/.ethereum/privatenet/geth.ipc
```

这将允许连接到正在运行的私有网络 geth 会话，并提供所有功能，例如钱包、账户管理以及通过 Mist 在私有网络上部署合约。

![](img/62ab880d2302f959d34db648cb0bec5e_306_0.png)

以太坊启动后，将显示此处所示的界面，明确指出它正在以**私有网络**模式运行。

![](img/62ab880d2302f959d34db648cb0bec5e_307_0.png)

私有网络上的 Mist

Mist 也可以通过 RPC 通过网络运行。如果 `geth` 运行在一个节点上，而 Mist 运行在另一个节点上，这很有用。可以通过使用此处所示的标志运行 Mist 来实现：

```text
--rpc http://127.0.0.1:8545
```

## 使用 Mist 部署合约

使用 Mist 部署新合约非常容易。Mist 提供了一个界面，可以在其中用 solidity 编写合约，然后部署到网络上。

在练习中，将使用一个简单的合约，该合约可以对输入参数执行各种简单的算术计算。下面显示了如何使用 Mist 部署此合约的步骤。由于尚未介绍 solidity，这里的目标是让用户体验合约部署和交互过程。有关编码和 solidity 的更多信息将在本章后面提供，之后将更容易理解所显示的代码。已经熟悉 JavaScript 或任何其他类似语言的人会发现这些代码几乎不言自明。

示例合约源代码如下所示：

```
pragma solidity ^0.4.0;
contract SimpleContract2
{
    uint x;
    uint z;
    function addition(uint x) returns (uint y)
    {
        z=x+5;
        y=z;
    }
    function difference(uint x) returns (uint y)
    {
        z=x-5;
        y=z;
    }
    function division(uint x) returns (uint y)
    {
        z=x/5;
        y=z;
    }

    function currValue() constant returns (uint)
    {
        return z;
    }
}
```

这段代码可以简单地复制到 Mist 的合约部分，如下所示。在左侧，可以复制源代码；一旦验证且未检测到语法错误，部署合约的选项将出现在右侧的下拉菜单中，其中显示“选择要部署的合约”。只需选择合约，然后按屏幕底部的“部署”按钮。

![](img/62ab880d2302f959d34db648cb0bec5e_308_0.png)

Mist 会要求输入账户密码，并显示一个类似于以下截图的窗口：

![](img/62ab880d2302f959d34db648cb0bec5e_309_0.png)

输入密码，然后点击**发送交易**以部署合约。

一旦部署并成功挖矿，它将出现在 Mist 的交易列表中，如下所示：

![](img/62ab880d2302f959d34db648cb0bec5e_310_0.png)

在 Mist 中创建后的交易列表

合约可用后，可以通过执行交易并调用 Mist 中的可用函数与之交互。

![](img/62ab880d2302f959d34db648cb0bec5e_310_1.png)

使用 Mist 中的读取和写入选项与合约交互

在上面的截图中，有**从合约读取**和**向合约写入**选项。此外，合约公开的函数可以在右侧看到。选择所需函数后，为函数输入适当的值并选择账户（执行来源）；按执行以执行交易，这将导致调用合约的所选函数。

此过程如下面的截图所示：

![](img/62ab880d2302f959d34db648cb0bec5e_311_0.png)

如截图所示，为账户输入适当的密码，然后按**发送交易**将交易发送到合约。

## 开发工具和客户端

有许多可用于以太坊开发的工具。下图显示了以太坊的各种开发工具、客户端、IDE 和开发框架的分类：

![](img/62ab880d2302f959d34db648cb0bec5e_312_0.png)

在本章中，主要重点将放在 geth、浏览器 Solidity、Solidity、solc 和 truffle 上。其余元素将简要讨论。

## 语言

合约可以使用多种语言进行编程。有四种语言可用于编写合约：

- **Mutan**：这是一种 Go 风格的语言，在 2015 年初被弃用，不再使用。
- **LLL**：这是一种类似 Lisp 的语言，因此得名 LLL。它也不再使用了。
- **Serpent**：这是一种简单而干净的类似 Python 的语言。它被积极用于合约开发。
- **Solidity**：这种语言现在几乎已成为以太坊合约编写的标准。这种语言是本章的重点，将在后面的章节中详细讨论。

## 编译器

编译器用于将高级合约源代码转换为以太坊执行环境可以理解的格式。Solidity 编译器是最常用的编译器，此处将进行讨论。

## Solc

Solidity 编译器将高级 Solidity 语言转换为**以太坊虚拟机（EVM）**字节码，以便 EVM 可以在区块链上执行。

在 Linux Ubuntu 操作系统上，可以使用以下命令安装 Solidity 编译器：

```
$ sudo apt-get install solc
```

如果尚未安装 PPA，可以通过运行以下命令安装：

```
sudo add-apt-repository ppa:ethereum/ethereum
sudo apt-get update
```

为了验证 Solidity 编译器的现有版本并确认其已安装，可以使用以下命令：

```
$ solc --version
solc, the solidity compiler commandline interface
Version: 0.4.6+commit.2dabdf0.Linux.g++
```

Solc 支持多种功能。下面显示了一些示例：

- 以二进制格式显示合约。

![](img/62ab880d2302f959d34db648cb0bec5e_313_0.png)

Solidity 编译器二进制输出

- 估算 gas：

```
imran@drequinox-OP7010:~$ solc --gas contract1.sol
====== SimpleContract ======
Gas estimation:
construction:
  97 + 54600 = 54697
external:
  division(uint256):  230
  addition(uint256):  231
  difference(uint256): 253
```

# 内部：

请注意，contract1.sol 仅作为示例显示；该文件可以包含任何智能合约 Solidity 代码。此处未显示文件的代码。

Solc 在 geth 控制台中由 web3 内部使用，以便编译合约。语法如下所示，contractsourcecode 是 Solidity 源代码：

```
web3.eth.compile.solidity(contractsourcecode)
```

这将在本章后面的合约开发介绍中详细讨论。

## 集成开发环境（IDE）

有多种可用于 Solidity 开发的 IDE。大多数 IDE 可在线获得，并通过 Web 界面呈现。浏览器 Solidity 最常用于较小的合约，这里将进行讨论。

## 浏览器 Solidity

浏览器 Solidity 是使用 Solidity 开发和测试合约的基于 Web 的环境。它不运行在实时区块链上；实际上，它是一个模拟环境，可以在其中部署、测试和调试合约。可访问 https://ethereum.github.io/browser-solidity。示例界面如下所示：

![](img/62ab880d2302f959d34db648cb0bec5e_314_0.png)

浏览器 Solidity

