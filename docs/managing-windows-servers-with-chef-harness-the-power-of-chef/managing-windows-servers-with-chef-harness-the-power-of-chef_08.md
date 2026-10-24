## 5 使用 Chef 管理云服务

Chef 的一个非常强大的用途是将其用于将基础设施无缝扩展到云端，就像本地硬件一样。Chef 有多种机制可与流行的云服务提供商集成。在本章中，我们将了解如何使用 Chef 管理和部署软件到各种流行的云服务提供商。

在本章中，我们将涵盖以下主题：

- Microsoft Azure
- AWS EC2
- Rackspace Cloud

### 使用 Microsoft Azure

Azure 是微软对 EC2 和 Rackspace Cloud 的竞争对手。这三者都会为你提供 Windows 虚拟化，因此本章包含了如何将 Azure 与 Chef 集成的信息。

`knife-azure` gem 提供了通过 `knife` 实用程序控制 Azure 所需的功能。Chef 使用插件来提供扩展功能，包括管理云服务。Azure 支持存在于名为 `knife-azure` 的 Ruby gem 中，并通过 `gem` 命令行实用程序安装，如下所示：

```
gem install knife-azure
```

此命令将安装 `knife-azure` 依赖的所有 gem，而不仅仅是 Chef。

> 就撰写本文时，`knife-azure` gem 依赖于旧版本的 bundler，这可能会导致问题，并可能要求你手动降低已安装 bundler 的版本。使用 RVM 或其他 Ruby 管理器将有助于隔离这些问题，并允许你管理 gem。

### 下载管理证书

为了使用 Azure API，你需要有一个管理证书来标识你与服务的身份。你可以从 `https://manage.windowsazure.com/` `publishsettings/index?client=xplat` 创建并下载生成的管理证书。

### 为 knife 配置 Azure

现在你已将管理证书放在 `.publishsettings` 文件中，`knife` 需要知道它。这通过 `knife.rb` 配置文件完成，正如你所料。要让 `knife` 知道在哪里找到你新下载的 `.publishsettings` 文件，首先将其移动到某个位置（例如你的主目录或 `$HOME/.chef`），然后将以下行添加到你的 `knife.rb` 文件中：

`knife[:azure_publish_settings_file] = "/path/to/publishsettings_file"`

现在 `knife` 已拥有所需的所有信息，我们将能够向 Azure 发出 API 调用。

### 创建新的 Azure 虚拟机

为了创建新服务器，你需要知道使用哪个镜像；你可以使用 `azure` 插件的 `image list` 子命令查看可用镜像列表，如下所示：

`knife azure image list`

Windows-Server-2012-Essentials Windows East Asia, Southeast Asia
Ubuntu-12_04_2-LTS-amd64-server Linux East Asia, Southeast Asia
Windows-Server-2008-R2 Windows West US, East Asia

如你所见，输出将列出镜像标识符（为打印而缩短）、它们提供的平台以及可用区域。可用镜像列表会相当长，并且会随 Azure 添加更多基础镜像到其不断增长的列表而随时间变化；因此，每当你配置新虚拟机时，此命令都会很有用。此列表为你提供了构建新主机所需的两条信息：镜像标识符，以及该镜像可用的区域。

使用这些信息，让我们看看以下命令，了解我们如何配置一台新的 Windows Server 2012 Datacenter 版主机，拥有 127 GB 存储空间，位于美国西部数据中心，DNS 名称为 `00c0ff3300`：

```
knife azure server create --azure-dns-name "00c0ff3300" \
--azure-service-location "West US" \
--azure-source-image a699494373c04fc0bc8f2bb1389d6106__Windows-
Server-2012-Datacenter-201401.01-en.us-127GB.vhd \
--winrm-user 'azure' --winrm-password '+0ps3kr3+' \
--distro 'windows-chef-client-msi'
```

发出此命令后，你将看到主机配置的状态随进度而变化。Chef 将等待虚拟机配置完成并标记为就绪。在此期间，你可以使用以下命令查看状态以及 Azure 达到该状态所需的时间：

```
Waiting for virtual machine to reach status 'provisioning'..... vm state
'provisioning' reached after 2.67 minutes
```

一旦主机处于就绪状态，Chef 将能够告诉你更多关于主机的信息，包括 IP 地址和 WinRM 端口。你应期望类似于以下命令的输出：

```
DNS Name: 0c0ff330.cloudapp.net
VM Name: 0c0ff330
Size: Small
Azure Source Image: a699494373c04fc0bc8f2bb1389d6106__Windows-Server-
2012-Datacenter-201401.01-en.us-127GB.vhd
Azure Service Location: West US
Public Ip Address: 138.91.227.224
Private Ip Address: 100.82.76.57
WinRM Port: 5985
Environment: _default
```

配置过程可能需要一段时间，具体取决于你选择的位置和基础镜像。在配置过程中，它将创建命令行中指定的 WinRM 用户，在此示例中为 azure，并使用指定的密码。但是，WinRM 的基本身份验证默认不会开启；Kerberos 身份验证默认会启用，但 Kerberos 支持超出了本书的范围。因此，你将在此处遇到无法引导主机的错误，我们将在下一组步骤中通过启用 WinRM 的基本 HTTP 身份验证机制来修复此问题。

### 引导你的 Azure 节点

你的初始 **虚拟机**（VM）需要为 WinRM 启用基本身份验证；完成此操作后，你可以将新创建的镜像保存为可重用的 Azure 镜像，其中包含你的管理凭据和已启用 WinRM 基本身份验证。

要为 WinRM 启用基本身份验证支持，你需要执行以下步骤：

1. 在 Azure 实例上启用 RDP。
2. 使用远程桌面连接到新虚拟机，并使用配置阶段指定的凭据登录（在此示例中，帐户为 `azure`，密码为 `+0ps3kr3+`）。
3. 打开 Windows 命令提示符并运行以下命令，以允许 WinRM 接受基本 HTTP 身份验证：

   ```
   winrm set winrm/config/client/auth @{Basic="true"}
   ```

此时，你可以返回工作站，并使用 `knife bootstrap` 命令引导新主机，如下所示：

```
knife bootstrap windows winrm 00c0ff3300.cloudapp.net \
  --winrm-user azure \
  --winrm-transport ssl
```

前面的命令将提示你输入管理员密码。输入正确凭据后，你可以继续在新 Azure 主机上引导 Chef 客户端并将其注册到 Chef 服务器。完成后，你可以使用 `node list` 命令验证其是否已正确注册，如下所示：

