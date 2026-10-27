

```json
"ModifyIndex": 237,
"LockIndex": 0,
"Flags": 0,
"Value": "Y2hxaXJ2b3lhbmN1",
"Session": ""
}
```

我们可以看到，该命令的输出为我们提供了与之前读取 key 值时从 API 获取的几乎相同的输出。然而，我们并不是在谈论手动操作；我们想要……跟我一起说……自动化！

在 Consul agent 配置中，我们可以定义希望在平台上运行的 watches 以及一个 handler。handler 根据 watch 的结果执行某些功能，可以调用 HTTP 接口或执行自定义脚本。因此，随着被监视 key 的值发生变化，Consul 将执行该 handler 以主动通知某个设备。例如，将以下 watch 部分添加到 agent 配置中将导致执行 update_configs.sh 脚本：

### Sample watches configuration

```hcl
watches = [
    {
        type = "key"
        key = "son/seventh/gift"
        handler_type = "script"
        args = ["/etc/consul/updates_configs.sh"]
    }
]
```

这个特定的 watch 特别关注一个特定的 key，不过请记住，我们可以使用 keyprefix 定义一个 watch，从而监视 Consul KV 存储中整个路径。这使我们能够为特定配置创建多个 key，并且当其中任何一个 key 发生变化时，watch 将执行 handler 并执行所需的操作。

正如我希望你能看到的，Consul watches 和 handlers 在动态调整组件配置方面所能实现的强大功能是相当巨大的。我们现在已经看到了多种利用 Consul KV 存储更新配置的方法。应用程序可以通过使用 API 的 pull 方法接收更新的配置值。我们可以利用 consul-template 函数自动调整配置文件。当然，我们也可以将 KV 信息推送到几乎任何可以通过脚本或 HTTP 接口控制的系统。接下来，我们将研究如何超越 KV 并利用 Consul 通过 Terraform 动态调整我们的基础设施组件！

## Floating with consul-terraform-sync

在本书中，我们一直在研究 Consul 如何帮助我们发现和连接服务，重点关注更高级别的连接性（记住那个模型，工作在第 4 层或以上）。虽然这正是我们服务到服务通信所需的，但整个网络世界仍然依赖 IP 地址来发现、路由和保护我们的系统这一事实并未消失。防火墙和负载均衡器仍然存在，主要基于地址运行，管理进入或离开我们受管网络的流量。这通常被称为 North/South 流量。历史上，这些组件会依赖无数健康检查来验证服务可用性，或庞大的配置文件，其中一个简单的拼写错误就可能导致整个网络瘫痪。因此，出于这些原因，由容易出错的人手工更改负载均衡和防火墙组件需要仔细审查。如果只有某种方法可以自动发现并以自动化且可追踪的方式实现对这些组件的更新呢？

我们已经熟悉一个用于管理基础设施的令人难以置信的工具 Terraform，我们在本书中构建和管理自己的基础设施时一直在使用它。通过使用 Terraform OSS 二进制文件，我们一直在创建和销毁为我们的学习所需的 Consul 系统。如果你不熟悉 Terraform，它可以做更多的事情。在撰写本文时，Terraform 提供了超过 1,100 个基础设施提供商的支持集成。这包括大型云厂商以及监控组件、负载均衡器、防火墙、内容分发系统、域名系统等等。对于自动化，许多组织将 Terraform 与流水线流程集成，然而，我们将研究如何利用 Consul 提供的海量数据来自动化 Terraform 基础设施调整。

> **Important note**
>
> 直到本章的这一点，我们的大部分讨论都围绕着在服务之间创建和共享配置参数。Consul 与 Terraform 的集成极大地扩展了该功能，因此作为变更控制流程的一部分也需要额外的审查。虽然我们的工作这里集中在 Consul 和 Terraform 的开源版本上，但企业集成使能够采用细粒度策略来指定自动化可以更改什么、不能更改什么，以及什么需要额外级别的批准。此外，Terraform Enterprise 在整个过程中记录状态更改，提供任何自动化或已批准更改的历史记录。

