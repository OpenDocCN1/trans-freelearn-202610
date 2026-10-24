

有一些非标准但使用广泛的节点被称为矿池协议服务器。这些节点使用替代协议，例如 Stratum 协议。一些节点仅执行挖矿功能，被称为矿工节点。仅计算哈希的节点使用 Stratum 协议将解决方案提交给矿池。可以运行一个 SPV 客户端，该客户端运行钱包和网络路由功能而无需区块链。

互联网上大多数协议都是基于行的，这意味着每一行都以回车和换行符 `\r\n` 字符分隔。Stratum 也是一个基于行的协议，它使用纯 TCP 套接字和人类可读的 JSON-RPC 来操作并在节点之间通信。

比特币网络由其不同的魔数标识。列表如下所示：

| Network | Magic value | Hex |
|---|---|---|
| main | 0xD9B4BEF9 | F9 BE B4 D9 |
| testnet3 | 0x0709110B | 0B 11 09 07 |

魔数用于指示消息来源网络。

一个全节点执行四个功能：钱包、矿工、区块链和网络路由节点。

当比特币核心节点启动时，首先，它会启动所有对等节点的发现。这是通过查询硬编码在比特币核心客户端中的 DNS 种子来实现的，并由比特币社区成员维护。此查找返回一些 DNS A 记录。比特币协议默认在主网络上使用 TCP 端口 8333，在 testnet 上使用 TCP 18333。

```
vSeeds.push_back(CDNSSeedData("bitcoin.sipa.be", "seed.bitcoin.sipa.be", true)); // Pieter Wuille
vSeeds.push_back(CDNSSeedData("bluematt.me", "dnsseed.bluematt.me")); // Matt Corallo
vSeeds.push_back(CDNSSeedData("dashjr.org", "dnsseed.bitcoin.dashjr.org")); // Luke Dashjr
vSeeds.push_back(CDNSSeedData("bitcoinstats.com", "seed.bitcoinstats.com")); // Christian Decker
vSeeds.push_back(CDNSSeedData("xf2.org", "bitseed.xf2.org")); // Jeff Garzik
vSeeds.push_back(CDNSSeedData("bitcoin.jonasschnelli.ch", "seed.bitcoin.jonasschnelli.ch", true)); // Jonas Schnelli
```

首先，客户端发送一个协议消息 Version，其中包含各种字段，例如版本、服务、时间戳、网络地址、nonce 和一些其他字段。远程节点用自己的 version 消息进行响应，然后双方节点之间进行 verack 消息交换，表明连接已建立。

之后，Getaddr 和 addr 消息被交换以找到客户端不知道的对等节点。与此同时，任一节点都可以发送 ping 消息来查看连接是否仍然存活。

现在可以开始区块下载。如果节点已经完全同步了所有区块，那么它使用 Inv 协议消息监听新区块；否则，它首先检查是否对 inv 消息有响应并且已有库存。如果是，则使用 Getdata 协议消息请求区块；如果不是，则使用 GetBlocks 消息请求库存。此方法一直使用到 0.9.3 版本。

![](img/62ab880d2302f959d34db648cb0bec5e_165_0.png)

协议可视化节点发现

初始区块下载可以根据比特币核心客户端的版本使用 blocks first 或 headers-first 方法来同步区块。blocks-first 方法非常慢，并且自 0.10.0 版本以来已被停用。

自 0.10.0 版本以来，引入了名为 headers-first 的初始区块下载方法。这带来了重大的性能改进，区块链同步曾经需要几天才能完成，现在只需要几个小时。核心思想是新节点首先向对等节点请求区块头并验证它们。一旦完成，区块将并行从所有可用对等节点请求，因为完整的链蓝图已以区块头链的形式下载。

在此方法中，当客户端启动时，它会检查区块链是否已完全同步；如果标头链已同步；如果不是，即客户端首次启动时，它使用 getHeaders 消息从其他对等节点请求标头。如果区块链已完全同步，它会通过 Inv 消息监听新区块，如果它已经拥有完全同步的标头链，则使用 Getdata 协议消息请求区块。节点还会检查标头链是否比区块拥有更多标头，然后通过发出 Getdata 协议消息请求区块。