```
$ knife node list
00c0ff3300
i-49117415
```

请注意，在我们的已注册节点中，新虚拟机现在已显示。





### 创建可重用的映像

要从新创建的云服务器创建可重用映像，请使用以下说明：

1. 通过 RDP 连接到虚拟机。
2. 以管理员用户身份打开 **命令提示符** 窗口。
3. 将目录更改为 `windows\system32\sysprep`，然后运行 `sysprep`。
4. 在 **系统清理操作** 中，单击 **进入系统初始体验 (OOBE)**，并确保选中 **通用化**。
5. 在 **关机选项** 中，选择 **关机**。
6. 单击 **确定** 将准备主机并关闭虚拟机。
7. 转到 Azure 管理门户并单击 **虚拟机**。
8. 选择要捕获的虚拟机。
9. 在页面底部的命令栏上，单击 **捕获**。
10. 在提供的对话框中为新映像键入名称；例如，我们将其命名为 `windows2012winrm`。
11. 单击对勾标记以捕获映像。

> 当你捕获虚拟机的映像时，原始机器将被删除。

12. 新映像现在可在 Web 控制面板上使用（在 **映像** 下），也可以通过 `knife azure image list` 命令找到。

现在你已保存映像，可以使用它来生成预先配置了启用 WinRM 基本身份验证的新主机。如果在映像完成后运行命令，你将看到类似于以下输出的内容：

`$ knife azure image list | grep winrm`
`windows2012winrm                      Windows West US`

在此，我们筛选出仅包含单词 `winrm` 的映像，仅显示我们新创建的映像 `windows2012winrm`。你还会注意到，映像被锁定在创建它们的区域。

你可以稍后使用映像名称的 `--azure-source-image` 值来预配主机，在我们的示例中该值为 `windows2012winrm`：

`knife azure server create --azure-dns-name "0c0ff330" \
--azure-service-location "West US" \
--azure-source-image windows2012winrm \
--winrm-user 'azure' --winrm-password '+0ps3kr3+' \
--distro 'windows-chef-client-msi'`

使用 `knife azure server list` 检查它是否已在 Azure 中创建，如以下代码所示：

`knife azure server list`

| DNS 名称 | VM 名称 | 状态 | IP 地址 | SSH 端口 | WinRM 端口 |
|---|---|---|---|---|---|
| 0c0ff330.cloudapp.net | 0c0ff330 | ready | 138.91.227.224 | 5985 | |

如你所见，一旦你拥有具有所需功能的基础映像，就可以使用它以一致的方式快速为基础架构预配新主机。

### 管理 Amazon EC2 实例

Amazon EC2 是一个非常流行的云计算平台，`knife` 通过 `knife-ec2` 插件支持从命令行管理 EC2 实例。在本节中，我们将演示使用 EC2 的以下步骤：

- 安装 EC2 `knife` 插件
- 使用 AWS 凭据配置 `knife`
- 查找所需的 **Amazon 机器映像 (AMI)**，即预构建的系统映像
- 使用 `knife` 预配新主机
- 引导新创建的主机
- 使用角色配置新主机

### 安装 EC2 knife 插件

从 Chef 0.10 开始，`ec2` 子命令已从 `knife` 的内置命令移至外部 gem `knife-ec2`。要使用 EC2 命令，你需要安装该 gem，可以使用以下命令完成：

`gem install knife-ec2`

这将安装 EC2 插件所需的所有 gem 依赖项。

### 设置 EC2 身份验证

要管理 EC2 主机，你需要将 SSH 密钥对和 AWS 访问密钥设置在 `knife` 配置文件中。将 SSH 密钥对下载到主机上的某个位置（类 UNIX 系统上的 `$HOME/.ssh`），并将以下命令添加到 `knife.rb` 配置文件中：

```
knife[:aws_access_key_id] = "YOUR ACCESS KEY"
knife[:aws_secret_access_key] = "SECRET KEY"
```

这些命令告诉 `knife` 在进行 API 调用以执行预配新主机和终止实例等操作时使用哪些 AWS 凭据。如果没有这些设置，`knife` 将无法向 EC2 进行 API 调用。完成这些必要更改后，让我们看看如何使用 `knife` 创建新的 EC2 实例。

### 预配 EC2 实例

最初，我们将查看使用 Windows 2012 Server AMI 之一预配实例。使用 `knife`，我们指定要使用的 AMI、要定位的可用区以及要创建的实例大小。例如，要在 `us-east-1e` 可用区中使用 Windows Server 2012 创建 `m1.large`，我们需要使用标识符为 `ami-2a80bd6f` 的 AMI。可以通过 AWS EC2 控制面板或各种其他提供此信息的网站找到 AMI ID。在决定使用哪个 AMI 时，请记住某些 EC2 实例是 32 位的，某些是 64 位的；根据你想要使用的实例类型、区域和存储方法选择合适的 AMI。

预配新主机的操作可以简单地通过向 `knife` 提供 AMI、区域、规格和 SSH 密钥来完成，如以下命令所示：

```
[user]% knife ec2 server create -I ami-2a80bd6f \
    -f m1.large -Z us-west-la \
    -S jewartec2
```

运行此命令时，`knife` 将交互式地显示预配新主机的进度。以下是在该过程中你可能会看到的示例：

```
Instance ID: i-3c5385d60
Flavor: m1.large
Image: ami-2a80bd6f
Region: us-west-1
Availability Zone: us-west-la
```

Security Groups: default
Tags: Name: i-3c5385d60
SSH Key: jewartec2

Waiting for instance...............
Public DNS Name: ec2-54-219-245-213.us-west-1.compute.amazonaws.com
Public IP Address: 54.219.245.213
Private DNS Name: ip-10-168-109-170.us-west-1.compute.internal
Private IP Address: 10.168.109.170

Waiting for winrm..........

如果你执行此操作，会发现它会在此处停留，并最终在尝试使用 WinRM 连接到新的 EC2 实例时超时。不幸的是，与 Azure 实例一样，WinRM 默认不允许 HTTP 基本身份验证，这使得引导主机变得有些棘手。但是，与 Azure 不同，EC2 允许你预配系统并在系统设置期间执行初始脚本。我们可以利用此功能在预配步骤期间在 Windows 中启用所需的功能，以便 Chef 可以引导主机，并且可以将其作为一步过程进行管理。

### 在 EC2 中执行自定义用户脚本

使用 `knife` 带自定义初始化脚本预配 EC2 实例是一个两步过程。首先要做的是在工作站上创建包含要执行的指令的文本文件作为脚本。完成后，你可以在使用 `knife` 预配主机时通过命令行参数将脚本提供给新创建的 EC2 实例。

