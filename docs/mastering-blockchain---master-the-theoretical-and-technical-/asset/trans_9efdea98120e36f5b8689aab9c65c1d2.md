

一旦达成共识，智能合约执行就可以开始，并按以下步骤进行：

1. 首先，验证消息的签名。
2. Pact 智能合约层接管。
3. Pact 代码被编译。
4. 交易被发起并执行智能合约中嵌入的任何业务逻辑。如果出现任何失败，会立即启动回滚，将状态恢复到执行开始之前的状态。
5. 最后，交易完成，相关日志得到更新。

Pact 已由 Kadena 开源，可在 `http://kadena.io/pact/downloads.html` 下载。这可以作为独立二进制文件下载，提供 Pact 语言的 REPL。下面给出了一个示例，其中通过在 Linux 控制台中发出命令 `./pact` 运行 Pact：

![](img/62ab880d2302f959d34db648cb0bec5e_411_0.png)

Pact REPL，显示示例命令和错误输出

Pact 中的智能合约通常由三个部分组成：keysets、modules 和 tables。首先，**keyset** 定义了表和模块的相关授权方案。其次，**module** 定义了智能合约代码，其中包含以 **functions** 和 **Pacts** 形式表示的业务逻辑。模块中的 Pacts 由多个步骤组成，并按顺序执行。

Pact 可在多种执行模式下使用。这些模式包括 **contract definition**、**transaction execution** 和 **querying**。合约定义模式允许通过单笔交易消息在区块链上创建合约。交易执行模式涉及执行代表业务逻辑的智能合约代码模块。查询仅用于探测合约数据，并出于性能原因在节点上本地执行。

Pact 使用类似 LISP 的语法，并在代码中精确表示将在区块链上执行的内容，因为它以人类可读的格式存储在区块链上。这与以太坊的 EVM 形成对比，后者编译为字节码执行，这使得难以验证区块链上正在执行的是什么代码。此外，它是不完备的图灵机，支持不可变变量，并且不允许空值，这提高了交易代码执行的整体安全性。

本章不可能涵盖 Pact 的完整语法和函数，然而，下面展示了一个小示例，说明了用 Pact 编写的智能合约的总体结构。该示例展示了一个简单的 addition 模块，定义了一个名为 `addition` 的函数，该函数接受三个参数。执行代码时，它将三个值相加并显示结果。

以下示例使用在线 Pact 编译器开发，可在 `http://kadena.io/try-pact/` 获取：

```lisp
(begin-tx) ;testTransaction ;Begin transaction with optional NAME
;Set transaction data in JSON format or pact types
(env-data { "keyset": { "keys": ["admin"], "pred": "keys-any" } })
;Define keyset as NAME with KEYSET
(define-keyset admin-keyset (read-keyset "keyset"))
;Set transaction signature KEYS.
(env-keys "admin")
;define module using syntax (module NAME KEYSET [DOCSTRING] DEFS...)
(module additionModule admin-keyset
  ;define function that takes three arguments x y z
  (defun addition (x y z) (+ x (+ y z)))
)
(commit-tx) ;Commit transaction.
;use the function addition
(use additionModule)
;run the function addition
(format "Result : {}" (addition 100 200 300))
```

Sample Pact code

当运行代码时，它会产生如下所示的输出：

```
Begin Tx 1
testTransaction
Setting transaction data
Keyset defined
Setting transaction keys
Loaded module "additionModule"
Commit Tx 1
Using "additionModule"
Result : 600
```

Output of the code

如前例所示，执行输出与代码布局和结构完全匹配，这允许更大的透明度并限制恶意代码执行的可能性。

Kadena 是一种新型的私有区块链，引入了 **pervasive determinism** 的新概念，其中除了标准的基于公钥/私钥的数据来源安全之外，还提供了一个完全确定性的共识的额外层。它在区块链的所有层提供加密安全，包括交易和共识层。

![](img/62ab880d2302f959d34db648cb0bec5e_413_0.png)

Pact 的相关文档和源代码可在以下位置找到：
https://github.com/kadena-io/pact.

## Ripple

Ripple 于 2012 年推出，是一种货币兑换和实时全额结算系统。在 Ripple 中，支付无需等待即可结算，而与传统结算网络不同，后者在结算时可能需要数天时间。它有一种原生货币称为 **Ripples (XRP)**。它还支持非 XRP 支付。该系统被认为类似于一种古老的传统汇款机制，称为 *Hawala*。该系统通过使用代理来运作，代理从发送方收取资金和密码，然后联系收款方的代理并指示他们向能提供密码的人发放资金。收款人随后联系当地代理，告知密码并提取资金。代理在 Ripple 中的类比是网关。这只是一个非常简单的类比，实际协议相当复杂，但在原则上是相同的。

Ripple 网络由可根据其类型执行不同功能的各种节点组成。首先，**user nodes**：这些用于支付交易中，可以付款或收款。其次，**validator nodes**：这些参与共识机制。每个服务器维护一组唯一节点，在达成共识时需要查询这些节点。**unique node List (UNL)** 中的节点被参与共识机制的服务器所信任，并且只会接受来自此唯一节点列表的投票。Ripple 有时不被认为真正去中心化，因为网络运营商和监管机构参与其中。然而，由于任何人都可以通过运行验证节点成为网络的一部分，因此可以认为它是去中心化的。此外，共识过程也是去中心化的，因为对账本提出的任何更改都必须通过超级多数投票方案来决定。然而，这在研究人员和爱好者中是一个热门话题，对每种观点都有支持和反对的论据。

- **Open ledger**：这是一个尚未验证的账本，尚未就其状态达成共识。每个节点都有自己的开放账本，其中包含建议的交易。
- **Unique node list**：这是一组唯一受信任节点的列表，验证服务器用它来寻求投票和随后的共识。
- **Proposer**：顾名思义，该组件提出新的交易以纳入共识过程。它通常是节点的一个子集（上面定义的 UNL），可以向验证服务器提出交易。

### Transactions

交易由网络用户创建以更新账本。交易预期是经过数字签名且有效的，以便被视为共识过程的候选者。每笔交易花费少量 XRP，这作为防止因垃圾邮件导致的拒绝服务攻击的保护机制。Ripple 网络中有不同类型的交易。Ripple 交易数据结构中的一个字段 `TransactionType` 用于表示交易的类型。交易通过四步流程执行。首先，交易被准备，其中根据标准创建未签名的交易。第二步是签名，其中交易被数字签名以授权它。之后，实际提交到网络通过连接的服务器进行。最后，进行验证以确保交易成功验证。

大致上，交易可以分为三类，即与支付相关、与订单相关以及与账户和安全相关。所有这些类型在以下部分中描述。

#### Payments related

此类中有几个字段会导致某些操作。所有这些字段描述如下：

1. **Payment**：此交易使用最广泛，允许一个用户向另一个用户发送资金。
2. **PaymentChannelClaim**：这用于从支付渠道申领 **Ripples (XRP)**。支付渠道是一种允许各方之间进行重复且单向支付的机制。这也可以用于设置支付渠道的到期时间。
3. **PaymentChannelCreate**：此交易创建新的支付渠道并以 *drops* 形式向其中添加 XRP。一个 drop 等于 0.000001 个 XRP。

