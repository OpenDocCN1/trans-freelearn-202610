
> 在 Windows 上，您可能需要以管理员身份执行此操作；在 Linux 系统上，如果使用系统 Ruby 安装，则可能需要通过 sudo 执行。
> 或者，您可能希望使用 Ruby 版本管理器（如 RVM、rbenv 或 pik）来安装 Chef 及任何后续 gem。
> knife-windows gem 在 Ruby 版本大于 1.9 时可能存在字符编码问题。如果您遇到如 https://tickets.opscode.com/browse/KNIFE-410 中概述的问题，
> 请尝试使用 Ruby 1.9 安装 chef 和 knife-windows gem。（这就是 Ruby 版本管理工具派上用场的地方。）

在进入引导阶段之前，您需要确保 Windows 主机上已配置以下内容：

- 已启用 Windows 远程管理
- 您与 Windows 主机之间的任何防火墙都允许 WinRM 流量
- 您能够通过 WinRM 进行身份验证

## 启用 Windows 远程管理

大多数现代 Windows 服务器操作系统安装默认启用 Windows RM，
但如果尚未启用（或您不确定），您可以通过从命令提示符（以管理员身份）运行以下命令快速配置它：

```
winrm quickconfig
```

前面的命令执行以下任务：

- 启动 Windows 远程管理服务并将其设置为开机自动启动
- 为 Windows 远程管理服务定义防火墙规则，并打开 HTTP 和 HTTPS 端口

## 配置防火墙端口

为了使用 WinRM，端主机上的防火墙需要配置为允许 WinRM 使用的端口的流量。WinRM 有两个版本，1.0
和 2.0；每个版本都有一组不同的默认端口。对于 Windows 远程管理版本 2.0，默认端口如下：

| 协议 | 端口 |
|----------|------|
| HTTP     | 5985 |
| HTTPS    | 5986 |

对于版本 1.0，默认端口如下：

| 协议 | 端口 |
|----------|------|
| HTTP     | 80   |
| HTTPS    | 443  |

## 启用基本身份验证

要允许从外部通过基本的非加密 HTTP 身份验证对 WinRM 进行身份验证，您需要从命令提示符（而不是通过 PowerShell 提示符）运行以下命令：

```
winrm set winrm/config/client/auth @{Basic="true"}
winrm set winrm/config/service/auth @{Basic="true"}
winrm set winrm/config/service @{AllowUnencrypted="true"}
```

> 由于这允许未加密的基本 HTTP 身份验证，请不要在公共主机上执行此操作，因为这可能会带来重大的安全风险。
> 请使用适当定义的策略和 HTTPS 证书来实现更安全的身份验证机制。

## 引导 Windows 主机

一旦 knife-windows gem 在工作站上安装，您就可以继续执行引导过程，例如引导 EC2 Windows 主机：

```
knife bootstrap windows winrm ec2-54-204-177-250.compute-1.amazonaws.com
-x Administrator
```

在前面的命令中，我们正在执行带有 bootstrap 命令的 knife 实用程序，指定我们希望通过 Windows 远程管理协议在主机 ec2-54-204-177-250.compute-1.amazonaws.com 上使用 Windows 引导脚本，以 Administrator 用户身份（通过 -x 命令行标志）。

这将花费一些时间，因为它需要下载并安装 Chef 客户端，然后向 Chef 服务器注册自身。在运行过程中，以下是它正在执行的操作概述：

1. `knife` 命令将从工作站（发起引导的那一台）上位于某个 ERB 模板的批处理文件进行渲染，并将其写入目标节点上的文件。
2. 从 ERB 文件模板生成批处理文件后，节点将执行新创建的批处理文件。
3. 生成的脚本将在节点上下载 Chef 客户端 MSI 安装程序并执行安装。
4. 一旦客户端安装成功，`knife` 将存储使主机注册所需的任何文件，包括证书和配置数据。
5. Chef 客户端将执行初始遍历，以执行命令行中指定的任何初始运行列表（在我们的例子中，没有）。
6. 一旦 Chef 客户端完成其初始工作集，它将打印出它做了什么以及完成该任务花了多长时间。

