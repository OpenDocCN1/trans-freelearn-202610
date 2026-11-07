
雅罗斯瓦夫·克罗赫马尔斯基

## 开发
使用 Docker

以这本快速入门指南改变您组织大规模部署软件的方式，探索 Docker 世界

![](img/0c87321919fa245e22f056c67ec70003_0_0.png)

![](img/0c87321919fa245e22f056c67ec70003_0_1.png)

## 使用 Docker 开发

以这本快速入门指南改变您组织大规模部署软件的方式
探索 Docker 世界

雅罗斯瓦夫·克罗赫马尔斯基

![](img/0c87321919fa245e22f056c67ec70003_1_0.png)

伯明翰 - 孟买

## 使用 Docker 开发

版权所有 © 2016 Packt Publishing

保留所有权利。未经出版者事先书面许可，本书的任何部分不得以任何形式或任何手段复制、存储于检索系统或传播，但用于评论性文章或评论中嵌入的简短引文除外。

在本书准备过程中已尽最大努力确保所提供信息的准确性。然而，本书所含信息按“原样”出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商不对因本书直接或间接造成或被认为造成的任何损害承担责任。

Packt Publishing 已尽力通过适当使用大写字母提供本书中提及的所有公司和产品的商标信息。然而，Packt Publishing 不能保证该信息的准确性。

首次出版：2016 年 11 月

生产参考：1241116

由 Packt Publishing Ltd 出版。
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.
ISBN 978-1-78646-990-8

www.packtpub.com

## 致谢

**作者**
Jaroslaw Krochmal ski

**校对编辑**
Laxmi Subramanium
Sneha Singh

**审阅者**
Jeeva S. Chelladhurai

**项目协调员**
Kinjal Bari

**约稿编辑**
Kartikey Pandey

**校对**
Safis Editing

**采购编辑**
Narsimha Pai
Rahul Nair

**索引**
Pratik Shirodhkar

**内容开发编辑**
Trusha Shiryan

**美术**
Kirk D'Penha

**技术编辑**
Naveenkumar Jain

**制作协调员**
Shantanu N. Zagade

## 关于作者

**Jaroslaw Krochmal ski** 是一位专注于金融业务领域的热情软件设计师和开发者。他在软件开发方面拥有超过 12 年的经验。他是整洁代码和软件工艺的爱好者。他是认证的 Scrum 大师，也是敏捷的粉丝。他的专业兴趣包括 Web 应用开发的新技术、设计模式、企业架构和集成模式。

他自 2000 年起就开始专业设计和开发软件，并自 2002 年起将 Java 作为其主要编程语言。过去，他曾为 Kredyt Bank（KBC）和 Bank BPS 等公司在国际汇款、快速支付和催收系统等多个大型项目上工作。他目前在丹麦公司 7N 担任顾问，为 Nykredit 银行担任基础设施架构师。您可以通过 Twitter @jkroch 或电子邮件 jarek@finsys.pl 联系他。

我想向我在 7N 和 Nykredit 的朋友们问好——继续保持出色的工作！

## 关于审阅者

**Jeeva S. Chelladhurai** 在过去 8 年里一直担任 IBM 全球云卓越中心 CoE 的 DevOps 专家。他是 PacktPub、UK 出版的 *Learning Docker* 的合著者。他拥有超过 20 年的 IT 行业经验。他在全球范围内技术管理并指导过多元化团队，致力于构想和构建开创性的电信产品。他专注于 DevOps 和云解决方案交付，重点关注数据中心优化、软件定义环境 SDEs，以及使用最新的 Docker 技术进行分布式应用的开发、部署和交付。Jeeva 也是敏捷方法论、DevOps 和 IT 自动化 的坚定倡导者。他拥有 Manonmaniam Sundaranar 大学的计算机科学硕士学位，以及美国波士顿大学的项目管理毕业证书。除了正式职责外，他还撰写书籍章节并撰写研究论文。他在为 IBM 解决方案架构师和顾问打造可重用的技术资产方面发挥了重要作用。目前，他正在为 Docker 书籍的第二版做出贡献。他在技术论坛上就 DevOps 技术和工具发表演讲。他的 LinkedIn 个人资料可在 https://www.linkedin.com/in/JeevaChelladhurai 找到。

## www.PacktPub.com

有关您书籍的相关支持文件和下载，请访问 www.PacktPub.com。

