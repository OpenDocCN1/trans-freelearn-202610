

| 级别 | 信息 |
|---|---|
| DisplayStatus | 预配成功 |
| Message |  |
| Status | 成功 |
| Capacity | 0 |
| Count | 0 |

虽然这种方法在我们的示例环境中没问题，但它会尝试对订阅中的所有 VM 运行你的 PowerShell 脚本。在正常的 Azure 渗透测试期间，你可能希望将命令执行集中在单个 VM 上。

这可以通过指定单个 VM 和资源组，或使用我们之前创建的 VM 变量的索引来实现。请记住，在 PowerShell 中索引从 0 开始，因此我们可以将 `$VMs[0]` 传递给同一个 `Invoke-AzRunCommand` 函数，以对列表中的第一台 VM 运行命令。

## 从 Azure REST API 运行命令

这可能是在 VM 上运行命令最困难的方式，但幸运的是，有一些脚本可以帮助我们简化此过程。虽然这有点偏离我们标准的已认证 Contributor 用户流程，但我们可能会遇到这样的情况：我们拥有某个托管标识的令牌，并且该标识具有 Contributor 角色。

如前一章所述，如果我们能访问具有托管标识的 VM，就可以为该标识生成一个访问令牌，该令牌可用于管理 API。然后，可以将此令牌与 MicroBurst 中的脚本结合使用，以在其他 VM 上运行命令。

下面是一个示例命令，用于获取可访问 Azure 管理 API 端点的访问令牌：

```bash
root@LinuxVM:~# curl -H Metadata:true -s 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' | jq
```

以下是命令及其输出的截图：

```text
root@LinuxVM:~#
root@LinuxVM:~# curl -H Metadata:true -s 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' | jq
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIn..."
}
```

图 5.14 – 使用 curl 查询元数据服务

获取令牌后，可以利用 MicroBurst 工具包中 REST 部分的 `Invoke-AzVMCommandREST` 命令，使用所获取的令牌来运行命令。该函数会提示你选择订阅以及要在其上运行命令的 VM。由于该命令通过 REST API 运行，因此我们无法看到命令的结果。要查看命令的直接结果，我们需要强制 VM 回调我们，或者创建一些证据来证明我们在该机器上运行了命令（例如 `whoami > test.txt`）。下面是一个使用此方法的示例：

```powershell
PS C:\> $mgmtToken = "TOKEN GOES HERE"
PS C:\> Invoke-AzVMCommandREST -commandToExecute "whoami > test.txt" -managementToken $mgmtToken
204cce89-27de-4669-a48b-04c27255e05e
Executing command on target VM: RunCommandTestVM
```

运行此命令后，我们可以前往该 VM，在 VM 上查找 `test.txt` 文件，以证明命令已经执行。在典型的测试中，我们更有可能使用这种命令执行方式来启动来自 VM 的反向 shell 连接。

从实际渗透测试的角度来看，这种命令执行方式可能是最有用的方法之一。随着越来越多的组织将其传统应用程序迁移到云端，将会有更多机会利用应用程序服务器并从 VM 和 App Service 主机收集访问令牌。

## Lava 简介

Lava 是一个基于 Python 的 Microsoft Azure 利用框架。它内置了多个可用于利用 Azure 中各种错误配置的模块。本书的作者之一定期为该项目做贡献。在下一个动手练习中，我们将使用它来利用具有特权的 VM 进行角色提升。

**Lava 信息**

- **作者**：Matt Rotlevi（Twitter：@mattrotlevi）
- **开源或商业**：开源项目
- **GitHub 仓库**：https://github.com/mattrotlevi/lava
- **语言**：Python

## 动手练习 – 使用 Lava 利用特权 VM 资源

在本练习中，我们将在渗透测试 VM 的 WSL 中下载 Lava，使用它查找 Azure 订阅中具有特权的 IaaS 资源，然后将权限从 Contributor 角色提升为 Owner 角色。

以下是我们将在本练习中完成的任务：

- 使用 WSL 中的 Azure CLI 工具进行 Azure 身份验证。
- 在 WSL 中下载 Lava。
- 使用 Lava 模块获取订阅中特权 VM 的列表。
- 使用 Lava 模块利用具有 Owner 角色分配的 VM。

让我们开始吧！

1. 在渗透测试 VM 上打开 PowerShell。

2. 在 PowerShell 控制台中，输入 `bash` 切换到 Windows Subsystem for Linux（WSL）。

3. 使用以下命令切换到 root 用户。这执行我们想要执行的安装任务时需要。在提示时输入管理员密码：

```text
azureuser@PentestVM:~# sudo su -
```

4. 使用 `az login` Azure CLI 命令进行 Azure 身份验证：

```text
root@PentestVM:~# az login
```

5. 在提示时，输入你在本章第一个练习的脚本输出中获得的 Contributor 凭据：

![](img/4fa338e84e0a590967a142dd798ba4e2_205_0.png)

图 5.15 – az login 密码提示

你现在已使用 Contributor 凭据完成身份验证。

6. 使用以下命令下载 Lava：

```bash
root@PentestVM:~# git clone https://github.com/mattrotlevi/lava.git
```

以下是命令及其输出的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_205_1.png)

图 5.16 – 克隆 Lava GitHub 仓库

7. 使用以下命令切换到 Lava 目录并运行它：

```bash
root@PentestVM:~# cd lava/
root@PentestVM:~# python3 lava.py
```

看起来如下：

![](img/4fa338e84e0a590967a142dd798ba4e2_206_0.png)

图 5.17 – Lava 初始化截图

8. 在 Lava 控制台中，使用 `whoami` 命令确认已认证用户：

```text
Lava $> whoami
```

以下是命令及其输出的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_206_1.png)

图 5.18 – Lava whoami 命令的结果

9. 使用以下命令列出 Lava 中的所有模块：

```text
Lava $> ls
```

看起来如下：

![](img/4fa338e84e0a590967a142dd798ba4e2_207_0.png)

图 5.19 – Lava ls 命令的结果

Lava 有许多模块，可以在不同场景下利用资源，但在本章中，我们将使用两个主要模块：`vm_list_privileged` 和 `vm_rce`。

10. 使用 `vm_list_privileged` 模块检查订阅中是否有任何 VM 与特权托管标识关联。要在 Lava 中执行模块，命令前需要加上 `exec` 关键字：

```text
exec vm_list_privileged
```

以下是命令及其输出的截图。从截图中可以看到，该模块找到了一台 VM，但该 VM 没有关联的托管标识（1）。你还可以看到，该模块找到了另一台与特权托管标识关联的 VM（2）。该模块还显示了分配给 VM 的角色以及角色分配范围（3）：

![](img/4fa338e84e0a590967a142dd798ba4e2_208_0.png)

图 5.20 – Lava vm_list_privileged 模块输出

总的来说，该模块应列出两台具有特权角色分配的 VM（假设你已删除了上一章的 VM）：一台在订阅范围内具有 Owner 角色分配的 Linux VM，以及一台具有 Contributor 角色分配的 Windows VM。请注意被发现具有 Owner 权限的 Linux VM 的名称（linuxvm01）和资源组（pentest-rg）。由于我们已经拥有分配了 Contributor 角色的凭据，因此我们将专注于利用具有 Owner 角色分配的 VM 来获取其令牌，以实现权限提升。

11. 使用 `vm_rce` 模块以 Contributor 身份利用 Run Command 功能。你需要指定 VM 名称和资源组，如下所示：

```text
Lava $> exec vm_rce -rgrp PENTEST-RG -vm_name linuxvm01
```

