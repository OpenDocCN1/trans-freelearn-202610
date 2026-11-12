

## UiPath Associate 认证指南

借助模拟测试和测验，通过 Associate 认证考试的必备指南

Niyaz Ahmed | Lahiru Fernando | Rajaneesh Balakrishnan

## UiPath Associate 认证指南

借助模拟测试和测验，通过 Associate 认证考试的必备指南

Niyaz Ahmed

Lahiru Fernando

Rajaneesh Balakrishnan

![](img/84b06ad2832c53c5f677f8ce4c80f184_1_0.png)

伯明翰—孟买

## UiPath Associate 认证指南

版权所有 © 2022 Packt Publishing

保留所有权利。未经出版商事先书面许可，任何人不得以任何形式或任何手段复制、存储于检索系统中或传播本书的任何部分，但批判性文章或评论中的简短引文除外。

在编写本书的过程中，已尽全力确保所呈现信息的准确性。然而，本书所含信息不附带任何明示或暗示的担保。无论是作者、Packt Publishing 还是其经销商和分销商，均不对因本书直接或间接造成或被声称造成的任何损害承担责任。

Packt Publishing 已努力通过适当使用大写字母，提供本书中提及的所有公司和产品的商标信息。但是，Packt Publishing 不能保证这些信息的准确性。

- **小组产品经理**：Rohit Rajkumar
- **出版产品经理**：Ashitosh Gupta
- **高级编辑**：Hayden Edwards
- **内容开发编辑**：Feza Shaikh
- **技术编辑**：Saurabh Kadave
- **文案编辑**：Safis Editing
- **项目协调员**：Manthan Patel
- **校对**：Safis Editing
- **编目员**：Tejal Daruwale Soni
- **制作设计师**：Joshua Misquitta
- **市场协调员**：Deepak Kumar

首次出版：2022 年 6 月
制作参考编号：1170622

由 Packt Publishing Ltd. 出版。
Livery Place
35 Livery Street
伯明翰
B3 2PB, 英国。

ISBN 978-1-80107-273-1
www.packt.com

献给我的父亲——您肩头的引导之手将永远与我同在。
– Niyaz Ahmed

我想感谢我的父亲 Sesha、我的妻子 Mihiri 和我的女儿 Emmie，在人生的所有艰难时刻都扶着我的手。同时，特别感谢我两位出色的导师 Erik Gillet 和 Lucas Pimenta，感谢他们始终激励和指导我。最后，我想感谢整个 UiPath 社区、UiPath 团队、我在 Boundaryless Group 的团队以及所有朋友，感谢他们提供的所有机会，并在许多方面帮助我改变人生。
– Lahiru Fernando

献给任何想要通过 UiPath Associate 认证、开始学习 UiPath、转向 UiPath 职业生涯，或仅仅想了解这项技术的人——我相信本书无疑将帮助您实现 UiPath 目标。感谢合著者、评审员和 Packt Publishing 团队的出色协作，他们以最简化的方式创建本书内容，帮助任何人学习 UiPath 并获得 UiPath Associate 认证。

献给我的妻子 Anusree、儿子 Saijesh 和女儿 Dakshya，感谢他们允许我体验撰写本书的过程。
– Rajaneesh Balakrishnan

## 贡献者

## 关于作者

**Niyaz Ahmed** 是 UiPath 的技术项目经理，致力于使当前和未来的劳动力为自动化就绪的世界做好准备。他通过与领先的机构（包括大学、学院和高中）合作，管理 UiPath Academic Alliance 和 Learning Partner 计划。他还是 Udacity RPA 开发者纳米学位课程的讲师。此外，他通过指导学习者开发专注于开发和部署软件机器人的专业级技能，继续传播 RPA。

> *特别感谢 Alok Shrivastava、Rajesh Nambiar、Leah Hojem 以及所有在本书不同阶段帮助过我的 UiPath 同事。*

**Lahiru Fernando** 是 UiPath MVP、RPA 执行团队负责人、UiPath 社区领袖、导师、讲师和公众演讲者，在 IT 行业拥有 11 年以上的经验。他于 2011 年在 Staffordshire University 完成专业软件工程学位后开始了自己的职业生涯。他曾在银行、金融、市场研究和农业领域参与多个商业智能项目。2019 年转入 RPA 领域后，他作为首席 RPA 解决方案架构师，在金融、零售、银行、儿童福利、石油和天然气、电信及服装领域主导了多个成功的 RPA 项目。他还负责 Boundaryless Automation 的 RPA 赋能和培训工作。

