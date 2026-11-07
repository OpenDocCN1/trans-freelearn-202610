
例如，当尝试在 Horizon 仪表板上创建一个名为 `default` 的新 Neutron 安全组时，你会收到一个类似图 10.7 的错误。

![](img/da864e0670c65edf6f5ab861ef9906b3_313_0.png)

Figure 10.7: 尝试创建额外的默认 Neutron 安全组时生成的带 request-ID 的错误

然后我们可以在 `neutron-server` 日志文件中 `grep` 该特定请求 ID，以查看请求的具体时间：

```
$ sudo grep -nar "req-017786ad-5b66-4b06-9dc5-ed35a9dff22c"
/var/log/neutron/neutron-server.log
```

我们应该会看到类似图 10.8 的输出：

![](img/da864e0670c65edf6f5ab861ef9906b3_313_1.png)

Figure 10.8: 尝试创建额外的 'default' Neutron 安全组时 neutron-server 日志中的错误

![](img/da864e0670c65edf6f5ab861ef9906b3_313_2.png)

你需要使用 `sudo` 或切换为 root (`sudo su -`) 来分析虚拟设备环境中的日志文件。`openstack` 用户没有这些目录的权限。

## 管理 MySQL

作为 OpenStack 管理员，你很可能依赖经验丰富的数据库管理员来处理所有与 OpenStack 相关的数据库维护工作。官方 OpenStack 考试要求中包含一个广泛的目标，即“分析数据库服务器并备份数据库”。以下是使用 MySQL/MariaDB 的一些基础知识，这应该能让你为考试做好准备。

我们可以使用 mysql 命令行工具来探索虚拟设备上的 MySQL。

运行以下命令：

```
$ sudo mysql -h localhost
```

连接后，我们可以运行以下命令查看所有 OpenStack 服务数据库：

```
MariaDB [(none)]> show databases;
```

你应该会看到类似图 10.9 的输出

![](img/da864e0670c65edf6f5ab861ef9906b3_314_0.png)

Figure 10.9: 'show databases' SQL 语句的输出

要使用特定数据库，我们可以运行以下命令：

```
MariaDB [(none)]> use nova;
```

要查看 Nova 数据库中的所有表，可以运行以下命令：

```
MariaDB [(none)]> show tables;
```

如果我们想查看有关虚拟机实例的所有数据，可以运行以下命令：

```
MariaDB [(none)]> select * from instances;
```

要退出 MySQL/MariaDB 提示符，运行以下命令：

```
MariaDB [(none)]> exit;
```

如果我们想对特定数据库执行 SQL 转储，可以运行以下命令：

```
$ sudo mysqldump -h localhost --databases nova > nova.sql
```

要恢复 SQL 转储，运行以下命令：

```
$ sudo mysql -h localhost < nova.sql
```

## 总结

所以就这样！在本节中，我们涵盖了一些通用的 OpenStack 故障排除技能，这些技能将为你准备 COA 考试。记住，故障排除领域仅占考试的约 13%，因此不要期望在这方面有很多问题。虽然这不是考试目标，但你应该了解如何使用 --debug 标志在 CLI 上排除 OpenStack 问题时提供线索。我还向你展示了一些 OpenStack 服务 API 如何让你检查其组件/代理的状态。本节中最重要的考试目标是记住 OpenStack 服务日志文件的位置，以及在识别特定消息时如何 grep 它们。最后，我们以一些非常基础的 MySQL/MariaDB 技能结束了本章，包括显示特定数据库的表、备份和恢复数据库。

## 11 最后的技巧和窍门

恭喜！到目前为止，我希望你对参加 COA 考试感到信心十足。以下是进入考试前需要牢记的一些实用技巧和窍门。

在本章中，我们将涵盖以下主题：

- 项目范围
- 时间管理
- CLI 帮助
- 外部监视器
- 考试期间的问题
- 深入 OpenStack

在回顾这些技巧后，你应该准备好参加考试。务必在参加正式考试前查看第 12 章中的模拟考试！

## 项目范围

项目范围是任何 OpenStack 环境中最基本的方面。如果你在 Horizon 仪表板上尝试某个目标，请不要忘记始终从顶部下拉菜单中将自己限定到正确的项目。参考图 11.1。

如果由于任何原因你在顶部下拉菜单中看不到项目，你应该切换到 CLI。

