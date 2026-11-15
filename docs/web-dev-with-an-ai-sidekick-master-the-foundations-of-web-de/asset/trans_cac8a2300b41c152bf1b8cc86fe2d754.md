

在下一节中，我们将使用 `DigitalOcean` 作为我们的托管服务商。它提供了一种简单直接的方式来创建和管理虚拟服务器，拥有可预测的定价和完善的文档。更重要的是，它让我们能够专注于理解部署过程，而不是纠结于基础设施的复杂性。

## 为什么 `DigitalOcean` 适合 `TallyApp`

我选择使用 `DigitalOcean` 作为主机提供商，因为它处于一个有用的中间地带：既不像过度抽象的平台那样隐藏一切，也不像超大规模云那样用海量选项和定价复杂性淹没初学者。

对于像 `TallyApp` 这样的项目，它可以说很利于学习，因为它迫使你理解部署期间发生了什么，又不会让你淹没在企业级运维的担忧中。

最大的优势是简单性。你可以在几分钟内从零开始拥有一台公网服务器，使用的是 `Droplet`，它本质上就是一台虚拟机。这与 `Django` 传统上使用 `Gunicorn` 和 `Nginx` 等工具进行部署的方式完美契合。

文档完善且对新手友好。`DigitalOcean` 花费多年时间专门为常见技术栈（如 `Django`）构建教程，包括针对 `Ubuntu`、`Gunicorn` 和 `Nginx` 的分步指南。你可以通过以下链接阅读 `DigitalOcean` 文档教程：[https://www.digitalocean.com/community/tutorials](https://www.digitalocean.com/community/tutorials)。

`DigitalOcean` VPS 还赋予你完全的控制权。你将学习 Web 服务器、反向代理和应用服务器是如何组合在一起的，而不是依赖一个黑盒平台。

### 你应该了解的缺点

简单性的代价是你需要自己负责更多事情。

你在管理一台服务器。这意味着：

*   应用安全更新
*   配置防火墙
*   管理进程和服务
*   手动处理扩缩容

没有自动扩缩容。如果你的网站突然变得流行或遭受攻击，单个 `Droplet` 可能会不堪重负。

你仍需自负应用安全责任，因为它不是全托管的。与 `Heroku` 或 `Vercel` 等平台不同，`DigitalOcean` 不会自动处理许多应用层面的防护。你需要自行配置。

备份和监控是可选的附加组件。除非你启用它们，否则它们不会默认完全集成。

### 它适合像 `TallyApp` 这样的公开演示吗？

适合，但有一个条件：你必须在它前面加一层薄薄的保护。单个低成本 `Droplet` 完全有能力运行流量轻度到中等的 `Django` 应用。对于图书配套网站来说，这通常绰绰有余。

然而，直接将裸露的服务器暴露在互联网上并不理想。如果没有额外保护，它将迅速收到：

*   扫描漏洞的机器人流量
*   登录暴力破解尝试
*   爬虫和抓取工具
*   偶发的流量高峰

这就是将 `DigitalOcean` 与代理层结合变得重要的原因。

### `DigitalOcean` 提供的防护

`DigitalOcean` 为你提供基础设施级别的安全，但不提供完整的应用防护。

包含的防护：

*   通过虚拟专用网络实现的基本网络隔离
*   云防火墙，按端口和 IP 限制入站流量
*   安全外壳（`SSH`）密钥认证而非密码
*   数据中心级别的物理安全
*   基础设施级别的 `DDoS` 防护（有限且不可深度配置）

这些是有用的服务器级防护，但它们无法防范：

*   应用层攻击（例如登录滥用）
*   `XSS` 或 `CSRF` 问题
*   `Django` 配置错误
*   针对你端点的机器人流量

一些有用的功能可用，但需要启用或额外付费，如 `表 15.2` 所示：

| **功能** | **描述** |
| --- | --- |
| 备份 | 自动备份通常费用约为 `Droplet` 价格的 20%。 |
| 监控和警报 | 提供基础监控，但你需要自行配置警报。 |
| 负载均衡器 | 起价约为每月 12 美元。如果你扩展到单服务器之外，这很有用。 |
| 托管数据库 | 比自托管 `PostgreSQL` 更贵，但能降低运维风险。 |

`表 15.2`：可启用的 `DigitalOcean` 功能

## 缺失的一环：边缘防护

`DigitalOcean` 给你提供了服务器，但它不会在每个坏请求到达你的应用之前将其过滤掉。这就是在你的 `Droplet` 前面添加 `Cloudflare`（或类似服务）能增强防护的原因。

即使使用免费的 `Cloudflare` 计划，你也能获得：

*   面向 `CSS` 样式表和图片等静态内容的 `CDN` 缓存
*   基础 `DDoS` 防护
*   机器人过滤
*   `TLS` 终止（在边缘处理 `HTTPS`）

以低成本，你可以添加：

*   速率限制（用于登录和表单）
*   Web 应用防火墙（`WAF`）规则

当与像 `Cloudflare` 这样简单的边缘服务以及良好的 `Django` 配置相结合时，`DigitalOcean` 加 `Cloudflare` 加正确的 `Django` 设置，足以应对小型公开 `TallyApp` 部署。

## 分步部署

你将从理解部署中获益良多，而非仅仅走通往可用服务器的最短路径。

我们将标准化以下目标技术栈：用于应用服务器的小型 `Droplet`、`Ubuntu` `LTS`、为追求简单而用的 `SQLite` 数据库、作为反向代理的 `Nginx`、用于 `Django` 的 `Gunicorn`、`Uncomplicated Firewall (UFW)` 以及可选的备份。这适用于流量适中的公开演示应用。

### 创建 `DigitalOcean` `Droplet`

现在我们可以创建一个 `Droplet` 来托管我们的网站项目：

1.  在以下链接注册 `DigitalOcean` 账号：[https://cloud.digitalocean.com/](https://cloud.digitalocean.com/)
2.  在你账号的 `Projects` 页面，你将已有一个默认项目，通常命名为 `first-project`。我们可以保持原样。
3.  点击 `Create`，然后点击 `Droplets`，如 `图 15.3` 所示：

![](Images/B34553_15_03.png)

`图 15.3`：创建 `Droplet`

4.  选择一个区域，要么靠近你大多数预期访客，要么靠近你。我选择了 `New York`。
5.  为你的操作系统选择一个镜像。我选择了 `Ubuntu 24.04 (LTS) x64`。
6.  选择 `Droplet` 规格。我选择了：
    *   Basic（共享 CPU）
    *   Regular SSD
    *   每月 12 美元
    *   2 GB / 1 CPU
    *   50 GB SSD 磁盘
    *   2 TB 流量
7.  可选，启用自动备份计划。我选择了 `Daily Backups`，费用为每月 3.60 美元。备份保留一周。对于时间窗口，我选择了美国东部时间午夜到凌晨 4 点。
8.  选择一种认证方式。我选择了 `SSH`。点击 `Add SSH Key` 并按说明操作。你必须为 `SSH` 密钥命名（我用了 `tallyapp-sshkey`），它将创建两个文件。没有 `.pub` 后缀的文件是你的私钥。**务必妥善保管**。`.pub` 文件是你复制到 `DigitalOcean` 的公钥。

你可以通过以下链接了解更多关于如何使用 `SSH` 连接到 `Droplets` 的信息：[https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/](https://docs.digitalocean.com/products/droplets/how-to/connect-with-ssh/)

9.  点击 `Create Droplet`。
10.  记下分配给你服务器的 `IP` 地址，如 `图 15.4` 所示：

![](Images/B34553_15_04.png)

`图 15.4`：你服务器的 `IP` 地址

11.  在命令提示符或终端中，输入命令使用你之前创建的私有 `SSH` 密钥文件、默认的 `root` 用户账户和远程服务器的 `IP` 地址连接到你的远程服务器：

```
ssh -i tallyapp-sshkey root@167.99.52.197
```

