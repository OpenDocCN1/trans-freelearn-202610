
在以下章节中，将讨论一个示例，其中 Raspberry Pi 将作为物联网设备连接到以太坊区块链，并在收到智能合约调用时执行相应操作。

首先，需要对 Raspberry Pi 进行设置。这可以通过使用 NOOBS 完成，它提供了一种安装 Raspbian 或其他操作系统的简便方法。可以从链接 https://www.raspberrypi.org/downloads/noobs/ 下载并安装。或者，也可以仅从链接 https://www.raspberrypi.org/downloads/raspbian/ 安装 Raspbian。另一种选择是使用 https://github.com/debian-pi/raspbian-ua-netinst 上提供的工具，它也可以用来安装一个最小的无图形界面的 Raspbian 操作系统。出于本示例的目的，我们使用 NOOBS 安装 Raspbian，因此后续练习均假设 Raspbian 已安装在 Raspberry Pi 的 SD 存储卡上。

Raspbian 操作系统安装完成后，下一步是为 Raspberry Pi 的 ARM 平台下载合适的 geth 二进制文件。可以在 Raspberry Pi 的 Raspbian 操作系统中的终端窗口运行以下命令来确认平台。命令输出会显示操作系统正在运行的架构。在本例中是 armv7l，因此将下载与 ARM 兼容的 geth 二进制文件。

![](img/62ab880d2302f959d34db648cb0bec5e_445_0.png)

Raspberry Pi 的架构

以下步骤将进行详细说明：

- 1. 下载 geth：请注意，下面的示例中下载的是一个特定版本，但也可以从 https://geth.ethereum.org/downloads/ 下载其他可用版本。
  
      wget https://gethstore.blob.core.windows.net/builds/geth-linux-arm7-1.5.6-2a609af5.tar.gz
  
- 2. 解压并提取到指定目录，执行下面的 tar 命令后，会自动创建一个名为 geth-linux-arm7-1.5.6-2a609af5 的目录：
  
      tar -zxvf geth-linux-arm7-1.5.6-2a609af5.tar

这将创建一个名为 geth-linux-arm7-1.5.6-2a609af5 的目录，并将 geth 二进制文件及相关文件解压到该目录中。可以将 geth 二进制文件复制到 /usr/bin 或 Raspbian 上的适当路径，以便在操作系统的任何位置都能访问它。下载完成后，下一步是创建创世区块。

需要使用之前在第8章“以太坊开发”中创建的同一个创世区块。可以从网络中的其他节点复制创世文件。如下面的截图所示。或者，也可以生成一个全新的创世区块。这一点已在第8章“以太坊开发”中详细讨论过。

![](img/62ab880d2302f959d34db648cb0bec5e_446_0.png)

创世文件

将 genesis.json 文件复制到 Raspberry Pi 后，可以运行以下命令来生成创世区块。必须使用之前生成的完全相同的创世区块，这一点非常重要，否则节点实际上将在不同的网络上运行：

```
$ ./geth init genesis.json
```

这将显示与下面截图类似的输出：

![](img/62ab880d2302f959d34db648cb0bec5e_446_1.png)

初始化创世文件

创建创世区块后，需要向网络中添加对等节点。这可以通过创建一个名为 static-nodes.json 的文件来实现，该文件包含 Raspberry Pi 上的 geth 将要连接以进行同步的对等节点的 enode ID。

```
pi@raspberrypi:~/.ethereum $ cat static-nodes.json
[
"enode://44352ede5b9e792e437c1c0431c1578ce3676a87e1f588434aff1299d30325c233c8d426fc57a25380481c8a36fb3be2787375e932fb4885885f6452f6efa77f@192.168.0.19:30301"
]
```

静态节点配置

此信息可以从 geth JavaScript 控制台中通过运行以下所示命令获取，此命令应在 Raspberry Pi 将要连接的对等节点上运行：

```
> Admin.nodeInfo
```

这将显示与下面截图类似的输出：

```
> admin.nodeInfo
{
enode: "enode://44352ede5b9e792e437c1c0431c1578ce3676a87e1f588434aff1299d30325c233c8d426fc57a25380481c8a36fb3b87375e932fb4885885f6452f6efa77f@[]:30301",
id: "44352ede5b9e792e437c1c0431c1578ce3676a87e1f588434aff1299d30325c233c8d426fc57a25380481c8a36fb3be2787375e9 4885885f6452f6efa77f",
```

