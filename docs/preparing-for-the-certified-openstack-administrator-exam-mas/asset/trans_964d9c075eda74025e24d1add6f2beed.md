
2006年，AWS 推出了其首批服务之一：Amazon Simple Storage Service，简称 S3。Amazon 将 S3 宣传为“高度可扩展、可靠且低延迟的数据存储基础设施，成本极低。” 但这意味着什么？嗯，让我们分解几点，说明当时 S3 为何如此受追捧：

- 与当时流行的托管服务提供商不同，用户可以根据实际使用量按需付费（前提是已存档信用卡）。这很像电费或公用事业账单。
- 用户可通过 Web API 存储文件，从而将其编程上传和下载集成到 Web 应用代码中（想想相册 Web 应用：超级强大！）。
- 价格非常便宜！用户每月只需几美元即可存储数千个对象。
- 所有对象都写入三个不同的故障域，因此即使 AWS 数据中心发生服务器故障也能得到保障。

现在，世界上一些最受欢迎的应用都在使用 S3。Dropbox 是一个使用 S3 存储用户文件的前端，而 Pinterest 和 Tumblr 使用 S3 来托管图像！

2009 年 8 月，Rackspace 受 Amazon S3 启发，开始着手开发自己的对象存储服务 Cloud Files（至今仍可用）。该项目演变成我们所知的 OpenStack Swift，这是一个具有 RESTful API 的开源可扩展冗余存储系统。与 S3 类似，Swift 为用户提供一种使存储看起来无限的对象存储服务。它还提供许多很酷的额外功能，例如静态网站托管、对象版本控制、访问控制列表（ACL）和对象过期。

在幕后，OpenStack Swift 将对象写入数据中心服务器中分布的多个磁盘驱动器。Swift 的软件（像其他 OpenStack 服务一样用 Python 编写）负责确保集群中数据的复制和完整性。如果服务器或硬盘发生故障，Swift 会从其他活动节点复制其内容到集群中的新位置。存储集群只需添加新服务器即可水平扩展。由于 OpenStack 使用软件逻辑确保数据在不同设备间的复制和分布，因此可以使用廉价的通用硬盘和服务器。

![](img/da864e0670c65edf6f5ab861ef9906b3_270_0.png)

图 8.1：Swift 允许用户通过 RESTful API 上传和下载文件

## Swift 架构

Swift 由十四项主要守护进程组成，如图 8.2 所示：

- **swift-proxy-server**：Swift 的 API 和主要网关，负责将 Swift 架构的其余部分连接起来。必须与 swift-proxy-server 交互才能创建容器、向这些容器上传对象、设置 ACL 以及启用特殊功能，例如对象版本控制或启用静态网站。对于每个请求，它都会在环中查找账户、容器或对象的位置并相应地路由请求。
- **swift-object-server**：负责检索和删除存储在 Swift 集群本地驱动器上的对象。对象以文件系统上的二进制文件形式存储，元数据存储在文件的扩展属性（xattrs）中。这要求对象服务器所选的底层文件系统支持文件上的 xattrs。每个对象都使用基于对象名称哈希和操作时间戳派生的路径进行存储。
- **swift-object-auditor**：遍历本地对象系统检查对象的完整性。如果发现损坏，文件将被隔离，复制将从另一个副本替换损坏的文件。
- **swift-object-expirer**：提供对象的计划删除。
- **swift-object-reconstructor**：擦除编码对象的重建守护进程。
- **swift-object-replicator**：在网络中断或驱动器故障等临时错误条件下保持系统处于一致状态。复制过程比较本地数据与每个远程副本，以确保它们都包含最新版本。对象复制使用哈希列表快速比较每个分区的子部分。
- **swift-container-server**：处理特定容器内对象的列出。列表以 SQLite 数据库文件形式存储，并在集群中进行复制（类似于对象）。
- **swift-container-auditor**：遍历本地容器系统，检查 SQLite 数据库文件的完整性。如果发现损坏，文件将被隔离，复制将从另一个副本替换损坏的文件。
- **swift-container-replicator**：类似于 swift-object-replicator，只是针对容器 SQLite 数据库文件。
- **swift-container-updater**：更新账户数据库中的容器信息。它会遍历系统中的容器路径，查找容器 SQLite 文件，并在需要时向账户服务器发送更新。
- **swift-container-sync**：负责允许通过后台同步将一个容器的内容镜像到另一个容器。
- **swift-account-server**：处理特定账户内容器的列出。列表以 sqlite 数据库文件形式存储，并在集群中进行复制（类似于对象）。
- **swift-account-auditor**：与 swift-container-auditor 相同，只是针对账户 sqlite 数据库文件。
- **swift-account-replicator**：类似于 swift-container-replicator，只是针对账户 sqlite 数据库文件。
- **swift-account-reaper**：删除已被 OpenStack 管理员标记为删除的账户。