### 编写用户脚本

我们将为 EC2 实例编写的脚本将负责执行以下操作：

- 为 WinRM 启用基本 HTTP 身份验证
- 通过纯文本启用 WinRM（在生产环境中你可能需要调整此设置）
- 增加每个 shell 实例的最大内存
- 将 WinRM 超时增加到 30 分钟
- 向主机添加防火墙规则，以在端口 5985 和 5986 上启用 WinRM
- 重启 WinRM 以确保它获取更改并设置为开机启动

在以下代码中，我们指定正在运行 PowerShell 脚本，并将内容放在 `<powershell>...</powershell>` 块中：

```
<powershell>
winrm quickconfig -q
winrm set winrm/config/winrs '@{MaxMemoryPerShellMB="300"}'
winrm set winrm/config '@{MaxTimeoutms="1800000"}'
winrm set winrm/config/service '@{AllowUnencrypted="true"}'
winrm set winrm/config/service/auth '@{Basic="true"}'
netsh advfirewall firewall add rule name="WinRM 5985" protocol=TCP dir=in localport=5985 action=allow
netsh advfirewall firewall add rule name="WinRM 5986" protocol=TCP dir=in localport=5986 action=allow
net stop winrm
sc config winrm start=auto
net start winrm
</powershell>
```

### 提供自定义用户脚本

现在我们有了自定义用户脚本，knife 可以被告知使用 `--user-data` 命令行选项加载脚本。为了在 EC2 上获取随机生成的管理员密码，我们还需要传递 `--identity-file` 选项，指向下载的 SSH 密钥对副本（在此示例中为 `$HOME/.ssh/jewartec2.pem`）：

```
$ knife ec2 server create -I ami-5eccf31b -f m1.large \
    -Z us-west-la -S jewartec2 \
    --user-data ec2userdata.txt \
    --identity-file $HOME/.ssh/jewartec2.pem
```

> 确保你的默认安全组允许连接到 WinRM（非 SSL 和 SSL 分别使用端口 5985 和 5986），或者指定允许此类连接的安全组。如果没有访问这些端口的权限，你将无法引导新预配的主机。





现在，你将看到实例已被配置并引导；我们的自定义脚本已启用 knife 通过 WinRM 连接并将 Chef 客户端安装到主机上。不过，这里你会看到一些不同之处；WinRM 将预先配置并可正常工作，因此引导过程将在配置阶段成功。你将看到引导脚本在新虚拟机上呈现并执行。输出示例（缩短版）如下：

```
Waiting for winrm..... done

Waiting for Windows Admin password to be available...
Bootstrapping Chef on ec2-54-193-161-138.us-west-1.compute.amazonaws.com
Rendering "C:\\Users\\ADMINI-~1\\AppData\\Local\\Temp\\bootstrap-293489-123904.
bat chunk 1
Rendering "C:\\Users\\ADMINI-~1\\AppData\\Local\\Temp\\bootstrap-293489-123904.
bat chunk 2
Starting chef to bootstrap the node...
C:\\Users\\Administrator>chef-client -c c:/chef/client.rb -j c:/chef/first-
boot.json -E _default
[2014-03-19T08:11:30+00:00] INFO: *** Chef 11.10.4 ***
[2014-03-19T08:11:30+00:00] INFO: Chef-client pid: 8754
```

引导完成后，你将再次看到实例详细信息，其中包含更多信息，例如 EC2 实例根卷的类型、ID 和本地设备名称（无论它使用的是 EBS 还是实例存储）。

> 偶尔，EC2 实例无法正确配置管理员密码。如果发生这种情况，在 WinRM 可用后，引导步骤将无法进行身份验证；请尝试丢弃该实例并配置一个新实例。

主机配置完成且引导步骤结束后，假设没有错误，你可以按以下方式验证新配置的主机是否列在你的 Chef 服务中：

$ knife node list
0c0ff3300
i-49117415

输出将包含你在命令行中指定的新引导节点 ID，或者如果你未指定节点名称，则包含默认的 EC2 实例名称。如果你在上一节中配置了 Azure 实例，还会看到 Azure 实例名称。恭喜，你现在已使用仅两条命令配置了一台新的 Windows Server EC2 实例，并将其注册到你的 Chef 服务中！

### 终止 EC2 实例

测试完成后，你可能不希望让 EC2 实例继续运行，因为它在空闲时会产生费用。为此，请执行以下步骤：

- 列出你的 EC2 实例。
- 从 EC2 中删除服务器。
- 从 Chef 中删除服务器。
- 验证实例在 Chef 或 EC2 中不再存在。

要列出 EC2 实例，请使用 `ec2` 命令的 `server list` 子命令，它将列出指定区域中的所有 EC2 实例。如果未指定区域，`us-east-1` 是默认区域。例如，在配置第一个主机后执行此命令将显示一个实例的列表。输出太宽，无法打印，但你还会在列表中看到诸如安全组、SSH 密钥、使用的 AMI、IAM 配置文件以及虚拟主机当前状态等信息，如下所示：

```
$ knife ec2 server list
Instance ID Name             Public IP        Private IP       Flavor
i-49117415  i-49117415  54.219.201.136  10.168.93.149  m1.large
```

删除实例与创建或列出实例一样简单。这里，`server delete` 子命令将与要终止的实例标识符一起调用。这将使用 EC2 API 发出终止命令——此操作不可逆转，因此命令会提示你确认是否真的要删除实例：

```
$ knife ec2 server delete i-49117415
Instance ID: i-49117415
Flavor: m1.large
Image: ami-5eccf31b
Region: us-west-1
Availability Zone: us-west-1a
```

### 移除 Chef 节点

此时，EC2 实例正在被终止并从你的账户中移除。但是，它并未从 Chef 服务中移除，这需要使用 `node delete` 命令单独完成。在以下代码中，指定的是 Chef 节点名称，而不是实例标识符：

```
$ knife node delete i-49117415
Do you really want to delete i-49117415? (Y/N) y
Deleted node[i-49117415]
```

使用 `node list` 验证节点是否已从 Chef 中移除，如下所示：

```
$ knife node list
00c0ff3300
```

输出应显示你的 EC2 实例不再与 Chef 注册。

### 与 Rackspace Cloud 交互

Rackspace Cloud 是另一个受 Chef 良好支持的流行云计算提供商。与 EC2 类似，Rackspace Cloud 也有一个 `knife` 插件，如下所示：

```
gem install knife-rackspace
```

