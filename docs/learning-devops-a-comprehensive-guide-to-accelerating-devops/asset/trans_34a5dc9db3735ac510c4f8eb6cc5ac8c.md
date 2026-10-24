

## 创建 Ansible 清单

在下一节中，我们将详细介绍静态 Ansible 清单以及如何创建它以定位主机。

清单包含 Ansible 将执行管理和配置操作的主机列表。

清单有两种类型：

- **静态清单**：主机以 INI（或 YAML）格式列在文本文件中；这是 Ansible 清单的基本模式。当我们知道主机地址（IP 或 FQDN）时，会使用静态清单。

- **动态清单**：主机列表由外部脚本动态生成（例如，使用 Python 脚本）。如果我们没有主机地址，例如，在由按需环境组成的基础架构中，则会使用动态清单。

在本节中，我们将学习如何以 `init` 格式创建静态清单，从基本示例开始，然后我们将查看组和主机配置。

让我们先学习如何创建静态清单文件。

## 清单文件

为了在运行 playbook 时配置主机，Ansible 需要一个包含主机列表的文件；也就是说，目标机器的 IP 或 **完全限定域名（FQDN）** 地址列表。此主机列表记录在名为 **清单文件** 的静态文件中。

默认情况下，Ansible 在安装过程中包含一个创建的清单文件；该文件是 `/etc/ansible/hosts`，其中包含多个清单配置示例。在我们的案例中，我们将手动在我们选择的目录中创建并填充此文件，例如 `devopsansible`。

让我们一步步来：

1. 首先，我们必须使用以下基本命令创建目录：

```bash
mkdir devopsansible
cd devopsansible
```

2. 现在，让我们创建一个名为 myinventory（不带扩展名）的文件，其中我们将写入目标主机的 IP 地址或 FQDN，如以下示例所示：

```
192.10.14.10
mywebserver.entreprise.com
localhost
```

当基于此清单执行 Ansible 时，它将在此清单中提到的所有主机上执行所有请求的操作（playbook）。

> **重要说明**
> 有关清单文件的更多信息，请阅读 https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html 上的文档。

但是，在企业中使用 Ansible 时，相同的 Ansible 代码（或 playbook）包含为应用程序的所有 VM 执行的配置操作。由于这些 VM 在应用程序中具有不同的角色，例如，应用程序由一个（或多个）Web 服务器和一个数据库服务器组成，我们必须将清单分开以按功能角色分离 VM。

要在清单中按角色对 VM 进行分组，我们将 VM 组织到将在 [] 中标记的组中，这给我们提供了以下清单：

```
[webserver]
192.10.20.31
mywebserver.exemple.com
[database]
192.20.34.20
```

在此示例中，我们定义了两个组，webserver 和 database。所有主机都分布在这些组中。

作为另一个示例，我们还可以按环境对主机进行分组，使用此示例清单：

```
[dev]
192.10.20.31
192.10.20.32
[qa]
192.20.34.20
192.20.34.21
[prod]
192.10.12.10
192.10.12.11
```

在本章后面，我们将学习这些组将如何在 playbook 编写中使用。

现在，让我们学习如何通过配置主机来完成我们的清单。

## 在清单中配置主机

如我们所见，整个 Ansible 配置都在 `ansible.cfg` 文件中。但是，此配置是通用的，适用于所有 Ansible 执行，以及与主机的连接。

但是，当使用 Ansible 配置来自不同环境或角色且具有不同权限的 VM 时，拥有不同的连接配置很重要，例如每个环境的不同管理用户和 SSL 密钥。出于此原因，可以通过按主机配置特定参数来在清单文件中覆盖默认的 Ansible 配置，如在此清单中定义。

可以覆盖的主要配置参数如下：

