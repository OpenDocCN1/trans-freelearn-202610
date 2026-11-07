
Matt Dorn

## 为 OpenStack 认证管理员考试做准备

考生完整指南

![](img/da864e0670c65edf6f5ab861ef9906b3_0_0.png)

Packt

由 Internet Archive 数字化
2022 年由
Kahle/Austin Foundation 资助

## 为 OpenStack 认证管理员考试做准备

考生完整指南

Matt Dorn

![](img/da864e0670c65edf6f5ab861ef9906b3_2_0.png)

BIRMINGHAM - MUMBAI

## 为 OpenStack 认证管理员考试做准备

版权所有 © 2017 Packt Publishing

保留所有权利。本书的任何部分，未经出版商事先书面许可，不得以任何形式或任何手段复制、存储在检索系统中或传播，但嵌入在评论或评论文章中的简短引文除外。

在编写本书时已尽一切努力确保所提供信息的准确性。然而，本书中包含的信息按原样出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成或声称造成的任何损害承担责任。

Packt Publishing 已尽力提供有关本书中提及的所有公司和产品的商标信息，并以适当的方式使用大写字母。然而，Packt Publishing 不能保证该信息的准确性。

首次出版：2017 年 8 月

生产参考：1220817

Packt Publishing Ltd 出版。
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-78728-841-6

www.packtpub.com

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

## 目录

- 前言 1
- 第 1 章：介绍 OpenStack 和 OpenStack 认证管理员考试 7
  - 云的简史 9
    - 软件开发人员的困境 9
    - 企业虚拟化的诞生 10
    - 亚马逊 - 不仅仅是书籍商店 12
    - 亚马逊的抱怨 14
    - NASA 和 Rackspace 开源云！ 15
  - 关于 OpenStack 15
    - OpenStack 基金会 16
    - 四个开放 16
  - OpenStack 类型 17
  - OpenStack 职位 18
    - 应用开发人员 - 在 OpenStack 上开发 19
    - OpenStack 管理员/操作员 - 管理云 19
    - OpenStack 工程师/基础设施工程师 - 做 DevOps 的事 19
    - OpenStack 产品开发人员 - 简化 OpenStack 部署/管理 20
    - 上游 OpenStack 开发人员 - 让 OpenStack 更好！ 20
  - OpenStack 服务概述 20
    - Keystone - 身份服务 21
    - Glance - 镜像服务 21
    - Nova - 计算服务 21
    - Neutron - 网络服务 22
    - Cinder - 块存储服务 22
    - Swift - 对象存储服务 22
    - Heat - 编排服务 23
  - OpenStack 服务实战 24
  - 与 OpenStack 交互 26
  - OpenStack 客户端 27
    - 基于服务的客户端 - 传统客户端 28
    - Python-openstackclient - 统一客户端 28
  - OpenStack 守护进程通信 29
  - OpenStack API 通信 31
  - 关于 OpenStack 认证管理员考试 32
    - 通过考试的好处 33
    - 如果你通过了... 33
- 成为 OpenStack 认证管理员的七个步骤 34
- 第 1 步 - 学习！ 35
- 第 2 步 - 购买 35
- 第 3 步 - COA 门户页面 35
- 第 4 步 - 硬件兼容性检查 35
- 第 5 步 - 身份识别 37
- 第 6 步 - 安排考试 37
- 第 7 步 - 参加考试！ 37
- 关于考试环境 38
- 考试目标 39
- 了解 OpenStack（3%）- 第 1 章 40
- Keystone：身份管理（12%）- 第 3 章 40
- Glance：镜像管理（10%）- 第 4 章 40
- Nova：计算（15%）- 第 5 章 40
- Neutron：网络（16%）- 第 6 章 41
- Cinder：块存储（10%）- 第 7 章 41
- Swift：对象存储（10%）- 第 8 章 42
- Heat：编排（8%）- 第 9 章 42
- Horizon：仪表板（3%）- 第 3 章至第 9 章 42
- 故障排除（13%）- 第 10 章 43
- 总结 43
- 第 2 章：设置您的练习考试环境 45
- 关于 VirtualBox 46
- Open Virtual Appliance（OVA） 46
- 传统的 OpenStack 云 47
- 全部在一体的 Newton 环境 48
- 最低硬件要求 50
- 下载和安装 VirtualBox 51
- 配置 VirtualBox 网络 51
- Mac/Linux 52
- Windows 53
- 确认主机专用网络 IP 地址 54
- 导入设备 54
- 通过 SSH 进入设备 56
- Mac/Linux 56
- Windows 56
- 使用 CLI 验证 OpenStack 服务连接 57
- 使用 Horizon 仪表板验证 OpenStack 连接 58
- 总结 60
- 第 3 章：Keystone 身份服务 61