Security Groups: default
SSH Key: jewartec2
Root Device Type: ebs
Public DNS Name: ec2-54-219-201-136.us-west-1.compute.amazonaws.com
Public IP Address: 54.219.201.136
Private DNS Name: ip-10-168-93-149.us-west-1.compute.internal
Private IP Address: 10.168.93.149

Do you really want to delete this server? (Y/N) y
WARNING: Deleted server i-49117415
WARNING: Corresponding node and client for the i-49117415 server were not deleted and remain registered with the Chef server

此命令故意设置得详细一些。你最不想做的就是因为输入错误而销毁一个关键的运行实例。请务必仔细检查属性，以确保要终止的节点是你预期的那个。

与 AWS 要求一组凭据来与 API 交互以创建和终止实例一样，Rackspace Cloud 也有其自己的配置。但是，Rackspace Cloud API 稍微简单一些；你需要向 knife 提供你的 Rackspace Cloud 用户名和 API 密钥。对于那些不知道自己 API 密钥的人，可以在 Rackspace Cloud 控制面板中找到。要添加到 knife.rb 文件中的适当配置如下：

```
knife[:rackspace_api_username] = "Your Rackspace API username"
knife[:rackspace_api_key] = "Your Rackspace API Key"
```

这些数据可以硬编码到配置文件中；由于 knife 配置文件只是 Ruby，数据可以通过评估环境变量或查看本地文件来生成。如果你将 knife.rb 文件提交到源代码仓库，这很有用，这样就不会泄露凭据。

### 配置 Rackspace 实例

Rackspace Cloud 服务器配置与 EC2 一样简单。由于 Rackspace 提供系统镜像的方式，传递给 knife 的命令行选项有一些差异。你不是使用实例大小和 AMI，而是指定要配置的系统规格（节点的 CPU、内存和磁盘分配）以及用于镜像实例的操作系统。为了确定有哪些规格可用，knife rackspace 插件提供了 rackspace flavor list 子命令，如下所示：

```
$ knife rackspace flavor list --rackspace-region=IAD
```

由于不同区域可能有不同的容量，因此检查你想要配置节点的区域中有哪些可用是个好主意。这将生成一个规格及其规格列表；截至目前，IAD 中的一些当前产品如下所示：

```
$ knife rackspace flavor list --rackspace-region=IAD
ID Name VCPUs RAM Disk
2 512MB Standard Instance 1 512 20 GB
3 1GB Standard Instance 1 1024 40 GB
4 2GB Standard Instance 2 2048 80 GB
5 4GB Standard Instance 2 4096 160 GB
```

除了知道要使用哪个规格的主机外，你还需要一个镜像标识符（类似于 AMI 标识符）来安装到新主机上。同样，此列表可能因区域而异并随时间变化，因此有一个命令来列出各种镜像，即 rackspace image list，如下所示：

```
$ knife rackspace image list --rackspace-region=IAD | grep -i windows
db7692f7-3cfa-4a9f-a072-99b21bd126da Windows Server 2008 R2 SP1
a2139adc-df9b-98dc-aa10-5dcdff8a982b Windows Server 2012 + SQL Server
7e106eaa-c863-49cb-bf5a-0cb7c7fc9ba1 Windows Server 2012 + SharePoint
```

如你所见，有多个可用的 Windows 发行版可供配置。为了配置新主机，你将使用 server create 命令，类似于 EC2 命令。以下 knife 命令将在 IAD 数据中心配置一台运行 Windows Server 2012 的 1 GB 主机：

```
$ knife rackspace server create \
--I db7692f7-3cfa-4a9f-a072-99b21bd126da \
--flavor 3 --rackspace-region=IAD \
--server-create-timeout 1800
```

### 将配置注入虚拟机

与 EC2 类似，我们需要在配置主机时注入一些命令来打开防火墙并执行其他一些任务。将以下内容保存到文件 bootstrap.cmd 中：

```
net start w32time
w32tm /config /manualpeerlist:"0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org" /syncfromflags:manual /reliable:yes /update
w32tm /resync
netsh advfirewall firewall set rule group="remote administration" new enable=yes
netsh advfirewall firewall add rule name="WinRM Port" dir-in action=allow protocol=TCP localport=5985
```

此脚本将启动时间服务，同步时钟，并在端口 5985 上打开防火墙以允许 WinRM 连接。创建此文件后，我们需要将其注入到 Rackspace 虚拟机中，作为名为 C:\cloud-automation\bootstrap.cmd 的特殊文件在配置时注入，并告诉 knife 使用 WinRM 而不是 SSH 来引导主机：



$ knife rackspace server create \
    -I db7692f7-3cfa-4a9f-a072-99b21bd126da \
    --flavor 3 --rackspace-region=IAD \
    --server-create-timeout 1800 \
    --file "C:\\cloud-automation\\bootstrap.cmd=bootstrap.cmd" \
    --bootstrap-protocol winrm \
    -x Administrator

API 响应配置新主机的请求后，您将看到该主机的 Rackspace 元数据，例如实例 ID、名称、flavor 和 image。之后，当 Rackspace Cloud 启动新实例时，您将看到终端上打印出一系列点。值得注意的是，这可能需要大约 20 到 30 分钟，具体取决于 image、实例大小等因素。因此，修改 `--server-create-timeout` 参数的值可能是有意义的。完成后，您将看到类似于以下命令的输出：

```
Instance ID: alad2318-28c4-47b7-a353-b6a99a7cc574
Name: rs-5889646228538071
Flavor: 1GB Standard Instance
Image: Windows Server 2012
Metadata: []
RackConnect Wait: no
ServiceLevel Wait: no
........
Public DNS Name: mail.drizzlelabs.com
Public IP Address: 162.209.101.233
Private IP Address: 10.176.13.30
Waiting for winrm...
Bootstrapping Chef on 162.209.101.233
```

一旦 bootstrap 步骤完成，假设没有错误，您将看到 Chef 客户端已运行，类似于之前的执行，并且您将再次获得 image 数据。只有一点细微的差别；即，administrator 密码将在输出中提供给您，如下所示：

Public IP Address: 162.209.101.233
Private IP Address: 10.176.13.30
Password: P123atLaMNbd
Environment: _default

您现在可以使用 `knife node list` 验证新配置的主机是否列在您的 Chef 服务中：

```
$ knife node list
0c0ff3300
rs-5889646228538071
```

输出将包含您在命令行（通过 -N）指定的新 bootstrapped 节点 ID，或由 Rackspace 生成的名称（在此示例中，它将是 rs-5889646228538071）。恭喜，您已使用单个命令配置了新的 Rackspace 实例！