![](img/da864e0670c65edf6f5ab861ef9906b3_271_0.png)

图 8.2：Swift 架构的高级视图

## Swift 概念

- **Account**：Swift 使用术语“account”来指代 OpenStack 项目。
  记住：在 OpenStack 世界中，accounts、tenants 和 projects 含义相同！
- **Container**：用于静态文件（也称为 **objects**）的“存储桶”。用户必须创建一个容器才能将对象上传到 Swift 账户。所有容器都由用户创建时所作用域的账户（或项目）拥有。默认情况下，容器中上传的所有对象都是私有的。
- **Objects**：对象是用户上传到容器中的文件。这些通常是静态文件，例如图片、电影、文档或日志。
- **Object expiration**：可以设置对象在特定时间过期。一旦对象过期，它将无法再访问并从 Swift 集群中删除。对象过期的一个很好的用例是临时注册码或密钥，其有效期很短。
- **ACL (access control list)**：默认情况下，Swift 容器是私有的，其他用户或项目无法访问。默认情况下，具有 admin 或 swiftoperator 角色的用户可以在容器级别设置 ACL 并支持读取和写入访问列表。
- **Static website hosting**：而不是使用传统的 Web 服务器如 Apache 或 Nginx 来托管网站，Swift 可以托管静态网站文件，例如 HTML、CSS 和客户端 JavaScript。通过将容器的 web-index 头设置为容器的公共 URL，它将导航到指定的文件，例如 `index.html`。然后可以更新 DNS 以指向可公开访问的 Swift 容器的 URL。
- **Object versioning**：版本控制允许用户上传特定文件的多个版本。用户只需创建一个用于存储版本的备用容器。当用户向 Swift 容器上传同名文件时，旧版本会被发布到备用容器。可以轻松检索并恢复到以前的版本。如果向对象发送 `DELETE` 请求，最新版本将被删除，并恢复之前的版本。

## 考试目标

让我们看看 Swift 的考试目标。记住，Certified OpenStack Administrator 考试期望你从管理员的角度理解如何使用 OpenStack，而不是基础设施或 DevOps 工程师。你不需要过于关注后端 Swift 配置。这是一个复杂的话题，你可以在 Packt Publishing 出版的《OpenStack Object Storage (Swift) Essentials》一书中了解更多。

下图分解了你在 COA 考试中应负责了解的各种 Swift 相关目标。如你所见，其中只有一些任务可以在 Horizon 仪表板和 CLI 上完成。当涉及到 ACL 和对象过期时，你需要掌握本章中所示的 CLI 命令。

![](img/da864e0670c65edf6f5ab861ef9906b3_273_0.png)

图 8.3：Swift ACL 和对象过期只能通过 CLI 完成

要在 Horizon 仪表板上向 Swift 上传对象，文件必须可通过本地文件系统访问。如果你想上传大于 5 GB 的文件，必须使用 CLI。

### 考试目标 - 管理容器和对象

容器在 Amazon S3 世界中类似于存储桶。容器由项目拥有（在 Swift 世界中也称为账户）。就权限而言，Swift 容器与传统的 OpenStack 资源略有不同。默认情况下，用户必须具有 admin 或 swiftoperator 角色才能创建、删除和修改容器。我们将在下一节学习如何设置 ACL。
