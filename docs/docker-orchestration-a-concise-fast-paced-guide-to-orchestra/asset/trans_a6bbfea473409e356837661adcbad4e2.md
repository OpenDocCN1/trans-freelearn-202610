

## 自动化使用 Docker Hub 构建镜像

Docker Hub 具有一项功能，可根据托管在 GitHub 或 Bitbucket 上的 Git 分支更新自动构建新镜像。要创建自动化构建，请从 **创建** 菜单中选择 **创建自动化构建**。如果您尚未关联，系统会提示您关联 GitHub 或 Bitbucket 帐户。

请注意，关联的 GitHub 或 Bitbucket 帐户只能关联到一个 Docker Hub 帐户。

向导将引导您完成新仓库的创建。系统会要求您为仓库提供名称和描述。创建仓库后，请转到 **构建设置** 来配置自动化构建。Docker Hub 可以跟踪 Git 仓库中的多个分支，并在该分支每次更新时构建一个新的带标签镜像。

您还可以配置 Docker Hub，以便在上游容器更新时重建您的镜像。例如，假设您的容器基于 `ubuntu` 镜像，换句话说，您在 `Dockerfile` 顶部使用了 `From ubuntu`。每当 `ubuntu` 镜像重建时，也会触发您的镜像重建。这是确保您的镜像始终拥有来自上游镜像的最新安全更新的一种方法。

自动化构建是开始使用 **持续集成 (CI)** 和 **持续部署 (CD)** 的好方法。它不具备 **GitLab** 或 **Jenkins** 那样的镜像测试功能，但您仍然可以构建自动化测试流程。例如，当开发人员准备测试其应用程序时，他们可以将更改推送到 GitHub 上的测试分支。此推送将触发 Docker Hub 上的构建，从而构建一个新镜像。一个进程可以在您的某台服务器上运行，定期拉取测试分支，针对其运行测试套件，并向您的团队发送构建报告。如果测试成功，团队随后可以将代码推送到 `master` 分支并触发生产构建。

CI/CD 工具是确保部署过程稳定性和可靠性的一种方法。*第 9 章，使用持续集成来构建、测试和部署容器，* 将向您展示如何使用 GitLab 中内置的 Docker Registry 来自动化测试和部署。

## 与 Docker Cloud 集成

Docker 提供了一个名为 Docker Cloud 的完全云端托管的 Docker 管理平台。Docker Cloud 使用 Docker Hub 提供注册表服务。这意味着，除了与管理环境的紧密集成外，您还可以获得与直接在 Docker Hub 上可用的相同自动化构建服务。除了自动化构建外，Docker Cloud 还提供自动化测试框架。这将在第 9 章“使用持续集成来构建、测试和部署容器”中进一步介绍。

Docker Cloud 对私有仓库的另一项功能是能够对镜像执行安全扫描。它会扫描镜像的每一层以查找已知漏洞，并提供报告。扫描在镜像推送时或触发自动化构建时运行。生成的报告可能会显示您的镜像所基于的镜像中存在漏洞。在这种情况下，您将不得不使用较新版本的基础镜像重建，或自行解决漏洞。

## 使用 GitLab 容器注册表

GitLab 是一款一体化开发套件。它提供 Git 仓库、问题跟踪、CI 和 Docker 注册表。GitLab 中 Git、问题跟踪和 Docker 注册表之间的紧密集成使其成为应用程序开发的理想解决方案。GitLab 是开源的，可以本地安装。企业托管也可在 gitlab.com 上获得。

使用 GitLab 最简单的方式是通过其云端服务，但本地安装也很简单。就我的经验而言，安装 GitLab 的最佳方式是使用 Omnibus 安装程序。它会安装 GitLab 所需的所有软件并管理所有更新。Omnibus 软件包适用于 Ubuntu、Debian、CentOS 和 Raspberry Pi。

有一个可用的 Docker 镜像，但使用起来稍微困难一些。我建议以 gitlab/gitlab-ce 为基础创建自己的镜像，以放置软件包配置和 SSL 证书。GitLab Docker 镜像文档位于 https://docs.gitlab.com/omnibus/docker/。

## 在 Ubuntu 上安装 GitLab

在安装 GitLab 之前，需要安装许多依赖项。Postfix 是可选的。如果您选择不使用它，则需要将 GitLab 配置为使用另一台服务器发送电子邮件：

```
$ sudo apt-get install curl openssh-server ca-certificates postfix
```

