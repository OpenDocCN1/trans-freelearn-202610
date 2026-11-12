# Docker 编排：可扩展服务编排与部署简明快速指南

# 致谢

- **作者**
- Randall Smith

- **副本编辑**
- Tom Jacob

- **审稿人**
- Vincent De Smet

- **项目协调员**
- Kinjal Bari

- **委托编辑**
- Pratik Shah

- **校对**
- Safis Editing

- **获编编辑**
- Rahul Nair

- **索引编辑**
- Tejal Daruwale Soni

- **内容开发编辑**
- Radhika Atitkar
- Sanjeet Rao

- **图形**
- Kirk D'Penha

- **技术编辑**
- Bhagyashree Rai

- **制作协调员**
- Melwyn Dsa

# 关于作者

**Randall Smith** 是 Adams State University 的高级系统管理员。他自 1999 年起一直管理 Windows、Linux 和 BSD 系统。

他积极帮助其他系统管理员解决线上和线下问题。他在 Colorado Higher Ed Computing Organization 和 Educause 会议上就包括 Linux KVM 和 Ceph 在内的主题进行过演讲。

在业余时间，Randall 在 YouTube 和 Twitch 的 Music Free Gaming 上直播 Let's Play 游戏视频。

我要感谢我的家人，在我写书期间长时间消失在卧室里，他们给予了支持和理解。我要感谢 Adams State University 提供用于测试的服务器，以及让我们有机会接触 Docker 等非常酷的软件。最后，我要感谢审稿人和 Packt 的所有人，是他们让这本书得以出版。

我想特别向所有开源本书中描述的工具的开发者和公司致谢。你们让我们其余的人完成工作变得容易得多。非常感谢大家。

# 关于审稿人

**Vincent De Smet** 是一位 Docker Captain，在越南和新加坡分别担任 IT 顾问和 DevOps 工程师。他还是 Docker Saigon 和 Cloud Native Singapore 聚会的联合组织者。Vincent 喜欢深入探究容器内部机制，是活跃的演讲者，并在 `docker-saigon.github.io` 博客上发表文章。

# www.PacktPub.com

有关本书的支持文件和下载，请访问 www.PacktPub.com。

你知道吗？Packt 为每本出版的书籍提供电子书版本，有 PDF 和 ePub 文件可供下载？您可以在 www.PacktPub.com 升级到电子书版本，作为印刷书籍客户，您有权享受电子书副本的折扣。如需更多详情，请联系 service@packtpub.com。

在 www.PacktPub.com，您还可以阅读一系列免费技术文章，订阅多种免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/02bead9c8ff4515f78dac0eb33163961_6_0.png)

https://www.packtpub.com/mapt

通过 Mapt 获取最热门的软件技能。Mapt 让您完全访问所有 Packt 图书和视频课程，以及行业领先的工具，帮助您规划个人发展并推进职业生涯。

# 为什么要订阅？

- 可在 Packt 出版的每本书中进行全面搜索
- 复制粘贴、打印和书签内容
- 按需通过网络浏览器访问

# 客户反馈

感谢您购买这本 Packt 图书。我们非常认真地对待改进内容和产品以满足您需求的承诺——这就是为什么您的反馈如此宝贵。无论您对购买有何感受，请考虑在此书的 Amazon 页面上留下评论。这不仅能帮助我们，更重要的是，它还能帮助社区中的其他人对其投资于学习的资源做出明智的决定。

您还可以通过加入我们的审稿俱乐部定期为我们撰写评论。如果您有兴趣加入，或想了解我们提供的更多福利，请联系我们：customerreviews@packtpub.com。

# 前言

Docker 容器是构建和部署服务的一项强大工具，可以保持一致性和可靠性。随着容器数量的增加，管理它们也成为一个问题。当容器在多个主机上运行时，这个问题会更加严重。本书将向您展示如何通过编排来消除混乱，带来秩序。

本书首先向您展示如何开始使用 Docker，然后深入探讨 Docker 集群所需的构建块。它将向您展示如何使用“三大”编排工具——Docker Swarm、Kubernetes 和 Mesosphere。它还将介绍另外两种工具，Fleet 和 Cattle，这些工具使用和安装可能更简单，但功能非常强大。

