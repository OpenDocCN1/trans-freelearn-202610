

> 注意
> 在本节中，我们学习了如何使用 Vagrant 配置文件创建 VM。如果希望在同一配置文件中创建多个 VM，请参阅官方文档 https://www.vagrantup.com/docs/multi-machine。

我们刚刚讨论了如何选择 Vagrant box 以及如何编写 Vagrant 配置文件。现在，在下一节中，我们将学习如何使用 Vagrant CLI 和配置文件在本地创建 VM。

## 使用 Vagrant CLI 创建本地 VM

既然我们已经编写了配置文件，就可以在本地创建 VM 了。

为此，我们将使用多个 Vagrant CLI 命令。要显示所有可用命令，我们将运行 `vagrant --help` 命令：

```
PS C:\Learning-DevOps-Second-Edition\CHAP05\VagrantFiles> vagrant --help
Usage: vagrant [options] <command> [<args>]

-h, --help Print this help.

Common commands:
 autocomplete manages autocomplete installation on host
 box manages boxes: installation, removal, etc.
 cloud manages everything related to Vagrant Cloud
 destroy stops and deletes all traces of the vagrant machine
 global-status outputs status Vagrant environments for this user
 halt stops the vagrant machine
 help shows the help for a subcommand
 init initializes a new Vagrant environment by creating a Vagrantfile
 login
 package packages a running vagrant environment into a box
 plugin manages plugins: install, uninstall, update, etc.
 port displays information about guest port mappings
 powershell connects to machine via powershell remoting
 provision provisions the vagrant machine
 push deploys code in this environment to a configured destination
 rdp connects to machine via RDP
 reload restarts vagrant machine, loads new Vagrantfile configuration
 resume resume a suspended vagrant machine
 snapshot manages snapshots: saving, restoring, etc.
 ssh connects to machine via SSH
 ssh-config outputs OpenSSH valid configuration to connect to the machine
 status outputs status of the vagrant machine
 suspend suspends the machine
 up starts and provisions the vagrant environment
 upload upload to machine via communicator
 validate validates the VagrantFile
 version prints current and latest Vagrant version
 winrm executes commands on a machine via WinRM
 winrm-config outputs WinRM configuration to connect to the machine
```

图 5.9 – 显示 Vagrant 命令

> 注意
> 关于 Vagrant CLI 命令的所有详细信息均记录在 https://www.vagrantup.com/docs/cli。

在下一节中，我们将学习如何创建 VM，然后连接到该 VM，最后执行一些脚本。

## 创建 VM

要创建 VM，请导航到创建 `Vagrantfile` 文件的文件夹，并运行以下命令：

```
vagrant up
```

输出如下截图所示：

![](img/d3344b39c888aab0b832625465f837c9_170_0.png)

执行上述命令将执行以下步骤：

- 从 Vagrant Cloud 导入 box。
- 在 hypervisor 中创建新 VM（在本例中，这是 VirtualBox）。
- 使用 SSH 密钥创建 SSH 连接（Vagrant 创建私有/公共 SSH 密钥）。
- 挂载共享文件夹。
- 应用 provisioning Ansible 脚本。

VM provisioning 完成后，我们可以在 VirtualBox 中的 hypervisor 里查看此 VM。

既然 VM 已启动并完成了 provisioning，我们可以连接到它。

## 连接到 VM

要使用 SSH 连接 VM，可以使用专用于 Vagrant 的默认 SSH 命令（以管理员模式运行）：

```
`vagrant ssh`
```

输出如下截图所示：

```
PS C:\Users\Administrator\Learning-DevOps-Second-Edition\CHAP05\VagrantFiles> vagrant ssh
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-154-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

System information as of Sun Aug 29 14:25:23 UTC 2021

System load:  0.01                Processes:             96
Usage of /:   4.1% of 38.71GB     Users logged in:       0
Memory usage: 17%                 IP address for enp0s3: 10.0.2.15
Swap usage:   0%

0 updates can be applied immediately.

New release '20.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

vagrant@ubuntu-bionic:~$
```

图 5.11 – 连接 SSH 到 Vagrant VM

执行此命令意味着 Vagrant 自动将 SSH 凭据连接到 VM，我们可以在 VM 内部运行任何命令或脚本。

例如，我们可以运行 `ansible --version` 命令以显示已安装的 Ansible 版本：

```
vagrant@ubuntu-bionic: $ ansible --version
ansible 2.9.24
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/vagrant/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.17 (default, Feb 27 2021, 15:10:58) [GCC 7.5.0]
vagrant@ubuntu-bionic: $
```

图 5.12 – 在 VM 内部执行命令

运行所需的测试后，如果我们希望销毁此 VM 以重新创建它，可以运行 `vagrant destroy` 命令：

```
PS C:\Users\<username>\Learning-DevOps-Second-Edition\CHAP05\VagrantFiles> vagrant destroy
default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...
```

图 5.13 – 销毁 Vagrant VM

在本节中，我们学习了如何使用 Vagrant CLI 创建 VM 并连接到它。

## 摘要

在本章中，我们了解到可以使用 HashiCorp 的 Vagrant 在本地创建 VM，以便拥有一个隔离的开发环境。

我们讨论了如何下载和安装它。然后，我们学习了如何使用 Bionic box、一个 `ansible install` 脚本和一个本地共享文件夹编写 Vagrant 配置文件。

最后，我们学习了如何执行 Vagrant 命令行以创建此 VM 并连接到它进行测试。

在下一章中，我们将开始一个新主题，即 CI/CD 的实现，并学习如何使用 Git 来管理您的代码。

## 问题

- Vagrant 的作用是什么？
- 创建 VM 的 Vagrant 命令是什么？
- 连接 SSH 到 VM 的 Vagrant 命令是什么？

## 延伸阅读

如果您想了解更多关于 Vagrant 的信息，请查看以下资源：

- 官方 Vagrant 文档：https://www.vagrantup.com/docs/
- 官方 Vagrant Cloud 文档：https://www.vagrantup.com/vagrant-cloud

## 第 2 节：DevOps CI/CD 管道

本部分涵盖 DevOps 管道流程，从持续集成和持续部署的原则开始。它将通过使用不同的工具进行说明，包括 Jenkins、Azure Pipelines 和 GitLab。

本节包含以下章节：

- 第 6 章，使用 Git 管理您的源代码
- 第 7 章，持续集成和持续部署
- 第 8 章，使用 CI/CD 管道部署基础设施即代码

## 第 6 章 使用 Git 管理您的源代码

几年前，当我们作为开发人员并作为团队的一部分编写代码时，我们遇到了反复出现的问题，大部分问题如下：

- 如何与团队成员共享我的代码
- 如何对我的代码更新进行版本控制
- 如何跟踪我代码的更改
- 如何检索我代码的旧状态或其部分

随着时间的推移，这些问题已通过源代码管理器的出现得到解决，也称为 **版本控制系统 (VCS)** 或更常见地称为 **版本控制管理器 (VCM)**。

这些 VCS 的目标主要是执行以下操作：

- 允许开发人员代码协作
- 检索代码
- 对代码进行版本控制
- 跟踪代码更改