您知道 Packt 提供每本出版书籍的电子书版本，支持 PDF 和 ePub 文件吗？您可以在 www.PacktPub.com 升级到电子书版本，作为印刷书籍客户，您有权享受电子书的折扣。欲了解更多详情，请联系 service@packtpub.com。

在 www.PacktPub.com，您还可以阅读大量免费技术文章，订阅各类免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/0c87321919fa245e22f056c67ec70003_6_0.png)

https://www.packtpub.com/mapt

使用 Mapt 获取最受欢迎的软件技能。Mapt 可让您完全访问所有 Packt 书籍和视频课程，以及业界领先的工具，帮助您规划个人发展并推进职业生涯。

## 为什么订阅？

- 在 Packt 发行的每本书中都可完全搜索
- 复制、粘贴、打印和书签内容
- 按需访问，可通过网络浏览器访问

## 目录

- 前言 1
- 第 1 章：Docker 简介 6
- 基本思想 7
- 容器化与虚拟化 7
- 传统虚拟化 7
- 容器化 8
- 使用 Docker 的好处 9
- 速度和大小 9
- 可重复且可移植的构建 9
- 不可变且敏捷的基础设施 10
- 工具和 API 11
- 工具概览 11
- Docker Engine 和 Docker Engine 客户端 12
- Docker Machine 12
- Kitematic 14
- Docker compose 14
- Oracle VirtualBox 15
- Git 16
- 摘要 17
- 第 2 章：安装 Docker 18
- 硬件要求 19
- 在 Windows 上安装 20
- 在 Mac OS 上安装 29
- 在 Linux 上安装 34
- 在云端安装 — AWS 37
- 摘要 43
- 第 3 章：理解镜像和容器 44
- 镜像 45
- 层 47
- 容器 53
- 保存容器更改 55
- Docker 注册表、仓库和索引 59
- 摘要 62

## 第 4 章：网络和持久化存储 63

- Docker 网络 64
- 默认网络 65
- 无网络 66
- 主机网络 67
- 桥接网络 68
- 创建网络 69
- 在网络中运行容器 70
- 创建多主机网络 74
- Swarm 模式 74
- 基于键值存储的覆盖网络 76
- 暴露和映射端口 83
- 链接容器 88
- 网络插件 91
- Docker 卷 91
- 创建卷 92
- 删除卷 98
- 卷驱动 99
- 摘要 100

## 第 5 章：查找镜像 102

- Docker Hub 103
- Docker Hub 账户 105
- 组织账户和团队 107
- 协作者 108
- 私有仓库 109
- 登录 Docker Hub 110
- 搜索镜像 113
- 镜像命名和标签 113
- 通过 Web 界面搜索 115
- 使用命令行搜索 117
- Docker Hub 和 docker registry REST API 119
- 摘要 124

## 第 6 章：创建镜像 125

- Dockerfile 指令 127
- FROM 128
- MAINTAINER 129
- ADD 129
- COPY 132

| 项目 | 页码 |
|------|------|
| CMD | 133 |
| ENTRYPOINT | 136 |
| LABEL | 139 |
| EXPOSE | 142 |
| RUN | 143 |
| USER | 146 |
| VOLUME | 147 |
| WORKDIR | 148 |
| ARG | 148 |
| ONBUILD | 149 |
| STOPSIGNAL | 150 |
| HEALTHCHECK | 151 |
| SHELL | 152 |
| 使用 Dockerfiles | 153 |
| 摘要 | 156 |
| 第 7 章：运行容器 | 158 |
| 运行时模式 – 分离和前台 | 160 |
| 分离 | 160 |
| 前台 | 161 |
| 识别镜像和容器 | 162 |
| PID 设置 | 163 |
| UTS 设置 | 164 |
| 覆盖 Dockerfile 中的默认命令 | 165 |
| 覆盖 CMD | 165 |
| 覆盖 ENTRYPOINT | 166 |
| 使用 exec 执行任意命令 | 167 |
| 监控容器 | 168 |
| 查看日志 | 169 |
| 容器事件 | 171 |
| 检查容器 | 172 |
| 统计信息 | 174 |
| 容器退出代码和重启策略 | 175 |
| no | 177 |
| always | 177 |
| on-failure | 177 |
| unless-stopped | 178 |
| 更新正在运行容器的重启策略 | 179 |
| 资源运行时限制 | 181 |
| 内存 | 181