### 终止实例

测试完成后，您可能不希望让 Rackspace 实例继续运行，因为它在空闲时会产生费用。为此，请执行以下步骤：

- 列出您的 Rackspace 服务器。
- 从 Rackspace 删除服务器。
- 从 Chef 删除服务器。
- 验证实例不再存在于 Chef 或 Rackspace 中。

要列出您的 Rackspace 实例，请使用 `rackspace` 命令的 `server list` 子命令，它将列出指定区域中的所有 Rackspace 实例。类似于 EC2 `server list` 命令的输出，`rackspace` 服务器列表输出将如下所示：

```
$ knife rackspace server list --rackspace-region=IAD
Instance ID Name Public IP Private IP Flavor
alad2318-28c4 rs-5889646228538071 162.209.10 10.176.13.30 3
```

与其他服务器列表输出一样，这对于打印来说太宽了，但您将在结果中看到 image ID 和服务器的状态。在这里，我们新创建的虚拟主机应该处于 `active` 状态。

删除实例只是一个命令——`rackspace server delete` 子命令将使用要终止的 Rackspace 实例标识符调用。请记住，这是不可逆的，因此命令将提示您确认是否真的要删除实例：

```
$ knife rackspace server delete alad2318-28c4-47b7-a353-b6a99a7cc574
Instance ID: alad2318-28c4-47b7-a353-b6a99a7cc574
Name: rs-5889646228538071
Flavor: 1GB Standard Instance
Image: Windows Server 2012
Public IP Address: 162.209.101.233
Private IP Address: 10.176.13.30
Do you really want to delete this server? (Y/N) y
WARNING: Deleted server alad2318-28c4-47b7-a353-b6a99a7cc574
WARNING: Corresponding node and client for the alad2318-28c4-47b7-a353-b6a99a7cc574 server were not deleted and remain registered with the Chef server
```

### 删除 Chef 节点

此时，Rackspace 实例正在被终止并从您的帐户中删除。但是，它未从 Chef 服务中删除；这需要使用 `node delete` 命令单独完成。在以下代码中，指定的是 Chef 节点名称，而不是实例标识符：

```
$ knife node delete rs-5889646228538071
```

使用以下命令验证节点是否已从 Chef 中删除：

```
$ knife node list
0c0ff3300
```

### 摘要

如您所见，Chef 对从命令行管理云服务器提供了强大的支持。您可以将云主机与物理基础架构相结合，使用 Chef 使您的系统具有可扩展性和弹性，这在以前可能无法实现。这些只是当今市场上最流行的三种云服务；Chef 对各种其他基础架构提供商也有越来越多的支持，并且随着时间的推移，此列表将不断增长。

现在您已经学习了如何编写食谱并使用流行的云服务扩展基础架构，让我们在下一章中看看一些高级主题，包括在 Chef 中测试食谱和多平台支持。

### 超越基础

现在您已经了解了如何将 Chef 集成到 Windows 环境中，让我们看看一些高级主题，如下所示：

- 管理异构网络
- 处理多个平台
- 版本控制和源代码管理
- 使用 ChefSpec 测试各种平台的食谱

#### Chef 的声明式语言

Chef 食谱是声明式的，这意味着它提供了一种高级语言，用于描述为完成手头任务需要做什么，而不需要您提供特定的实现或过程。这意味着您可以专注于构建食谱和使用抽象资源建模基础架构，从而清楚发生了什么，而不需要知道它是如何发生的。以我们之前研究过的用于部署 IIS 应用程序的食谱的一部分为例，该部分负责安装某些 Windows 功能：

```
features = %w{IIS-ISAPIFilter IIS-ISAPIExtensions
NetFx3ServerFeatures NetFx4Extended-ASPNET45
IIS-NetFxExtensibility45}

features.each do |f|
windows_feature f do
action :install
end
end
```

由于 Chef 的声明式语言，前面的代码段读起来很自然。我们有一个功能列表。对于其中的每个功能，我们知道它们是 Windows 功能，请安装它们。

由于这种高级抽象，您的食谱可以描述正在发生的事情，而不包含执行实际工作所需的所有逻辑。如果您查看 windows cookbook，您会看到有许多使用 DISM、PowerShell 和 ServerManagerCmd 的实现。与其在食谱本身中担心这个问题，不如将逻辑推迟到为给定资源选择的提供程序。功能资源知道，如果主机有 DISM，它将使用 DISM 提供程序；否则，它将查找 servermanagercmd.exe 是否存在，如果存在，则将其用作安装提供程序。这使得食谱更具表现力，并且杂乱程度大大降低。

如果 Chef 不提供这种高级抽象，您的食谱将更像以下代码片段：

```
features = %w{IIS-ISAPIFilter IIS-ISAPIExtensions
               NetFx3ServerFeatures NetFx4Extended-ASPNET45
               IIS-NetFxExtensibility45}

features.each do |f|
  if ::File.exists?(locate_cmd('dism.exe'))
    install_via_dism(f)
  elsif ::File.exists?(locate_cmd('servermanagercmd.exe'))
    install_via_servermgrcmd(f)
  else
    fail
  end
end

def install_via_dism(feature_name)
  ## some code here to execute DISM
end

def install_via_servermgrcmd(feature_name)
  ## some code here to execute servermgrcmd.exe
end
```

这虽然可以理解，但会显著增加食谱的整体复杂性并降低可读性。现在，您不仅仅专注于安装功能，食谱中还包含了大量关于如何执行安装的逻辑。现在，想象一下编写一个需要创建文件并设置这些文件所有权，并且可以在多个平台上使用的食谱。如果没有抽象，食谱将包含如何确定平台是 Windows 还是 Linux、如何从字符串表示中确定用户或组 ID、不同平台上的文件权限是什么样子等实现细节。但是，使用 Chef 提供的抽象级别，该食谱将如下代码片段所示：

```
file_names = %w{hello.txt goodbye.txt README.md myfile.txt}
file_names.each do |file_name|
```





### 处理多个平台

Chef 的优势之一是能够将 Windows 主机与非 Windows 主机集成。不仅要开发能够同时支持 Linux 和 Windows 主机的食谱和 cookbook，还要在将其部署到基础设施之前对其进行彻底测试，这一点至关重要。让我们来看看如何在食谱中支持多个平台，以及如何使用流行的测试框架 ChefSpec 来编写测试，以测试食谱并验证特定于平台的行为。

### 在 cookbook 中声明支持