![](img/62ab880d2302f959d34db648cb0bec5e_166_0.png)

Bitcoin Core Client >= 0.10.0 header and block synchronization. IBD = Initial block download and sync node means the node from where the blocks are being requested from

Getblockchaininfo 和 getpeerinfo RPC 已更新为包含新的功能以适应此更改。RPC getchaintips 用于列出区块链的所有已知分支。这也包括仅标头的区块。Getblockchaininfo 用于提供有关区块链当前状态的信息。Getpeerinfo 用于列出对等节点之间共同拥有的区块数和标头数。

Wireshark 还可以用于可视化对等节点之间的消息交换，并且可以作为学习比特币协议的宝贵工具。这里显示了一个示例。这是一个基本示例，显示了 version、verack、getaddr、ping、addr 和 inv 消息。

在详细信息中，可以看到诸如数据包类型、命令名称和协议消息结果等有价值的信息。

![](img/62ab880d2302f959d34db648cb0bec5e_167_0.png)

Wireshark 中的一个示例区块消息

这里显示了一个协议图，展示了两个对等节点之间的数据流。这可以帮助你理解节点启动时使用什么类型的消息。

在以下示例中，使用比特币解码器来分析流量并识别比特币协议命令。

可以看出 version、getaddr 和 getdata 等消息的交换，以及描述消息名称的相应注释。此练习对于学习比特币非常有用，建议在比特币测试网上进行实验，在那里可以发送各种消息和交易到网络，然后由 Wireshark 分析。

| Time | 192.168.0.13 136.243.139.96 | Comment |
|---|---|---|
| 97.734135000 | version | Bitcoin: version |
| 98.025045000 | verack | Bitcoin: verack |
| 98.025177000 | getaddr, ping, addr | Bitcoin: getaddr, ping, addr |
| 98.025468000 | getheaders... | Bitcoin: getheaders, [unknown command], [unknown command], [unknown command], headers |
| 98.160419000 | [TCP Retran... | Bitcoin: [TCP Retransmission], getheaders, [unknown command], [unknown command], [unknown command] |
| 98.598399000 | getdata | Bitcoin: getdata |
| 144.343544000 | inv | Bitcoin: inv |
| 176.152240000 | getdata | Bitcoin: getdata |
| 179.493755000 | getdata | Bitcoin: getdata |
| 218.101646000 | ping | Bitcoin: ping |
| 218.192004000 | [unknown co... | Bitcoin: [unknown command] |
| 218.444431000 | [TCP Retran... | Bitcoin: [TCP Retransmission], [unknown command] |
| 336.234936000 | getdata | Bitcoin: getdata |
| 337.843423000 | [unknown co... | Bitcoin: [unknown command] |
| 338.143885000 | ping | Bitcoin: ping |
| 448.764093000 | getdata | Bitcoin: getdata |
| 457.894823000 | [unknown co... | Bitcoin: [unknown command] |
| 458.195265000 | ping | Bitcoin: ping |
| 578.011774000 | [unknown co... | Bitcoin: [unknown command] |
| 578.212044000 | ping | Bitcoin: ping |
| 585.587671000 | inv | Bitcoin: inv |
| 647.169633000 | inv | Bitcoin: inv |
| 671.962545000 | getdata | Bitcoin: getdata |
| 698.037067000 | [unknown co... | Bitcoin: [unknown command] |
| 698.237350000 | ping | Bitcoin: ping |
| 701.563581000 | inv | Bitcoin: inv |
| 701.986269000 | inv | Bitcoin: inv |
| 705.022173000 | inv | Bitcoin: inv |
| 812.115878000 | inv | Bitcoin: inv |
| 818.198570000 | [unknown co... | Bitcoin: [unknown command] |
| 818.298733000 | ping | Bitcoin: ping |

总共有 27 种协议消息，但随着协议的发展，它们可能会随着时间推移而增加。最常用的协议消息及其解释如下所示：

- **Version:** 这是节点发送到网络的第一条消息，广告其版本和区块计数。远程节点然后用相同的信息进行回复，然后建立连接。
- **Verack:** 这是 version 消息的响应，接受连接请求。
- **Inv:** 这用于节点广告其对区块和交易的了解。