![](img/da864e0670c65edf6f5ab861ef9906b3_317_0.png)

Figure 11.1: 在 Horizon 仪表板上限定项目

如果你在 CLI 上，将自己限定到新项目最简单的方法是使用本书前几章介绍的方法：

```
$ cp openrc openrc-<project-name>
```

复制原始 openrc 文件后，使用你选择的编辑器填写项目和租户的正确变量：

```
$ nano openrc-<project-name>
```

确保同时设置 OS_PROJECT_NAME 和 OS_TENANT_NAME 变量：

```
export OS_PROJECT_NAME=<project-name>
export OS_TENANT_NAME=<project-name>
```

![](img/da864e0670c65edf6f5ab861ef9906b3_318_0.png)

尽管 python-openstackclient 只需要 OS_PROJECT_NAME 变量，但一些旧客户端可能依赖于旧的 OS_TENANT_NAME 变量。我们在这里设置它以保持一致性。

保存带有新设置变量的文件后，source 该文件以在当前 Bash 会话中设置变量：

```
$ source openrc-<project-name>
```

在运行任何命令之前，验证变量是否已正确设置！

```
$ export | grep OS
```

你应该会看到类似图 11.2 的输出。

![](img/da864e0670c65edf6f5ab861ef9906b3_318_1.png)

Figure 11.2: 继续的 Bash 会话变量设置为正确的项目范围

## 时间管理

要通过 COA，你必须合理管理时间！始终注意考试控制台中剩余的时间。如第 1 章《介绍 OpenStack 和 OpenStack 认证管理员考试》中所述，你可以自由使用官方上游 OpenStack 文档 https://docs.openstack.org/ocata/。然而，我不建议在考试期间依赖它！在网站上搜索答案会浪费你宝贵的时间！如果一个问题似乎花费超过 4–5 分钟才能完成，你应该点击前进按钮并继续！如果还有时间，稍后再回来处理！

如果任务可以在 Horizon 仪表板上完成，通常比 CLI 快得多。如果你是 CLI 高手并且不想挖掘 --help 命令，CLI 可能对你来说更快。

![](img/da864e0670c65edf6f5ab861ef9906b3_319_0.png)

Figure 11.3: 尽管你在考试期间可以自由浏览 docs.openstack.org，但这是在浪费时间！

## CLI 帮助

如果你忘记了某个特定 CLI 命令的工作方式，你始终可以依赖帮助！以下是使用 `python-openstackclient` 帮助的一些示例。

```
dns quota list List quotas
dns quota reset Delete blacklist
dns quota set Set blacklist properties
dns service list List service statuses
dns service show Show service status details
domain create Create new domain
domain delete Delete domain(s)
domain list List domains
domain set Set domain properties
domain show Display domain details
ec2 credentials create Create EC2 credentials
ec2 credentials delete Delete EC2 credentials
ec2 credentials list List EC2 credentials
ec2 credentials show Display EC2 credentials details
endpoint create Create new endpoint
endpoint delete Delete endpoint(s)
endpoint list List endpoints
endpoint set Set endpoint properties
endpoint show Display endpoint details
extension list List API extensions
federation domain list List accessible domains
federation project list List accessible projects
federation protocol create Create new federation protocol
federation protocol delete Delete federation protocol(s)
federation protocol list List federation protocols
federation protocol set Set federation protocol properties
federation protocol show Display federation protocol details
flavor create Create new flavor
flavor delete Delete flavor(s)
flavor list List flavors
flavor set Set flavor properties
flavor show Display flavor details
flavor unset Unset flavor properties
floating ip create Create floating IP
floating ip delete Delete floating IP(s)
floating ip list List floating IP(s)
floating ip pool list List pools of floating IP addresses
floating ip show Display floating IP details
group add user Add user to group
group contains user Check user membership in group
group create Create new group
group delete Delete group(s)
group list List groups
group remove user Remove user from group
group set Set group properties
group show Display group details
help print detailed help for another command
host list List hosts
host set Set host properties
host show Display host details
hypervisor list List hypervisors
hypervisor show Display hypervisor details
hypervisor stats show Display hypervisor stats details
```

Figure 11.4: 'openstack help' 命令的输出

使用帮助是一个 **两步过程**。

第 1 步：使用 `openstack help` 确定如何执行特定操作。
