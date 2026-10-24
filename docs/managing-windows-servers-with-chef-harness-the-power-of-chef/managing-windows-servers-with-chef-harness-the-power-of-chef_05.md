# 1 Chef 和 Windows

如果你正在阅读本书，推测你已经熟悉 Chef，至少在概念上，并且你有一些需要管理的基于 Windows 的系统。如果你使用 Chef 管理过一些基于 Linux 的系统，或者与 chef-solo 交互过以管理虚拟机，你将准备好立即开始。如果没有，不用担心——有大量的解释和示例可以让你感到宾至如归。

如果你需要帮助安装 Chef 服务器，有许多其他资源可供你使用，或者你可以注册托管 Chef 的免费层级，无需管理自己的服务器即可立即开始。

在本章中，我们将涵盖以下主题：

- 使用 Chef 和 Microsoft Windows
- 集成 Chef 和 Windows 的方式
- 支持的平台和技术

## 使用 Windows

对于初学者来说，Chef 的客户端组件是用 Ruby 编写的，这是一种非常流行的语言。由于 Ruby 的跨平台特性，对 Windows 的支持与对 Linux 和类 UNIX 系统的支持一样直接，并且已经存在相当长一段时间，可以追溯到大约 2011 年发布的 **knife-windows** gem。





Chef 在客户端系统上使用 Ruby 作为脚本语言，因此只要 Ruby 支持的地方，Chef 就能运行。仅凭这一点，Chef 就成为管理多种操作系统组合的非常强大的工具。Chef 更进一步，提供了一种**领域特定语言 (DSL)**，使得编写用于与 Windows 主机交互的配方看起来与 UNIX 类平台毫无二致。除了某些资源名称和路径以及 Windows 特定资源（如 Windows 注册表）的存在之外，配方几乎可以与任一平台直接兼容。这意味着，现有的大量 Chef 配方书和配方几乎无需修改即可使用。

还需要注意的是，虽然许多讨论集中在服务器管理、云端扩展等方面，但 Chef 并非仅用于管理服务器；它也可以用于管理客户端工作站。利用可用资源，你可以同样轻松地安装和配置桌面应用程序、导入注册表设置、管理用户、设置打印机等等。

### 与最终主机交互

Linux 类系统可以通过 SSH 执行命令，而 Windows 平台有一个名为 **Windows Remote Management (WinRM)** 的额外机制。就像你会使用 `knife ssh` 来管理 Linux 系统一样，`knife winrm` 可用于使用 WinRM 协议远程在 Windows 主机上执行命令。

例如，有人可能会执行以下命令来连接到 Linux 主机：

```
knife ssh "role:mysql" "chef-client" --sudo -x ubuntu
```

以下命令将以相同的角色连接到 Windows 主机：

```
knife winrm "role:mysql" "chef-client" -x Administrator
```

如你所见，`winrm` 子命令支持在匹配所提供搜索条件的任意数量主机上执行命令，这与 `ssh` 子命令完全一样。尽管与主机通信的协议可能不同，但通过 `knife` 与它们交互的机制保持一致。

![](img/5439c37d2ec2a9036b89ca6c7fbf894e_16_0.png)

### 引导 Windows 主机

引导主机旨在为主机做好准备，并完成与 Chef 服务（无论是你自己的 Chef 服务器还是托管安装）的注册。正在引导的主机通常只包含一个裸机操作系统安装；不过，也可以引导已有软件配置的主机。引导脚本负责执行以下功能：

- 安装 Ruby 1.8.7 及 RubyGems
- 安装 RubyInstaller 开发工具包 (DevKit)
- 安装支持 Chef 的 Windows 特定 gem
- 从 RubyGems.org 安装 Chef
- 将验证密钥写入 `C:\\chef\\validation.pem`
- 可选地写出加密数据包密钥
- 在 `C:\\chef\\client.rb` 中写出 Chef 的默认配置文件
- 在 `C:\\chef\\first-boot.json` 中创建初始运行列表 JSON 文件
- 首次运行 `chef-client` 以将节点注册到 Chef

使用 Windows 远程管理协议引导 Windows 主机的示例可能如下命令所示：

```
knife bootstrap windows winrm windowshost.domain.com -x Administrator
```

此命令将通过 Windows 远程管理协议以 Administrator 身份连接到 `windowshost.domain.com`，然后运行 Windows 引导脚本中的命令。要查看正在运行的命令的完整列表，可以在 `https://github.com/opscode/knife-windows/blob/master/lib/chef/knife/bootstrap` 找到 Windows 引导脚本。

### 与云提供商一起扩展

通过将自动引导 Windows 系统的能力与提供 Windows 虚拟主机的提供商相结合，你可以轻松地将云服务器集成到基础架构中。Chef 已支持使用 Azure、AWS 和 Rackspace API 来管理云基础架构，包括自动启动、停止和配置这些服务的新实例。如果你使用的服务目前不受支持，完全有可能开发一个插件来提供与该提供商的集成。通过 Chef，你可以轻松管理一组现场和非现场主机，包括物理服务器和虚拟服务器的混合。这意味着当你需要时，可以在更短的时间内启动新服务器，并在不使用时将其删除，从而节省时间和金钱。