- `ansible_user`：这是连接到远程主机的用户。
- `ansible_port`：可以更改 SSH 端口的默认值。
- `ansible_host`：这是主机的别名。
- `ansible_connection`：这是到远程主机的连接类型，可以是 Paramiko、SSH 或 local。
- `ansible_private_key_file`：这是用于连接到远程主机的私钥。

> **重要说明**
> 参数的完整列表可在以下文档中找到：
> https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#list-of-behavioral-inventory-parameters。

## 测试清单

编写清单后，可以测试是否可以从 Ansible 访问提到的所有主机。为此，我们可以执行以下命令：

```bash
ansible -i inventory all -u demobook -m ping
```

`-i` 参数是清单文件的路径，`-u` 参数对应于用于连接到远程机器的远程用户名，`-m` 是要执行的命令。在这里，我们在清单中的所有机器上执行 `ping` 命令。

以下是我们配置了主机连接的清单示例：

```
[webserver]
webserver1 ansible_host=192.10.20.31 ansible_port=2222
webserver2 ansible_host=192.10.20.31 ansible_port=2222
[database]
database1 ansible_host=192.20.34.20 ansible_user=databaseuser
database2 ansible_host=192.20.34.21 ansible_user=databaseuser
[dev]
webserver1
database1
[qa]
webserver2
database2
```

在此清单示例中可以看到以下内容：

- 连接信息已在每个主机旁指定。
- 别名实现（例如 `webserver1` 和 `webserver2`）用于另一个组（例如本示例中的 `qa` 组）。

在实现了 Ansible 清单后，我们现在将学习如何测试此清单。

以下屏幕截图显示了此命令的执行：

![](img/d3344b39c888aab0b832625465f837c9_104_0.png)

图 3.6 – 使用 all 选项测试 ansible ping 命令

我们还可以通过使用组名而不是 `all` 来调用此命令，测试特定组中主机的连接性。例如，在我们的案例中，我们将执行此命令：

`ansible -i inventory webserver -u demobook -m ping`

以下屏幕截图显示了此命令的执行：

![](img/d3344b39c888aab0b832625465f837c9_104_1.png)

图 3.7 – 针对特定主机测试 ansible ping 命令

在本节中，我们了解到 Ansible 需要一个清单文件来配置主机。然后，我们创建并测试了我们的第一个清单文件，然后再学习如何进一步配置此文件。

在下一节中，我们将学习如何在 Ansible playbooks 中设置和编写配置操作代码。

## 执行第一个 playbook

Ansible 的基本元素之一是它的 playbooks，因为如在介绍中规定的那样，它们包含配置或管理 VM 所需执行的操作或任务的代码。

确实，一旦 VM 已配置，就必须对其进行配置，并且必须安装在该 VM 上托管的应用程序所需的所有中间件。此外，有必要执行有关目录配置及其访问的管理任务。

在本节中，我们将了解 playbook 由什么组成，它的模块，以及如何使用角色来改进我们的 playbook。

现在，让我们开始学习如何编写基本 playbook。

## 编写基本 playbook

playbook 的代码是用 YAML 编写的，YAML 是一种声明性语言，允许我们轻松可视化配置步骤。

为了了解 playbook 的样子，让我们看一个简单而经典的示例；也就是说，在 Ubuntu VM 上安装 NGINX 服务器。之前，我们创建了一个工作目录 devopsansible，我们将在其中创建一个 playbook.yml 文件并插入以下内容代码：

```yaml
---
- hosts: all
tasks:
- name: install and check nginx latest version
  apt: name=nginx state=latest
- name: start nginx
  service:
    name: nginx
    state: started
---
```

让我们详细看看：

- 首先，YAML 文件以可选的 -- 字符开始和结束。

- `- hosts` 属性包含要配置的主机列表。在这里，我们将此属性的值写为 `all` 以在我们清单中列出的所有 VM 上安装 NGINX。如果我们想仅在特定组上安装它，例如，在 webserver 组上，我们将按如下方式记录：

```yaml
---
- hosts: webserver
```