一旦目标节点完成引导，您应该使用以下命令验证它是否已被配置：

`[user@workstation]% knife node list`

WIN-7QKOA4QQ21M
`[user@workstation:~]`

## 通过 MSI 安装

对于那些已经有工具部署软件使用 **Microsoft 安装程序包 (MSI)** 的人来说，另一种选择是使用可用于在 Microsoft Windows 机器上安装 Chef 客户端的 MSI。要下载 Windows 系统的 Chef 客户端，请执行以下步骤：

1. 转到 `http://www.opscode.com/chef/install`。
2. 单击 **Chef Client** 选项卡。
3. 单击 **Windows** 并选择适当的版本和架构。
4. 在 **Downloads** 下，选择要下载的 Chef 客户端版本，然后单击下拉菜单下方出现的链接以下载软件包。

执行前面的步骤后，将出现以下截图所示的网页：

![](img/5439c37d2ec2a9036b89ca6c7fbf894e_28_0.png)

5. 安装程序下载成功后，按如下方式启动安装程序：

![](img/5439c37d2ec2a9036b89ca6c7fbf894e_28_1.png)

6. 选择默认选项（有关服务的信息，请参阅以下截图后的信息框）：

![](img/5439c37d2ec2a9036b89ca6c7fbf894e_29_0.png)

默认情况下，Chef 客户端仅手动运行——某人必须通过 WinRM 会话或在控制台上执行命令。为了在后台定期运行客户端，您必须启用服务或配置计划任务。如果在安装阶段选中 **Chef Client Service** 复选框，该服务将设置为自动运行。如果您想稍后启用它，可以使用以下命令：

```
chef-service-manager -a install
```

如果您选择在后台运行客户端，则无法看到控制台输出。要查找执行日志，请参阅 `c:\chef` 下的文件 `chef-client.log`。

> 通过 MSI 下载选项手动安装 Chef 客户端不会自动将主机注册到 Chef 服务。要执行此操作，您需要将验证器密钥和功能正常的 client.rb 配置文件复制到目标主机。

作为参考，自管理 Chef 服务的示例 client.rb 文件将如下所示：

```
log_level :info
log_location STDOUT
chef_server_url 'https://yourchefserver.com/'
validation_client_name 'chef-validator'
validation_key "C:/chef/validator.pem"
```

使用托管 Chef 服务时的 client.rb 文件将如下所示：

```
log_level :info
log_location STDOUT
chef_server_url 'https://api.opscode.com/organizations/ORGNAME'
validation_key "C:/chef/ORGNAME-validator.pem"
validation_client_name 'ORGNAME-validator'
```

对于自管理 Chef 服务，validator.pem 文件可能位于 /etc/chef 下。对于托管 Chef 服务，您可以从管理控制台下载验证器密钥。

## 摘要

到现在为止，您应该对 Chef 架构的关键组件（包括重要术语）以及如何通过两种不同的机制在基于 Windows 的主机上安装 Chef 客户端有了良好的了解：

- 从工作站使用 knife 引导主机
- 使用 MSI 包手动将客户端安装到节点

在下一章中，我们将查看通过 Chef 可用的 Windows 特定资源和功能。

## Windows 特定资源

使用 Chef 管理 Windows 时，有一些作为 Windows 堆栈一部分可用的 Windows 特定资源。本章涵盖了特定于 Windows 的资源，例如 Windows 注册表、角色、MSI 等；这些资源在 Linux 系统上不可用。

### 使用 Windows 特定资源

正如大多数系统管理员所预期的那样，管理 Windows 意味着有非 Windows 系统无法使用的资源和配置数据。这些资源列表包括以下内容：

- 角色和功能
- 批处理脚本
- 自动运行脚本
- 软件包 (MSI、安装程序等)
- 打印机
- 注册表操作
- 路径
- 任务 (需要 Windows Server 2008)
- 页文件
- 系统重启
- ZIP 文件
