# 认证 OpenStack 管理员考试备考：掌握通过认证 OpenStack 管理员考试所需的目标




## 贡献

作者
Matt Dorn

校对编辑
Stuti Srivastava

审阅者
James Denton

项目协调员
Virginia Dias

委托编辑
Kartikey Pandey

校对
Safis Editing

收购编辑
Prateek Bharadwaj

索引
Aishwarya Gangawane

内容开发编辑
Sharon Raj

图形
Kirk D'Penha

技术编辑
Mohit Hassija

制作协调员
Aparna Bhagat

## 关于作者

**Matt Dorn** 是一名高级技术讲师，曾担任过 IT 领导职务，并帮助全球数百支团队使用 OpenStack 构建私有云。他理解许多人在接触开源项目时会感到很大的畏惧，并热衷于提供易于理解的学习路径，使 OpenStack 变得易于访问且有趣。

*特别感谢 OpenStack 社区成为良好组织力量的令人难以置信的证明。感谢 Flaticon 的 Oliver 在本书许多图像中使用的图标。*

## 关于审阅者

**James Denton** 在系统管理和网络方面拥有超过 17 年的经验，并自 2012 年底以来一直部署、运营和维护 OpenStack 云。他是 Rackspace 的首席架构师，在加入 Rackspace Private Cloud 团队之前，他曾担任企业网络安全工程师 5 年。James 拥有德克萨斯州圣马科斯德克萨斯州立大学的工商管理学士学位，主修计算机信息系统。他是 Packt 出版的 *Learning OpenStack Networking* 第一版和第二版以及 *OpenStack Networking Essentials* 的作者，并在 Twitter 上可找到，用户名为 `@jimmydenton`。

*我想感谢 Matt Dorn 提供我审阅本书的机会，并感谢 Rackspace、NASA 和 OpenStack 社区打造如此出色的平台。*

## www.PacktPub.com

有关与本书相关的支持文件和下载，请访问 www.PacktPub.com。您是否知道 Packt 提供每本出版书籍的电子书版本，提供 PDF 和 ePub 文件？您可以在 www.PacktPub.com 升级到电子书版本，作为印刷书籍客户，您有权享受电子书副本的折扣。请通过 service@packtpub.com 与我们联系了解更多详情。在 www.PacktPub.com，您还可以阅读大量免费技术文章，订阅各种免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/da864e0670c65edf6f5ab861ef9906b3_7_0.png)

https://www.packtpub.com/mapt

使用 Mapt 获得最需求的软件技能。Mapt 让您完全访问所有 Packt 书籍和视频课程，以及帮助您规划个人发展和推进职业生涯的行业领先工具。

### 为什么订阅？

- 在 Packt 发行的每本书中完全可搜索
- 复制、粘贴、打印和书签内容
- 按需访问并通过网络浏览器访问

## 客户反馈

感谢您购买这本 Packt 书籍。在 Packt，质量是我们编辑流程的核心。为了帮助我们改进，请在本书的 Amazon 页面 https://www.amazon.com/dp/1787288412 上留下诚实的评论。

如果您想加入我们的常驻审阅者团队，您可以发送电子邮件至 customerreviews@packtpub.com。我们会用免费的电子书和视频作为对您宝贵反馈的回报。请帮助我们无情地改进我们的产品！

第 6 章，Neutron 网络服务，深入 OpenStack 网络世界，通过提供 Neutron 概述并涵盖考试目标，包括管理租户和提供商网络、子网、安全组、路由器和浮动 IP。

第 7 章，Cinder 块存储服务，解释了 Cinder 持久块存储和考试目标，包括创建卷、挂载/卸载卷、卷快照和创建卷类型。

第 8 章，Swift 对象存储服务，讨论了 AWS S3 的诞生、Swift 对象存储以及考试目标，包括在容器上设置访问控制列表（ACL）和设置对象在特定时间过期。

