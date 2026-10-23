

7. 使用以下命令启动 puppet master 服务：

```
# systemctl start puppetmaster.service
# systemctl enable puppetmaster.service
```

8. 验证服务是否正确启动：

```
# systemctl status puppetmaster.service -l
```

你应该会看到服务已创建证书颁发机构。环境中的客户端访问 Puppet master 时必须使用与证书颁发机构相同的 hostname。如果客户端将通过除 hostname 之外的其他方式访问 Puppet master，请查阅 puppet.conf 中的 dns_alt_names 设置。

## 安装 Jenkins

以下说明将在 CI 服务器上设置 Jenkins 服务器。如果你已有可在实验室中使用的 Jenkins 服务器，可以跳过本节。有关设置 Jenkins 的更多信息，请参阅 Jenkins 网站 (https://wiki.jenkins-ci.org/display/JENKINS/Use+Jenkins)。

- 1. 以 root 用户身份在 CI 服务器上执行以下命令以安装 Jenkins 仓库，该仓库提供对 Jenkins 的访问权限：

```
# curl http://pkg.jenkins-ci.org/redhat/jenkins.repo > /etc/yum.repos.d/jenkins.repo
# rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
# yum install -y java-1.7.0-openjdk jenkins
```

- 2. 要验证软件是否正确安装，运行以下命令：

```
# rpm -q java-1.7.0-openjdk jenkins
```

- 3. 要启动 jenkins 服务器，使用以下命令：

```
# systemctl start jenkins
```

- 4. 使用以下命令验证 Jenkins 是否正确启动：

```
# systemctl status jenkins -l
```

你应该会看到服务正在运行，并且日志输出中没有错误。

5. 现在你可以通过在 Web 浏览器中访问主机 8080 端口来登录 Jenkins Web UI；也就是说，如果你的 CI 服务器的 hostname 为 `jenkins.example.com`，Jenkins 应可在 `http://jenkins.example.com:8080/` 上访问。登录后你应会看到以下页面：

![](img/2f752555f69992669defa8b25e6dcee9_93_0.png)

6. 我们需要做的最后一件事是安装 Git SCM 插件。点击左侧导航中的 Manage Jenkins 链接。从下一页选择 Manage Plugins。在 Plugins 页面，选择 Available 选项卡，并在 Filter 栏中搜索 git plugin。选择 Git 插件并点击 Install without restart：

![](img/2f752555f69992669defa8b25e6dcee9_94_0.png)

你将看到安装进度状态。完成后，你会看到类似以下页面：

![](img/2f752555f69992669defa8b25e6dcee9_95_0.png)

Jenkins 现已安装并配置了 Git 插件。

## 创建组合层

现在我们的基础架构软件已就位，我们将开始创建我们的组合层。如前所述，我们将从在第 2 章 *Architecting the Cloud* 中运行 `packstack` 命令时获得的“已知良好”配置开始。

## 启动我们的 Puppet 模块

我们的组合层将存储在两个 Puppet 模块中，使用 Craig Dunn 开发的 **profiles and roles** 模式 (http://www.craigdunn.org/2012/05/239/)。我们将模块存储在新 Git 仓库中名为 `puppet/modules` 的目录中。以下步骤将在我们的仓库中创建新模块：

- 1. 克隆仓库。建议对此部分使用非特权用户帐户：
```
$ git clone http://git@localhost/git/openstack/
```
- 2. 接下来，创建模块的目录：
```
$ mkdir -p openstack/puppet/modules
```
- 3. 使用 `puppet module` 命令在目录中创建新模块：
```
$ cd openstack/puppet/modules
$ puppet module generate openstack-profile
$ puppet module generate openstack-role
```
对于每个命令，你将被提示输入有关模块的一组元数据。输入你想要的尽可能多的信息或采用默认值。
- 4. 设置好模块后，返回到代码仓库的顶层，并使用以下命令将其检入版本控制：
```
$ cd ~/openstack
$ git add puppet
$ git commit -m 'Initial check-in of the profile and role modules'
$ git push origin master
```
你应该会看到模块已提交并推送到仓库上的新 master 分支。
- 5. 现在让我们将这些模块添加到我们的 Puppet master。为此，以 root 用户身份在服务器上的位置克隆仓库的干净副本：
```
# cd /srv
# git clone http://localhost/git/openstack
```

你应该会收到类似 `Cloning into 'openstack'...` 的消息，这将表明命令成功。

- 6. 我们将使用 Jenkins 作业更新此仓库，因此将目录的所有权更改为 Jenkins 用户：

```
# chown -R jenkins:jenkins /srv/openstack
```

如果使用远程 Jenkins 服务器，你需要设置权限，以便可以远程登录此机器并在该目录中运行 `git pull` 命令。

- 7. 接下来，将两个新的 puppet 模块符号链接到 master 上的 `/etc/puppet/modules`：

```
# cd /etc/puppet/modules
# ln -s /srv/openstack/puppet/modules/openstack-profile profile
# ln -s /srv/openstack/puppet/modules/openstack-role role
```

现在你应该在 master 上 `puppet module list` 的输出中看到这两个模块：

```
/etc/puppet/modules
├── openstack-profile (v0.1.0)
└── openstack-role (v0.1.0)
```

## 定义第一个角色和配置文件

我们已在设计文档中定义了一组主机组。我们将直接将它们映射到我们 Puppet 组合层中的角色。每个角色将在 role Puppet 模块中拥有自己的类和文件。角色由配置文件组成，每个配置文件代表 OpenStack 功能的一部分。例如，`mariadb` 配置文件将安装并配置我们部署的 MySQL 数据库服务器，`amqp` 配置文件将安装并配置 RabbitMQ 消息服务器，等等。让我们从创建 Database Node 角色和相关配置文件开始。

## 构建部署管道

如前所述，我们将使用在 *第 2 章，Architecting the Cloud* 中创建的 Packstack 配置作为我们组合层的起点。通常，当 Packstack 运行 时，它会删除用于在主机上安装 OpenStack 软件的 Puppet 清单。但是，如果你指定 `-y` 选项，它将退出而不执行任何操作并保留 Puppet 清单。在 *第 2 章，Architecting the Cloud* 中运行 Packstack 的原始主机上，使用相同的 answer 文件和 `-y` 选项重新运行 Packstack：

```bash
# packstack --answer-file=packstack.answers -y
```

命令执行完成后，它将打印写入 Puppet 清单的目录名称。它应类似于 `/var/tmp/packstack/20151123-214424-gJbmJA`。进入该目录并将 Hiera 数据和清单复制到你的 CI 服务器。

Packstack 的 Puppet 清单使用 Hiera，Puppet 的 **key/value lookup tool**，使存储配置信息（如密码和 IP 地址）变得容易，以便它们不必存储在清单本身中。我们将继续采用这种做法。将 `hieradata` 目录复制到 CI 服务器上的 `/etc/puppet/hieradata`。要告诉 Puppet 引用此数据，创建一个名为 `/etc/puppet/hiera.yaml` 的文件，内容如下：

```yaml
---
:backends:
  - yaml
:hierarchy:
  - defaults
  - "%{clientcert}"
  - "%{environment}"
  - global

:yaml:
  :datadir: /etc/puppet/hieradata
```

现在，让我们创建第一个配置文件。在我们检出的 openstack 仓库副本中创建名为 `puppet/modules/openstack-profile/manifests/mariadb.pp` 的新文件。再次使用非特权帐户。

从 RDO 的 Newton 版本开始，Packstack 工具创建的清单已采用我们在本章中实现的 roles and profiles 模型。Packstack 中存在两个角色——一个用于控制器，一个用于计算节点。我们将通过从控制器清单中复制相关配置文件来创建新角色。在 `mariadb.pp` 中，创建如下的类定义：

```
class profile::mariadb {
  include '::packstack::mariadb'
  include '::packstack::mariadb::services'
}
```

这使用我们在 Packstack 安装中用于安装数据库的相同 Puppet 代码定义了 `mariadb` 配置文件。现在创建名为 `puppet/modules/openstack-role/manifests/database.pp` 的文件。在该文件中创建名为 `role::database` 的类，该类引用我们之前创建的配置文件：

```
class role::database {
  include profile::mariadb
}
```

将两个文件添加到提交，然后提交并推送到 Git 服务器。

## 运行第一次构建

访问 Jenkins http://jenkins:8080/，将 `jenkins` 替换为你的 CI 服务器的 hostname。Jenkins UI 应出现，提示你创建新作业。