### 使用 PowerShell 进行脚本编写

现代 Windows 系统自带 PowerShell 运行时，这是一个与系统交互的极其强大的工具。自然而然地，由于 Chef 是一种面向开发者的系统管理方式，编写在最终主机上执行的脚本是扩展 Chef 功能的一种便捷且灵活的方式。Chef 提供了一种在 Windows 上执行 PowerShell 脚本的机制，就像它支持在 Linux 主机上运行 Bash 脚本一样。一个非常简单的示例可能是以下 PowerShell 脚本，它将一行文本写入文件：

```
powershell "say-hello" do
  code <<-EOH
    $stream = [System.IO.StreamWriter] "C:\\hello.txt"
    $stream.WriteLine("Hello world!")
    $stream.Close()
  EOH
end
```

前面的代码允许你在配方中通过执行在受管系统上定义的脚本来充分发挥 PowerShell 的全部功能。这些脚本甚至可以从配方中的配置数据和其他变量动态生成。

### 与 Linux 类系统集成

随着时间的推移，拥有异构网络变得越来越常见。某些软件要么不存在，要么在一个平台上的支持不如在另一个平台上好。因此，管理员会遇到需要部署和管理并行运行多种操作系统的主机的情况。集成 Windows 和 Linux 类系统伴随着其自身的一系列挑战，Chef 通过提供一种与 Linux 和 Windows 类系统交互的一致方式来帮助解决这些问题。

对于管理此类基础架构（具体来说，是运行 Windows 和 Linux 组合的系统集合）的任何人来说，Chef 有一些惊人的功能。因为它能够使用相同的声明语言和配置数据来对 Windows 和 Linux 系统进行建模，你可以使用同一工具轻松配置 Linux 和 Windows 系统。

例如，你可以拥有一个配方书，其中包含一个配置网络主机防火墙的配方。该配方可以搜索 Chef 的配置数据，查找网络中所有其他主机，收集它们的 IP 地址列表，并打开通往所有这些 IP 地址的流量。如果你配置了一个新主机，所有受管主机都会自动了解新主机并添加一条新的防火墙规则。此外，因为 Chef 提供了自己的声明语言来隐藏实现细节，你可以专注于想要实现的目标，而不是如何实现它。Chef 知道它正在运行的平台，以及如何加载资源的特定于系统的实现，例如网络接口、防火墙、用户、文件等。完全有可能编写配方来安装 Apache 或 MySQL，这些配方可以在 Linux 和 Windows 平台上工作。

这使得集成任意数量的 Windows 和 Linux 系统变得更加容易，而无需维护多套脚本来实现相同的最终目标。例如，假设你同时拥有 Windows 和 Linux 的防火墙配方书，那么可以编写类似于以下的配方：

```
search(:node, 'role:web_server').each do |node|
  ip = node[:external_ip]
  firewall_rule "#{ip}" do
    source "#{ip}"
    action :allow
  end
end
```

在前面的代码中，我们正在搜索所有具有 web_server 角色的节点，并调用 firewall_rule 资源以允许来自该源的流量。请注意，配方并未引用 Windows 防火墙软件或 Linux 的防火墙工具 iptables。相反，Chef 的自定义语言允许我们描述我们正在做什么，而不是如何实现我们的目标。“如何”在提供程序中实现，“什么”由资源描述，两者都在我们的防火墙配方书中提供。





### 使用 Windows 特定资源

Chef 在 Windows 系统中提供了一些特定于 Windows 的资源。Chef 可以自动确定正在执行配方的主机类型，并根据主机类型执行不同的操作。例如，安装特定软件包（如 MySQL）在不同主机之间可能大体相同，但在 Windows 系统中可能需要略有不同的设置或需要存储注册表设置。特定于 Windows 的一些资源包括以下内容：

- Batch 脚本
- PowerShell 脚本
- Autorun 脚本
- 软件包（MSI、安装程序等）
- 打印机
- Windows 注册表
- 网络路径
- 系统任务

### 支持的平台

Chef for Windows 支持编写时的最近版本的 Windows。这包括以下内容（但也可能适用于其他较新的版本）：

- Windows Server 2003 R2
- Windows Server 2008
- Windows 8
- Windows 7
- Windows Vista

### 摘要

如您所见，Chef 为 Windows 管理员提供了许多功能，无论是管理仅限于 Windows 的基础设施，还是管理异构的 Windows 和 Linux 基础设施。

现在您已经了解了在管理 Windows 系统时 Chef 可以为您带来的好处，让我们在下一章中了解如何安装客户端以及它如何融入 Chef 生态系统的整体架构。
