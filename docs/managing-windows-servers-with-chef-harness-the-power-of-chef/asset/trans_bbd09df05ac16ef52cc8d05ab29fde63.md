

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

## 终止实例

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

## 删除 Chef 节点

此时，Rackspace 实例正在被终止并从您的帐户中删除。但是，它未从 Chef 服务中删除；这需要使用 `node delete` 命令单独完成。在以下代码中，指定的是 Chef 节点名称，而不是实例标识符：

```
$ knife node delete rs-5889646228538071
```

使用以下命令验证节点是否已从 Chef 中删除：

```
$ knife node list
0c0ff3300
```

## 摘要

如您所见，Chef 对从命令行管理云服务器提供了强大的支持。您可以将云主机与物理基础架构相结合，使用 Chef 使您的系统具有可扩展性和弹性，这在以前可能无法实现。这些只是当今市场上最流行的三种云服务；Chef 对各种其他基础架构提供商也有越来越多的支持，并且随着时间的推移，此列表将不断增长。

现在您已经学习了如何编写食谱并使用流行的云服务扩展基础架构，让我们在下一章中看看一些高级主题，包括在 Chef 中测试食谱和多平台支持。

## 超越基础

现在您已经了解了如何将 Chef 集成到 Windows 环境中，让我们看看一些高级主题，如下所示：

- 管理异构网络
- 处理多个平台
- 版本控制和源代码管理
- 使用 ChefSpec 测试各种平台的食谱

### Chef 的声明式语言

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