最后，您将了解到使集群管理人员和创建将要运行的镜像的开发人员生活更加简单的工具。您将探索监控集群的工具并查看性能瓶颈所在。您将看到如何使用持续集成来一致且可靠地构建、测试和部署 Docker 镜像。最后，本书将向您展示如何将相同的原则应用于 Docker 运行所在的主机。

# 本书内容概要

- 第 1 章，Docker 编排入门，将帮助您开始使用 Docker。它将向您展示如何在本地以及 Amazon Web Services、Google Compute Engine 和 Microsoft Azure 等流行的云服务上安装 Docker。
- 第 2 章，使用 Docker Compose 构建多容器应用程序，介绍了如何使用 Docker Compose 构建多容器应用程序。它还展示了如何使用命名网络进行数据隔离以及使用数据卷共享数据。
- 第 3 章，集群构建块 – 镜像仓库、覆盖网络和共享存储，描述了 Docker 集群所需的构建块。您将探索镜像仓库、覆盖网络和共享存储，并了解它们为何必要。
- 第 4 章，使用 Docker Swarm 进行编排，向您展示如何使用 Docker Swarm 创建和管理集群。您将学习如何管理服务、以多个副本运行服务以及执行“零停机”升级。
- 第 5 章，使用 Kubernetes 部署和管理服务，探讨了使用 Kubernetes 编排 Docker 集群的基础知识。您将看到如何创建由多个容器提供的服务，如何为这些服务提供负载均衡，以及如何无缝升级它们。最后，您将看到如何创建持久卷以在容器之间共享数据。
- 第 6 章，使用 Mesosphere，深入探讨了使用 Mesosphere 和 DC/OS 进行编排。您将学习如何创建和管理容器、对它们进行负载均衡以及提供持久存储。您还将看到如何创建健康检查以确保服务正常运行，以及如何执行“零停机”升级。
- 第 7 章，使用更简单的编排工具 – Fleet 和 Cattle，介绍了 Fleet 和 Cattle。Fleet 是一种低级工具，可用于构建自定义管理工具。Cattle 是 Rancher 提供的一种功能强大但易于使用的编排工具。您将看到如何使用每种工具来管理容器和服务。
- 第 8 章，监控您的集群，探讨了确保集群及其中运行的容器行为正常的重要任务。您将学习如何在 Elasticsearch 中汇总所有容器的日志并在 Kibana 中查看它们。您还将学习如何使用 collectd、InfluxDB 和 Grafana 收集性能数据。
- 第 9 章，使用持续集成构建、测试和部署容器，解释了如何在 GitLab 中使用持续集成来构建、测试甚至部署容器。这有助于确保镜像以一致且可靠的方式构建。
- 第 10 章，为何止步于容器？自动化您的基础架构，超越容器，展示了构建和编排 Docker 容器所用的相同原则也可应用于容器运行所在的主机。您将看到如何使用配置管理来配置主机并确保其保持配置状态。本章还将向您展示如何使用 Packer 构建可在本地或云服务上部署的主机镜像。最后，它将介绍 Terraform，可用于从头开始创建整个 Docker 集群。

# 本书所需内容

您需要一个运行 Docker 主机的位置。这可以是 OpenStack 等本地集群，也可以是 Amazon EC2、Google Compute Engine 或 Microsoft Azure 等云托管提供商。第 1 章将向您展示如何在 Linux 上安装 Docker，本书其余部分假设工具从 Linux 运行。目前，Ubuntu、CentOS 和 Red Hat Enterprise Linux 支持最好。




需要文本编辑器（例如 Emacs），因为大多数编排工具都将配置存储在文本文件中。本书中描述的许多工具和示例都需要网页浏览器来访问。

# 本书适用人群

本书面向具备 Docker 基础知识、现在希望跨多台主机部署和管理容器的开发人员和系统管理员。

# 约定

本书中，您将看到多种文本样式，用于区分不同类型的信息。以下是这些样式的示例及其含义。