geth 节点信息

完成此步骤后，可以按照下面给出的进一步说明，将 Raspberry Pi 连接到私有网络上的其他节点。在示例中，Raspberry Pi 将连接到在第8章“以太坊开发”中创建的网络 ID 786。关键是使用之前创建的相同创世文件以及不同的端口号。不过，不同端口并非严格必需。如果两个节点在私有网络中运行，并且需要从网络外部环境进行访问，那么将使用 DMZ/路由器和端口转发的组合。因此，建议使用不同的 TCP 端口，以便端口转发能够正常工作。下面的命令中的身份切换参数（之前尚未介绍过）可以为节点指定一个标识名称。

## 第一个节点设置

首先，需要使用以下命令在第一个节点上启动 `geth`：

```bash
$ geth --datadir .ethereum/privatenet/ --networkid 786 --maxpeers 5 --rpc --rpcapi web3,eth,debug,personal,net --rpcport 9001 --rpccorsdomain "*" --port 30301 --identity "drequinox"
```

![](img/62ab880d2302f959d34db648cb0bec5e_448_0.png)

`geth` 启动后，应保持其运行，并从 Raspberry Pi 节点启动另一个 `geth` 实例。

## Raspberry Pi 节点设置

在 Raspberry Pi 上，需要运行以下命令来启动 `geth` 并使其与其他节点（本例中仅一个节点）同步。命令如下：

```bash
$ ./geth --networkid 786 --maxpeers 5 --rpc --rpcapi web3,eth,debug,personal,net --rpccorsdomain "*" --port 30302 --identity "raspberry"
```

这应该会产生与下面截图类似的输出。当输出中包含显示 **区块同步已启动** 的行时，表示该节点已成功连接到其对等节点。

![](img/62ab880d2302f959d34db648cb0bec5e_449_0.png)

Raspberry Pi 上的 geth。

可以通过在两个节点的 geth 控制台中运行命令来进一步验证这一点，如下面的截图所示。在 Raspberry Pi 上只需运行以下命令即可附加到 geth：

```
$ geth attach
```

![](img/62ab880d2302f959d34db648cb0bec5e_449_1.png)

在 Raspberry Pi 上运行 geth 控制台 admin peers 命令

类似地，可以在第一个节点上运行以下命令来附加到 geth：

```
$ geth attach ipc:.ethereum/privatenet/geth.ipc
```

一旦控制台可用，就可以运行 admin.peers 来显示其他已连接节点的详细信息，如下面的截图所示：

```
> admin.peers
[
  {
    caps: ["eth/62", "eth/63"],
    id: "98a36ecaff011803d634da45752abd2510f20a62f23427afc3f2b0017bc134833dd5ba400bb195ac6ed59c3b01ca2a3f1460a8a52097a1bbb f967fc84274",
    name: "Geth/raspberry/v1.5.6-stable-2a609af5/linux/go1.7.4",
    network: {
      localAddress: "192.168.0.19:30301",
      remoteAddress: "192.168.0.21:56512"
    },
    protocols: {
      eth: {
        difficulty: 11700366137,
        head: "0x1188f5b8a4900a1d771d333141ea9480d78400bb8e561494ab436519ae64e1e34",
        version: 63
      }
    }
  }
]
```

在另一个对等节点上运行 geth 控制台 admin peers 命令

两个节点都启动并运行后，可以安装进一步的前提条件来准备实验。需要安装 Node.js 和相关 JavaScript 库。下面列出了所需的库和依赖项。首先需要在 Raspberry Pi 的 Raspbian 操作系统上更新 Node.js 和 npm。为此，可以按照以下步骤操作：

- 1. 使用以下命令在 Raspberry Pi 上安装最新版 Node.js：

```
$ curl -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
```

这应该会显示类似下面的输出。输出内容很大，因此下面的截图仅显示输出的顶部部分：

![](img/62ab880d2302f959d34db648cb0bec5e_450_0.png)

Node.js 安装

- 2. 通过 apt-get 运行更新：

```
$ sudo apt-get install nodejs
```