第 9 章，Heat 编排服务，解释了 OpenStack Heat 和考试目标，包括创建和更新堆栈。

第 10 章，故障排除，展示了如何对 OpenStack CLI 进行调试、管理 OpenStack 守护进程、分析 OpenStack 日志文件以及对 OpenStack 数据库执行基本操作。

第 11 章，最终技巧和窍门，在参加 COA 考试前为您提供一些有用的技巧和窍门。

第 12 章，模拟考试，是一个模拟 COA 考试，您可以用它来检验您的 OpenStack 技能！

## 本书所需

请从官方书籍仓库 https://github.com/PacktPublishing/Preparing-for-the-Certified-OpenStack-Administrator-Exam 下载所有材料。该仓库还包含一个 OpenStack 虚拟设备，可导入到 VirtualBox 中以执行所有考试目标练习。

## 本书适合谁

本书面向具有基本 Linux 命令行和网络知识的 IT 专业人员、系统管理员、DevOps 工程师和软件开发人员。

## 惯例

在本节中，您将找到多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义说明。

文本中的代码词、数据库表名、Linux 程序、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下：“您可以使用 `python-openstackclient` 完成大部分考试目标。”

代码块如下所示：

```
export OS_AUTH_URL=http://192.168.56.56:5000/v3
export OS_REGION_NAME=RegionOne
export OS_PROJECT_DOMAIN_NAME=default
export OS_USER_DOMAIN_NAME=default
export OS_USERNAME=admin
export OS_PASSWORD=admin
export OS_PROJECT_NAME=logistics
export OS_TENANT_NAME=logistics
```

任何命令行输入或输出如下所示：

```
$ openstack help
```

新术语和 **important words** 以粗体显示。您在屏幕上看到的文字，例如菜单或对话框中的文字，在文本中如下所示：“从 VirtualBox 菜单栏中选择 **Preferences**。”

![](img/da864e0670c65edf6f5ab861ef9906b3_20_0.png)

警告或重要说明以如下框形式出现。

![](img/da864e0670c65edf6f5ab861ef9906b3_20_1.png)

技巧和窍门如下所示。



## 读者反馈

读者反馈总是受欢迎的。请告诉我们您对本书的看法——您喜欢或不喜欢的部分。读者反馈对我们非常重要，因为它帮助我们开发您能真正充分利用的图书。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及图书标题。

如果您在某个领域有专长并有兴趣撰写或为图书做出贡献，请参阅我们的作者指南 www.packtpub.com/authors。

## 客户支持

现在您已成为 Packt 图书的自豪拥有者，我们提供多项帮助以助您充分利用购买。

## 下载本书的彩色图片

我们还为您提供一份 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图片。这些彩色图片将帮助您更好地理解输出的变化。您可从 https://www.packtpub.com/sites/default/files/downloads/PreparingfortheCertifiedOpenStackAdministratorExam_ColorImages.pdf 下载此文件。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍会发生。如果您在我们的某本书中发现错误——也许是文本或代码中的错误——我们将不胜感激您能向我们报告。这样做可以让其他读者免于困扰，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择您的图书，点击勘误提交表单链接，并输入勘误详情。一旦您的勘误得到验证，您的提交将被接受，勘误将被上传到我们的网站或添加到该标题勘误部分下现有勘误列表中。

要查看之前提交的勘误，请前往 https://www.packtpub.com/books/content/support 并在搜索字段中输入图书名称。所需信息将出现在勘误部分下。

## 盗版

互联网上版权材料的盗版是所有媒体持续存在的问题。在 Packt，我们非常重视版权和许可的保护。如果您在互联网上以任何形式遇到我们作品的非法复制品，请立即提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上疑似盗版材料的链接。

我们感谢您帮助保护我们的作者以及我们为您提供有价值内容的能力。

## 问题

如果您对本书的任何方面有问题，可联系我们 questions@packtpub.com，我们将尽力解决问题。