与 **consul-template** 类似，HashiCorp 提供了一个额外的独立二进制应用程序，称为 **Consul-Terraform-Sync**。为简单起见，我将继续称 Consul-Terraform-Sync 为 CTS。利用 CTS，Consul 监控服务目录中的更改，并基于这些更改，向 Terraform 发出请求以相应地调整基础设施：

![](img/52479dafaefba00517c48a137e037b7e_207_0.png)

Figure 7.2 – CTS managed infrastructure

作为当前部署的一部分，我们已为 CTS 创建了一台专用机器，其 IP 地址由 `terraform output` 中的变量 `CTS_IP` 提供。如果找不到 Terraform 运行的输出，也没问题。运行 `terraform output` 将再次为你提供所有必要的数据。所以，让我们获取该 IP 地址并 SSH 进入 CTS 机器：

```bash
$ ssh -i <username>-consul-key.pem ubuntu@<CTS_IP>
```

当我们为本章构建基础设施时，创建了一个新目录来保存我们的 CTS 配置文件。让我们查看一下该配置文件：

### cts-config.hcl

```hcl
log_level = "INFO"
port = 8558
syslog {}
buffer_period {
  enabled = true
  min = "5s"
  max = "20s"
}
consul {
  address = "192.168.100.65:8500"
}
driver "terraform" {
  log = false
  persist_log = false
  working_dir = ""
  backend "consul" {
    gzip = true
  }
}
task {
  name        = "find-httpd-nodes"
  description = "Example task with two services"
  source      = "findkim/print/cts"
  version     = "0.1.0"
  services    = ["httpd"]
}
service {
  name = "httpd"
  tag = "python-server"
  # namespace = "service-namespace"
  # datacenter = "dc1"
  description = "Filter for httpd services that have the python-server tag"
}
```

我们在此配置中有几个部分，让我们一次一个地来看。

前八行描述了 CTS 应用的通用参数。log_level、port 和 syslog 部分应该很容易理解，但这个 buffer 是什么？

我们在自动化此类更改时发现的一个挑战是，当存在任何不稳定性时，该不稳定性可能会产生连锁反应。作为人类，我们有时会花时间对情况做出反应，而到我们能够反应时，情况已经发生了变化。当然，我说有时是因为并非总是如此，我们都知道当我们不花时间时，问题就会发生。机器通常没有这种延迟问题。实际上，我们与机器和软件的问题通常恰恰相反，即应用程序响应太快，导致来回波动。我们的 buffer 配置有助于管理这种波动，并确保在进行调整之前有一定时间等待系统达到稳定状态。

下一个部分在 consul 配置块中标识了 Consul 相关配置参数；本质上告诉 CTS 它可以在哪里找到 Consul 服务。这里我们只有 address，然而，任何证书或令牌都将在此区域说明。正如我们已经看到的 ACL 和令牌可以有多细粒度，我们希望确保 CTS 服务使用的是具有读取服务目录访问权限的令牌。

我们知道 CTS 需要连接到 Terraform 才能启动基础设施更改。可选地，terraform 配置块可以指定要使用的预下载 Terraform 二进制文件和路径。然而，如果没有此指定，CTS 将下载 Terraform 二进制文件（遵循版本规范（如果包含））并将该二进制文件保存在 CTS 工作目录中。对 Terraform 操作的日志记录以及 Terraform 后端配置应特别注意。

在处理期间，Terraform 可以记录与所管理基础设施组件的交互。此日志输出可能包含敏感信息，如果存储在运行 CTS 的机器上，可能会带来安全问题。因此，除非你正在调试，否则建议不要记录 Terraform 交互。Terraform 状态中也会记录相同的敏感信息，而这不是可选的。Terraform 利用该状态文件来了解所管理基础设施的当前状态，并评估需要哪些更改。