Rajaneesh Balakrishnan 是 2AutomateAnything 的创始人，自 2020 年以来一直是 UiPath 高级认证 RPA 开发者和 UiPath MVP。他最初从事质量分析师工作，后来成为软件测试实践负责人，随后成为拥有大量自动化工具实施经验和多个领域经验（包括 BFSI）的 RPA 负责人。他在 IT 行业拥有 15 年以上的经验，已为许多知名客户成功实施了 UiPath RPA，为组织节省了大量时间和成本。他也是一位充满热情的教师，举办各种企业和个性化培训活动，指导许多人转向 UiPath 的 RPA 职业生涯。他曾在 YouTube、Udemy 和 LinkedIn 上分享教育内容，以支持 UiPath 社区。

## 关于审稿人

在获得计算机科学硕士学位后，**Mukesh Kala** 开始了软件开发员的职业生涯。他拥有 8 年以上的专业经验，目前担任高级 RPA 分析师，负责银行和金融服务行业的自动化工作。

作为学习和知识共享力量的坚定信徒，Mukesh 定期参加 UiPath 社区活动，培训并帮助技术初学者。Mukesh 于 2021 年被评为 UiPath MVP，并运营着 YouTube 频道 _Mukesh Kala 的教程_。在闲暇时间，他喜欢阅读书籍和玩魔方。

**Andy Menon** 是 2021 年的 UiPath MVP。在日常工作中，他是一名集成工程师，拥有为企业集成系统的经验。

Andy 也是 RPA Vanguard 的创始人，以及同名 YouTube 频道的创办人，该频道通过真实案例教授 RPA。他在 Udemy 上的企业 RPA 课程在 CourseMarks 近 100,000 门课程中排名前 20%。

在闲暇时间，Andy 喜欢钻研业余电子、3D 打印和 **物联网（IoT）**，尝试使用 Arduino、Raspberry Pi 和 ESP8266 系列微控制器。

Andy 也是 _赋予自己 IoT 超能力_ 一书的 Kindle 出版作者。

他已在 Instructables 和 Thingiverse 上在线发布了几个业余项目和设计。

## 目录

## 前言

## 第 1 部分：RPA 的重要性

### 1 自动化之旅与确定合适的业务流程

- 技术要求 ... 4
- 了解自动化如何推动数字化转型 ... 5
- 在商业中实施 RPA ... 7
- 薪资处理 ... 9
- 客户信息管理 ... 9
- 发票处理 ... 10
- 财务报表对账 ... 10
- 呼叫中心自动化 ... 10
- 贷款处理 ... 11
- RPA 可以自动化什么？ ... 11
- 为 RPA 确定流程 ... 13
- 了解 RPA 之旅的阶段 ... 17
- 发现 ... 17
- 构建 ... 18
- 管理 ... 19
- 运行 ... 19
- 参与 ... 19
- 衡量 ... 19
- 了解 RPA 项目中的 RPA 资源和职责 ... 20
- RPA 解决方案架构师 ... 20
- RPA 开发者 ... 20
- BA ... 20
- 实施经理 ... 21
- 基础设施工程师/经理 ... 21
- RPA 支持 ... 21
- 摘要 ... 21

### 2 UiPath 生态系统

- 技术要求 24
- UiPath 企业级 RPA 平台概览 24
- 发现 26
- 构建 27
- 管理 28
- 运行 29
- 参与 29
- 设置 UiPath Studio 30
- 探索不同版本 30
- 安装 UiPath Studio 31

- 探索 UiPath Studio 界面 36
- 设计器面板 38
- 项目面板 38
- 活动面板 40
- 属性面板 40
- 变量面板 41
- 参数面板 42
- 输出面板 43
- 大纲面板 44
- 摘要 44

### 3 介绍 UiPath Orchestrator

- 技术要求 46
- 创建您的首个云端 Orchestrator 实例 46
- 介绍 UiPath 云平台 47
- 主屏幕和资源中心 48
- 操作 48
- 应用 49
- 流程 51
- 数据服务 52
- AI Fabric 53
- 管理 53
- 介绍 UiPath Orchestrator 58

- 了解 UiPath Orchestrator 的基本概念 58
- 机器人 59
- 文件夹 60
- 环境 62
- 组织单元 62
- 包 62
- 流程 64
- 作业 66
- 触发器 68
- 资产 72
- 队列 75
- 摘要 79

