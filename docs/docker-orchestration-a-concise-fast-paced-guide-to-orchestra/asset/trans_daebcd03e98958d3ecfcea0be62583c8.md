

查看脚本后，运行它：

```
$ sh install-docker.sh
```

运行安装脚本的用户需要是 root 用户，或具有使用 su 或 sudo 的能力。

如果你运行的是受支持的 Linux 发行版，脚本将准备你的系统并安装 Docker。安装完成后，Docker 将由本地包管理系统更新，例如 Debian 和 Ubuntu 上的 `apt` 或 CentOS 和 **红帽企业 Linux (RHEL)** 上的 `yum`。`install` 命令会启动 Docker 并将其配置为随系统启动。

受支持的操作系统、发行版和云提供商列表位于 https://docs.docker.com/engine/installation/。

默认情况下，任何在本地使用 Docker 的人都需要 root 权限。你可以通过将其添加到由安装包创建的 `docker` 组来更改这一点。他们将能够在下次登录后无需 root 即可使用 Docker。

## 使用 Docker Machine 进行安装

Docker 提供了一个非常实用的工具，用于便利地在各种云服务和 Linux 主机上部署和管理 Docker 主机，该工具名为 **Docker Machine**。Docker Machine 作为 Docker Toolbox 的一部分安装，但也可以单独安装。完整说明可在 https://github.com/docker/machine/releases/ 中找到。

Docker Machine 支持许多不同的云服务，包括 AWS、Microsoft Azure 和 GCE。它还可以配置为连接到任何现有的受支持 Linux 服务器。`docker-machine` 使用的驱动程序由 `--driver` 标志定义。每个驱动程序都有自己特定的标志，用于控制 `docker-machine` 与服务的交互方式。

## 在 AWS 上启动主机

AWS 是运行 Docker 主机的绝佳方式，而 `docker-machine` 使启动和管理它们变得容易。你可以使用 **弹性负载均衡 (ELB)** 将流量发送到特定主机上运行的容器，或在多个主机之间进行负载均衡。

首先，你需要从 AWS 获取访问凭据。有几种使用方式。首先，你可以在运行 docker-machine 时将其包含在命令行中：

```
$ docker-machine create --driver amazonec2 --amazonec2-access-key AK*** --amazonec2-secret-key DM*** ...
```

其次，你可以将其添加到 ~/.aws/credentials。将凭据放在凭据文件中意味着你每次使用 docker-machine 处理 AWS 时都不需要在命令行中包含它们。它还可以使你的凭据不出现在命令行和进程列表中。以下示例将假设你已创建凭据文件，以避免命令行混乱：

```
[default]
aws_access_key_id = AK***
aws_secret_access_key = DM***
```

使用 create 子命令创建新的 Docker 主机。你可以使用 --amazonec2-region 标志指定区域。默认情况下，主机将在 us-east-1 区域启动。命令行上的最后一项是实例名称，本例中为 dm-aws-test：

```
$ docker-machine create --driver amazonec2 --amazonec2-region us-west-2 dm-aws-test
Creating CA: /home/user/.docker/machine/certs/ca.pem
Creating client certificate:
/home/user/.docker/machine/certs/cert.pem
Running pre-create checks...
Creating machine...
(dm-aws-test) Launching instance...
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on
this virtual machine, run: docker-machine env dm-aws-test
```

![](img/02bead9c8ff4515f78dac0eb33163961_25_0.png)

该命令运行需要几分钟，但完成后，你将拥有一个功能齐全的 Docker 主机，准备好运行容器。`ls` 子命令将显示 `docker-machine` 知道的所有主机：

```
$ docker-machine ls
NAME                ACTIVE   DRIVER       STATE     URL                         SWARM
DOCKER              ERRORS
dm-aws-test         -        amazonec2    Running   tcp://52.43.71.87:2376
                        v1.12.1
```

主机的 IP 地址列在 `docker-machine ls` 的输出中，但你也可以通过运行 `docker-machine ip` 获取它。要开始使用新主机，请通过运行 `eval $(docker-machine env dm-aws-test)` 设置环境。现在当你运行 Docker 时，它将与运行在 AWS 上的实例通信。甚至可以使用 `docker-machine` 通过 `ssh` 登录到服务器：