所有 Chef cookbook 都有一个 metadata.rb 文件；该文件概述了依赖项、所有权、版本数据和兼容性。在同构环境中，兼容性是一个不太重要的属性——所有主机都运行相同的操作系统。当你建模异构环境时，描述兼容性的能力就更加重要；如果没有它，你可能会尝试将仅适用于 Windows 的食谱应用到 Linux 主机上，或者反之亦然。为了指示 cookbook 支持的平台，你需要向 metadata.rb 文件添加一个或多个 supports 语句。例如，支持 Debian 和 Windows 的 cookbook 将有如下两个 supports 语句：

```
supports "windows"
supports "debian"
```

但是，如果你要支持大量不同的平台，可以始终对配置进行脚本化。例如，你可以使用类似于以下代码片段的内容：

```
%w(windows debian ubuntu redhat fedora).each |os|
  supports os
end
```

### 多平台食谱

在以下代码示例中，我们将探讨如何在单个食谱中同时在 Windows 和 Debian 系统上安装 Apache（一种流行的 Web 服务器）：

```
if platform_family? 'debian'
  package 'apache2'
elseif platform_family? 'windows'
  windows_package node['apache']['windows']['service_name'] do
    source node['apache']['windows']['msi_url']
    installer_type :msi
    # The last four options keep the service from failing
    # before the httpd.conf file is created
    options %W[
      /quiet
      INSTALLDIR="#{node['apache']['install_dir']}"
      ALLUSERS=1
      SERVERADMIN=#{node['apache']['serveradmin']}
      SERVERDOMAIN=#{node['fqdn']}
      SERVERNAME=#{node['fqdn']}
    ].join(' ')
  end
end
template node['apache']['config_file'] do
  source "httpd.conf.erb"
  action :create
  notifies :restart, "service[apache2]"
end
## The apache service
service "apache2" do
  if platform_family? 'windows'
    service_name "Apache#{node['apache']['version']}"
  end
  action [ :enable, :start ]
end
```

在此示例中，我们在主机上执行 Apache 2.x 服务的非常基本的安装。没有启用任何模块，没有虚拟主机，也没有其他内容。但是，它确实允许我们定义一个食谱，该食谱将安装 Apache，生成一个 `httpd.conf` 文件，然后启用并启动 Apache 2 服务。你会注意到这里有一点特定于平台的配置，首先是如何安装软件包，其次是如何启用服务。

由于 package 资源不支持 Windows，因此 Windows 上的软件包安装将使用 `windows_package` 资源，而 Debian 主机上则使用 package 资源。

为了使其正常工作，我们需要在安装期间应用一些配置数据；浏览食谱，我们发现需要一个类似于以下代码片段的配置哈希：

```
'config': {
    'apache': {
        'version': '2.2.48',
        'config_file': '/opt/apache2/conf/httpd.conf',
        'install_dir': '/opt/apache2',
        'serveradmin': 'admin@domain.com',
        'windows': {
            'service_name': 'Apache 2.x',
            'msi_url': 'http://some.url/apache2.msi'
        }
    }
}
```

此食谱允许我们的配置在所有节点上保持一致；我们不需要覆盖任何配置值即可在 Linux 或 Windows 主机上使其正常工作。你可能会对自己说“但是，/opt/apache2 在 Windows 上无法工作”。它可以工作，但它会被解释为 Chef 客户端当前工作目录所在驱动器上的 \opt\apache2；因此，如果你从 C: 运行 Chef，它将变成 c:\opt\apache2。通过使配置保持一致，我们不需要构建任何特殊角色来将 Windows 配置与 Linux 配置分开存储。

如果你不喜欢在 Linux 和 Windows 主机上将 Apache 安装在同一目录中，你可以轻松修改食谱，添加一些条件逻辑，如下所示：

```
apache_config_file = if platform_family? 'windows'
    node['apache']['windows']['config_file']
else
    node['apache']['linux']['config_file']
end
```

```
template apache_config_file do
    source "httpd.conf.erb"
    action :create
    notifies :restart, "service[apache2]"
end
```

在此，食谱变得稍微复杂一些，以便适应两个平台，但其好处是具有一个一致的跨平台配置规范。

或者，你可以按原样使用食谱，并创建两个角色，一个用于 Windows Apache 服务器，另一个用于 Linux Apache 服务器，每个角色都有自己的配置。apache_windows 角色可能具有以下覆盖配置：

```
'config': {
    'apache': {
        'config_file': "C:\\Apps\\Apache2\\Config\\httpd.conf",
        'install_dir': "C:\\Apps\\Apache2"
    }
}
```

相比之下，apache_linux 角色可能具有类似于以下代码片段的配置：

```
'config': {
    'apache': {
        'config_file': "/usr/local/apache2/conf/httpd.conf",
        'install_dir': "/usr/local/apache2"
    }
}
```

这种方法的影响是，现在你必须维护特定于平台的单独角色。当主机被配置或正在配置时（无论是通过控制面板还是通过 knife），你都需要记住它是 Windows 主机，因此与之关联的特定 Chef 角色。这会导致由于复杂性增加而可能出现主机配置错误。

### 降低复杂性

使用 Chef 等工具的一些关键目标如下：

- 尽量减少配置的复杂性
- 提高自动化程度
- 保证可重复性

为了实现这些目标，重要的是要确保你不会引入不必要的复杂性。虽然 Chef 为你提供了使用其多级配置覆盖创建极其复杂的配置集的机制，但这很可能会导致麻烦。正如 Eric Allman 所说的一句流行引语：

> "Unix 给你足够的绳子来吊死自己——然后再多几英尺，以确保万无一失。"

### 版本控制和源代码管理

Chef 的工作是存储 cookbook 和配置数据，然后将正确的组合提供给它所知道的主机。Cookbook 维护着自己的版本概念以及其依赖项的版本。出于这个原因，重要的是将它们视为源代码控制项目的工件，并像对待任何其他软件库一样对它们进行适当的版本控制。保持可预测的版本控制方案有助于确保与其他 cookbook 的兼容性，并减少在添加新功能或重构代码时的冲突和错误。

与任何其他软件项目一样，cookbook 版本通常以 `MAJOR.MINOR.PATCH` 的形式表示。主版本号的更改表示公共 API 不向后兼容。例如，弃用或删除公共功能或公共 API 的行为不一致，都是进行主版本更新的充分理由。相比之下，当添加新功能但现有公共 API 向后兼容时，次要版本号会递增。实现新方法或添加对不同服务管理器的支持，都是增加次要版本的充分理由。修补级别在修复错误时递增，因此这些对公共 API 应该是非常小的更改，如果有的话。有关良好版本控制实践的更多信息，请参阅 `http://semver.org`。