文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 帐号将显示如下：

> “使用 `docker run` 命令启动容器。”

代码块设置如下：

```
#cloud-config
users:
  - name: "demo"
    passwd: "$6$HpqJOCs8XahT$mSgRYAn..."
    groups:
    - "sudo"
    - "docker"
```

当我们希望提请您注意代码块的特定部分时，相关行或项目将以粗体显示：

```
#cloud-config
users:
  - name: "demo"
    passwd: "$6$HpqJOCs8XahT$mSgRYAn..."
    groups:
    - "sudo"
    - "docker"
```

命令行输入或输出将按以下方式编写：

```
$ wget -qO - https://get.docker.com/ | sh
```

新术语和重要单词以粗体显示。您在屏幕上看到的单词，例如在菜单或对话框中，将出现在文本中，如下所示：“在下一页选择您的帐户，然后单击允许。”

警告或重要注释会以如下方框形式出现。

提示和技巧将按如下形式出现。

# 读者反馈

我们始终欢迎读者反馈。告诉我们您对本书的看法——您喜欢或不喜欢什么。读者反馈对我们非常重要，因为它能帮助我们开发出您真正能从中获益的书籍。如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中注明书名。如果您在某个主题方面有专长，并有兴趣编写或参与编写书籍，请参阅我们的作者指南 www.packtpub.com/authors。

# 客户支持

既然您是 Packt 图书的自豪拥有者，我们有许多资源可帮助您充分利用购买的书籍。

# 下载示例代码

您可以从 http://www.packtpub.com 的帐户中下载本书的示例代码文件。如果您在其他地方购买了本书，可以访问 http://www.packtpub.com/support 并注册，以便将文件直接发送到您的电子邮箱。

您可以按以下步骤下载代码文件：

- 1. 使用您的电子邮件地址和密码登录或注册我们的网站。
- 2. 将鼠标指针悬停在顶部的 SUPPORT 选项卡上。
- 3. 单击 代码下载与勘误。
- 4. 在搜索框中输入书名。
- 5. 选择您要下载代码文件的图书。
- 6. 从下拉菜单中选择购买本书的渠道。
- 7. 单击 **代码下载**。

文件下载完成后，请确保使用最新版本的以下工具解压或提取文件夹：

- WinRAR / 7-Zip（适用于 Windows）
- Zipeg / iZip / UnRarX（适用于 Mac）
- 7-Zip / PeaZip（适用于 Linux）

本书的代码包也托管在 GitHub 上，网址为 https://github.com/PacktPublishing/Docker-Orchestration。我们丰富的图书和视频目录中还有其他代码包，网址为 https://github.com/PacktPublishing/。请查看！

# 下载本书的彩色图像

我们还提供一个 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图像。彩色图像将帮助您更好地理解输出中的变化。您可以从 https://www.packtpub.com/sites/default/files/downloads/DockerOrchestration_ColorImages.pdf 下载此文件。

# 勘误

尽管我们已竭力确保内容的准确性，但错误仍难免发生。如果您在我们的某本书中发现错误——无论是文本中的错误还是代码中的错误——我们将不胜感激，如果您能将其报告给我们。这样做可以避免其他读者感到沮丧，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择您的图书，单击 **勘误提交表单** 链接，并输入勘误详细信息。您的勘误经过验证后，您的提交将被接受，勘误将上传到我们的网站，或添加到该书名下勘误部分的现有勘误列表中。

要查看先前提交的勘误，请访问 https://www.packtpub.com/books/content/support 并在搜索字段中输入图书名称。所需信息将出现在 **勘误** 部分下。

# 盗版

互联网上对受版权保护材料的盗版是所有媒体中的持续问题。在 Packt，我们非常重视版权和许可的保护。如果您在互联网上以任何形式发现我们作品的非法副本，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并提供涉嫌盗版材料的链接。

感谢您帮助保护我们的作者以及我们为您提供有价值内容的能力。

# 问题

如果您对本书的任何方面有任何问题，可以通过 questions@packtpub.com 联系我们，我们将尽力解决问题。