GitLab 是另一个希望您下载脚本并直接通过 sudo 管道运行的项目。我再次强调，这样做非常危险。相反，请下载脚本，查看它，如果看起来没问题，则运行它：

```
$ wget -qO script.deb.sh
https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh
$ sudo bash script.deb.sh
$ sudo apt-get install gitlab-ce
```

GitLab 配置在 /etc/gitlab/gitlab.rb 中。有关所有选项的完整指南超出了本书范围。相反，让我们专注于配置 Docker Registry 所需的内容：

```
registry['enable'] = true
registry_external_url 'https://registry.example.com'
gitlab_rails['registry_path'] = "/var/opt/gitlab/gitlab-rails/shared/registry"
registry_nginx['ssl_certificate'] = "/etc/gitlab/ssl/registry.example.com.crt"
registry_nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/registry.example.com.key"
gitlab_rails['lfs_enabled'] = true
```

第一行在 GitLab 中启用注册表服务。第二行设置注册表的 URL。它可以使用与 GitLab 其余部分相同的地址，但使用不同的端口。也可以使注册表在单独的主机名上可用。第三行设置注册表在文件系统上的位置。

第四行和第五行定义 SSL 证书和密钥文件。注册表使用的证书必须受客户端信任，否则 Docker 会抛出错误。如果您使用的证书包含中间证书，则需要将证书链添加到证书文件中：

```
$ cat intermediate.pem your_certificate.pem > registry.pem
```

> 不推荐在没有证书的情况下使用 GitLab Docker 注册表，但这是可行的。将 --insecure-registry 和您的注册表地址添加到 Docker 守护进程的启动命令中。完整详情可在 https://docs.docker.com/registry/insecure/ 上找到。

最后一行启用 Git 大文件存储 (LFS)。这并非严格必要，但它确实允许将大文件添加到镜像的 Git 仓库中。例如，我有需要 Oracle Instant Client 的服务。客户端的 ZIP 文件很大，但构建镜像时需要它们。LFS 允许轻松地将客户端添加到 Git。LFS 必须在客户端上安装并配置才能使用。

对 `gitlab.rb` 所做的更改后，需要重新配置 GitLab。这是通过运行 `sudo gitlab-ctl reconfigure` 完成的。升级后也会运行此命令以更新数据库。

> 有关配置 GitLab 容器注册表的完整详情，请参阅：
> https://docs.gitlab.com/ce/administration/container_registry.htm

## 在项目中启用 Docker 注册表

可以为在 GitLab 中创建的任何项目启用 Docker 注册表。单击右上角的齿轮图标并选择 **编辑项目**。向下滚动到 **功能** 部分，启用 **容器注册表**，然后单击 **保存更改**：

![](img/02bead9c8ff4515f78dac0eb33163961_68_0.png)

启用后，拥有仓库访问权限的用户可以使用其 GitLab 凭据通过 `docker login` 登录。用户可能对仓库具有读取或写入权限，具体取决于其项目访问权限。登录后，镜像可以照常推送或拉取。请注意，镜像名称以注册表地址开头：

```
$ docker pull registry.example.com/namespace/project
$ docker push registry.example.com/namespace/project
```

GitLab CI 有能力自动构建、测试甚至部署镜像。这将在第 9 章“使用持续集成来构建、测试和部署容器”中详细介绍。

> 有关在项目中使用 GitLab 容器注册表的完整文档，请参阅：
> https://docs.gitlab.com/ce/user/project/container_registry.html

## 介绍 Docker Trusted Registry 和 Docker Datacenter

Docker Trusted Repository (DTR) 是 Docker Universal Control Plane (UCP) 可用的应用程序，也是 Docker Datacenter 的一部分。Docker Datacenter 提供了一个本地或基于云的集群管理工具，具备基于角色的访问控制 (RBAC)、LDAP 和 AD 身份验证，以及对 Docker 主机的监控。实际上，Docker Datacenter 是 Docker Cloud，DTR 充当在您自己的网络上运行的 Docker Hub 的角色。

Docker Datacenter 支持带有完整 CI/CD 的镜像自动化构建或测试。它还内置了对高可用性和镜像安全性的支持。Docker Datacenter 使用集成的 Docker Content Trust 对镜像进行数字签名和验证。这确保您获得的镜像就是您想要的镜像。

## 自己构建

如果这些选项都不适合您，还有自己构建的选项。这将允许您根据特定需求自定义注册表，但实现可能需要大量工作。它在较小的环境中也效果良好，这些环境不需要 GitLab 的复杂性或 Docker Datacenter 的费用，但仍然需要私有仓库。