此外，你可能会发现自己正在从公共存储库下载 cookbook。将它们提交到内部存储库（公共或私有）可能是有意义的，这样如果你对 cookbook 进行了更改，或者公共存储库消失，你就可以使用 cookbook 代码的一个版本。

对于任何复杂的系统，无论是软件包、工具还是基础设施配置，都可以说同样的话。重要的是看看你在哪里可以简化组件，以便它们不仅现在容易理解，而且将来由其他工程师（甚至可能是你自己在凌晨 3 点试图拼命修复某个问题时）也能轻松理解。

在可能的情况下，保持配置一致并依靠食谱来区分应用程序的配置方式是个好主意。请记住，保持对分散配置片段的心理模型通常比阅读代码要复杂得多。





一些组织将所有的 cookbook 都保存在一个仓库中；有些组织则保留多个仓库，每个 cookbook 对应一个。无论你选择哪种版本控制系统和仓库策略，将 cookbook 保存在源代码控制系统中，都是确保配方一致部署的关键部分。在本地进行更改，发出 `knife cookbook upload` 命令，然后忘记提交更改，这非常容易，结果可能会被其他人在稍后日期擦除。此外，在修改 cookbook 时，请确保递增版本号，即使它们仅供内部使用，以便清楚地识别 cookbook 的变化。

如果你计划将配方发布到公共领域，你可能会采用每个仓库一个 cookbook 的方法，使 cookbook 成为一个完整且独立的实体。同样，最好确保包含某种许可证信息（通常以 `LICENSE` 文件的形式）以及某种 `README` 文件，其中包含有关 cookbook 的有用信息、示例用法、如何贡献等等。

### 测试配方

测试配方有多种方法；最简单的方法是将它们安装到 Chef 服务器上，并在测试主机上运行。只要你的客户端运行不会花费过长时间，这是一种可行的方法。然而，也许不出所料，还有更好的选择。其中一种选择是使用名为 `ChefSpec` 的库，它提供了类似于 `RSpec` 的功能，用于测试你的 cookbook。

### RSpec 和 ChefSpec

对于那些没有使用过 `RSpec` 的人来说，它是一个用于测试 Ruby 代码的框架，允许你使用特定领域语言来提供测试。这在概念上类似于 Chef 用于管理基础设施的特定领域语言。`RSpec` 和 `ChefSpec` 提供了一系列语言原语，用于表达代码执行、期望和外部组件的模拟。

### 测试基础

如果你是软件测试的新手，特别是测试 Ruby 代码，这是对我们将要介绍的一些概念的简要介绍。测试可以在软件生命周期的许多不同级别上进行，如下所示：

- 单模块级别（称为单元测试）
- 多模块级别（称为功能测试）
- 系统级测试（也称为集成测试）

在 **测试驱动开发 (TDD)** 哲学中，测试被编写和执行得早且频繁，甚至在编写代码之前。这保证了，假设你的测试是准确的，你的代码从一开始就符合你的期望，并且不会退回到不符合要求的先前状态。本节不会深入探讨 TDD 哲学和持续测试，而是为你提供足够的知识，以开始测试你编写的配方，并对它们在部署到生产环境时能做正确的事情感到有信心。

### RSpec

如前所述，RSpec 旨在提供一种更具表现力的测试语言。这意味着所提供的方法和原语被有意地设计为协同工作，以构建一种更高级的语言，感觉更像自然语言，例如英语。例如，使用 RSpec，可以编写以下代码：

`factorial(4).should eq 24`

如果你读一下，感觉就像“四的阶乘应该等于 24”。这是 RSpec 的 DSL 的目标，在阅读测试时感觉自然。将其与类似的 JUnit 测试（用于 Java）进行比较，如下所示：

`assertEquals(24, factorial(4));`

虽然大多数程序员都能读懂，但它不那么自然。除此之外，RSpec 的 DSL 具有上下文和活动块，使测试更易于阅读。例如，我们可以使用 RSpec 如下：

`describe Array do`
`  it "should be empty when created" do`
`    Array.new.should == []`
`  end`
`end`

同样，与类似的 NUnit (.NET 示例) 相比：

`namespace MyTest {`
`  using System.Collection`
`  using NUnit.Framework;`
`  [TestFixture]`
`  public class ArrayTest {`
`    [Test]`
`    public void NewArray() {`
`      ArrayList list = new ArrayList();`
`      Assert.AreEqual(0, list.size());`
`    }`
`  }`
`}`

很明显，RSpec 测试要简洁得多，也更易于阅读，这是 RSpec 的目标。ChefSpec 测试将 RSpec 的表现力带到 Chef cookbook 和配方中，通过在 RSpec 简单的测试语言之上提供 Chef 特定的原语和机制。

### ChefSpec

ChefSpec，对于那些没有使用过它的人来说，是任何 Chef 开发者工具集中的一个绝佳补充。它易于使用，并提供了一种方便的方式来为你的 cookbook 和配方添加测试。

### 开始使用 ChefSpec

为了开始使用 ChefSpec，你需要安装一个包含 ChefSpec 库和所有支持组件的 gem。不出所料，该 gem 名为 chefspec，可以通过运行以下命令简单安装：

`gem install chefspec`

然而，由于正在安装许多其他依赖项，这里有一个 Gemfile，用于将 gem 锁定到编写这些示例时使用的版本：

```
source 'https://rubygems.org'

gem 'chef',
    '11.10.0'
gem 'chefspec',
    '3.2.0'
gem 'colorize',
    '0.6.0'
```

为了使用 Gemfile，你需要安装 bundler；如果你使用 RVM，bundler 应该随你创建的每个 gem 集一起安装。如果没有，你需要自己通过键入以下代码行来安装它：

`gem install bundler`

一旦 bundler 安装完成，并且包含上述代码行的 Gemfile 放置在你的 cookbook 根目录中，你可以从 cookbook 目录内执行以下命令：

`bundle install`

Bundler 将解析 Gemfile 以下载和安装 chefspec、chef 和 colorize，以及你尚未拥有的任何依赖项。安装完成后，你将希望在 cookbook 内创建一个 spec 目录，并创建一个 default_spec.rb 文件。spec 文件的名称应与配方文件的名称匹配，因此如果你有一个名为 default.rb 的配方文件（大多数 cookbook 都会有），你将把 spec 文件命名为 default_spec.rb。让我们来看看一个非常简单的配方和匹配的 ChefSpec 测试。