```
$ docker-machine ssh dm-aws-test
Welcome to Ubuntu 15.10 (GNU/Linux 4.2.0-18-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
 Get cloud support with Ubuntu Advantage Cloud Guest:
 http://www.ubuntu.com/business/services/cloud

New release '16.04.1 LTS' available.
Run 'do-release-upgrade' to upgrade to it.
```

```
*** System restart required ***
ubuntu@dm-aws-test:~$
```

使用完实例后，可以使用 docker-machine stop 停止它，并使用 docker-machine rm 删除它：

```
$ docker-machine stop dm-aws-test
Stopping "dm-aws-test" ...
Machine "dm-aws-test" was stopped.
```

```
$ docker-machine rm dm-aws-test
About to remove dm-aws-test
Are you sure? (y/n): y
Successfully removed dm-aws-test
```

> 可以向 docker-machine create 传递多个选项，包括用于使用自定义 AMI、实例类型或卷大小的选项。完整文档可在 https://docs.docker.com/machine/drivers/aws/ 中找到。

## 在 GCE 上启动主机

GCE 是云计算领域的另一大参与者。其 API 使在 Google 高性能基础架构上启动新主机变得非常容易。如果你已经在使用其他 Google Cloud 服务，Google 托管 Docker 主机是一个绝佳选择。

你需要在 GCE 为容器创建一个项目。身份验证通过 Google 应用程序默认凭据 (ADC) 进行。这意味着如果你在 GCE 主机上运行 docker-machine，身份验证将自动进行。如果你从自己的计算机运行 docker-machine，则需要使用 gcloud 工具进行身份验证。gcloud 工具需要 Python 2.7，可从以下网站下载：https://cloud.google.com/sdk/。

```
$ gcloud auth login
```

gcloud 工具将打开一个网页浏览器以使用 OAuth 2 进行身份验证。选择你的帐户，然后在下一页点击 Allow。你将被重定向到显示已通过身份验证的页面。现在，进入有趣的部分：

```
$ docker-machine create --driver google \
--google-project docker-test-141618 \
--google-machine-type f1-micro \
dm-gce-test
```

![](img/02bead9c8ff4515f78dac0eb33163961_27_0.png)

完成所需时间取决于你选择的映像大小，可能需要几分钟。完成后，你将拥有一个在 GCE 上运行的 Docker 主机。现在，你可以像之前在 AWS 上一样使用 ls、ssh 和 ip 子命令。完成后，运行 docker-machine stop 和 docker-machine rm 来停止和删除映像。

> 可以向 docker-machine 传递多个选项，包括用于设置区域、映像和机器时间的选项。完整文档可在 https://docs.docker.com/machine/drivers/gce/ 中找到。

## 在 Microsoft Azure 上启动主机

Microsoft 是云服务领域相对较新的参与者，但他们构建了一项令人印象深刻的服务。Azure 支撑着多个大型系统，包括 Xbox Live。

Azure 使用订阅 ID 进行身份验证。你将获得一个访问代码，并被引导到 https://aka.ms/device/login 输入它。选择 Continue，选择你的帐户，然后点击 Accept。完成后，可以关闭浏览器窗口：

```
$ docker-machine create --driver azure --azure-subscription-id 30*** dm-azure-test
```

![](img/02bead9c8ff4515f78dac0eb33163961_28_0.png)

同样，完成需要一些时间。完成后，你将能够在新主机上运行容器。一如既往，你可以使用 docker-machine 管理新主机。在 Azure 上删除主机时，输出中有一条重要通知。值得确保一切都已清理干净：

```
$ docker-machine rm dm-azure-test
About to remove dm-azure-test
Are you sure? (y/n): y
(dm-azure-test) NOTICE: Please check Azure portal/CLI to make sure you have no
leftover resources to avoid unexpected charges.
(dm-azure-test) Removing Virtual Machine resource. name="dm-azure-test"
(dm-azure-test) Removing Network Interface resource. name="dm-azure-test-nic"
(dm-azure-test) Removing Public IP resource. name="dm-azure-test-ip"
(dm-azure-test) Removing Network Security Group resource.
```