如这里定义的配方，将简单地在目标主机上创建一个位于 /tmp/myfile.txt 的文件，如下所示：

```
file "/tmp/myfile.txt" do
  owner "root"
  group "root"
  mode "0755"
  action :create
end
```

如以下代码片段所示的匹配测试，将验证我们的 cookbook 是否会按我们期望的那样工作：

```
require 'chefspec'

describe 'demo_cookbook::default' do
  let(:chef_run) {
    ChefSpec::Runner.new.converge(described_recipe)
  }

  it 'creates a file' do
    expect(chef_run).to create_file('/tmp/myfile.txt')
  end
end
```

### 执行测试

现在，为了运行它，我们使用 `rspec` 命令，它将使用 RSpec 语言将测试作为 Ruby 脚本运行。在这里，它还将使用 ChefSpec 扩展，因为在我们的 spec 测试中，我们在顶部有 `require 'chefspec'` 行来包含 ChefSpec。以下示例使用 bundler 运行 RSpec，以便在执行前加载所有必需的 Ruby gem：

`bundle exec rspec spec/default_spec.rb`

这将使用 bundler 运行 RSpec，它将确保加载 Gemfile 中指定的正确版本的 gem，并处理 `default_spec.rb` 文件。运行后，你应该看到测试结果，它们应该类似于以下输出：

```
Finished in 0.17367 seconds
1 example, 0 failures
```

这是 RSpec 告诉你它已完成，并且你有一个测试，零个失败。然而，如果我们有一个失败的测试，结果就会大不相同；RSpec 会告诉我们哪个测试失败了以及原因。

### 理解失败

RSpec 非常擅长告诉你测试中出了什么问题；如果无法确定出了什么问题，有失败的测试对你也没有好处。当测试中的期望未得到满足时，RSpec 会告诉你哪个期望未得到满足以及意外的值是什么。例如，让我们从以下代码中获取我们的演示配方文件资源：

```
file "/tmp/myfile.txt" do
```

让我们将其替换为不同的文件名，例如 myfile2.txt 而不是 myfile.txt，如以下代码所示：

```
file "/tmp/myfile2.txt" do
```

现在，如果我们重新运行 spec 测试，我们会发现测试现在失败了，因为 Chef 做了一些 spec 测试未预期的事情，如以下示例所示：

```
[user@host]$ bundle exec rspec spec/default_spec.rb
F

Failures:

1) demo_cookbook::default creates a file
   Failure/Error: expect(chef_run).to create_file('/tmp/myfile.txt')
     expected "file[/tmp/myfile.txt]" with action :create to be in Chef
   run. Other file resources:

   file[/tmp/myfile2.txt]

   # ./spec/default_spec.rb:9:in `block (2 levels) in <top (required)>'

Finished in 0.18071 seconds
1 example, 1 failure
```

在前面的示例中，你可以看到 RSpec（与 ChefSpec 配合使用）正在告诉我们，'demo_cookbook::default' 测试套件中的 'creates a file' 测试失败了。它还告诉我们，它在 default_spec.rb 的第 9 行失败了（由包含 ./spec/default_spec.rb:9 的行指示），因为它期望文件资源 /tmp/myfile.txt 被 :create 操作交互，但相反，配方与一个文件资源 /tmp/myfile2.txt 进行了交互。





### 扩展你的测试

ChefSpec 提供了一套全面的工具用于测试你的食谱；你可以存根和模拟资源（用人工行为替换其真实行为），模拟不同平台，等等。让我们看看一些更复杂的示例，看看我们还可以用 ChefSpec 做些什么。让我们看一个假设的示例，用于模拟在 Windows 2012 上安装 MySQL，以及在这样的运行过程中我们可能想要验证的一些事情。以下代码示例演示了在 Windows 2012 主机上执行我们的食谱时的测试：

```
context 'when run on Windows 2012' do
  let(:chef_run) do
    # construct a 'runner' (simulate chef-client) running
    # on a Windows 2012 host
    runner = ChefSpec::ChefRunner.new(
      'platform' => 'windows',
      'version' => '2012'
    )
    # set a configuration variable
    runner.node.set['mysql']['install_path'] = 'C:\\temp'
    runner.node.set['mysql']['service_user'] = 'SysAppUser'
    runner.converge('mysql::server')
  end

  it 'should include the correct Windows server recipe' do
    chef_run.should include_recipe 'mysql::server_windows'
  end

  it 'should create an INI file in the right directory' do
    ini_file = "C:\\temp\\mysql\\mysql.ini"
    expect(chef_run).to create_template ini_file
  end
end
```

通过使用平台和版本选项构建 runner，测试将执行运行 `mysql::server` 食谱，并假装它正在 Windows 2012 主机上运行。这使我们可以设置关于将要创建的模板、正在执行的食谱以及在该特定平台上的更多内容的预期。假设 `mysql::server` 食谱将在给定平台上委托给特定于操作系统的食谱，我们可以编写另一个测试，如下所示：

```
context 'when run on Debian' do
  let(:chef_run) do
    runner = ChefSpec::ChefRunner.new(
      'platform' => 'debian'
    )
    runner.node.set['mysql']['install_path'] = '/usr/local'
    runner.node.set['mysql']['service_user'] = 'mysql'
    runner.converge('mysql::server')
  end

  it 'should include the correct Linux server recipe' do
    chef_run.should include_recipe 'mysql::server_linux'
  end

  it 'should create an INI file in the right directory' do
    ini_file = "/usr/local/mysql/mysql.ini"
    expect(chef_run).to create_template ini_file
  end

  it 'should install the Debian MySQL package' do
    expect(chef_run).to install_package('mysql-server')
  end
end
```

这样，我们可以编写测试来验证我们可能无法直接访问的平台上的预期行为，以确保它们将为一系列平台执行预期的操作。带有 ChefSpec 扩展的 RSpec 为我们提供了强大的工具来测试我们的 cookbook 和食谱，这是迈向自动化管理的关键一步。

### 总结

到此为止，你已经了解了如何管理 Windows 主机、自动化部署和配置 IIS 应用、将基础设施扩展到云端，以及测试你的食谱和 cookbook。从中，你可以利用所学知识，开始开发高级的 Windows 特定食谱和支持多个平台的跨平台食谱，并使用 RSpec 和 ChefSpec 对它们进行测试。使用 Chef，你可以轻松管理异构的 Windows 和 Linux 环境，将你的系统扩展到物理主机和云服务器，以满足基础设施的增长需求。
