

# 学习 DevOps

## 第二版

一本全面指南，帮助通过 Terraform、Azure DevOps、Kubernetes 和 Jenkins 加速 DevOps 文化采用

Mikael Krief

伯明翰—孟买

版权所有 © 2022 Packt Publishing

_保留所有权利._ 未经出版商事先书面许可，本书任何部分不得以任何形式或任何方式复制、存储在检索系统中或传输，但简短引用用于批判性文章或评论的情况除外。

在本书编写过程中，已尽力确保所呈现信息的准确性。但是，本书包含的信息是不带任何明示或暗示担保地出售的。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成或声称造成的任何损害承担责任。

Packt Publishing 已努力对本书中提及的所有公司和产品提供商标信息，并适当使用大写字母。但是，Packt Publishing 不能保证此信息的准确性。

**团队产品经理：** Rahul Nair  
**出版产品经理：** Preet Ahuja  
**高级编辑：** Shazeen Iqbal  
**内容开发编辑：** Romy Dias  
**技术编辑：** Arjun Varma  
**复制编辑：** Safis Editing  
**项目协调员：** Shagun Saini  
**校对：** Safis Editing  
**索引编制：** Hemangini Bari  
**制作设计师：** Jyoti Chauhan  
**市场协调员：** Nimisha Dua

首次出版：2019 年 10 月  
第二版：2022 年 2 月  

制作参考编号：1240222  

由 Packt Publishing Ltd. 出版  
Livery Place  
35 Livery Street  
Birmingham  
B3 2PB, UK.  

ISBN 978-1-80181-896-4  
www.packt.com  

我谨将此书献给我的妻子和孩子们，她们是我幸福的源泉。

# 贡献者

## 关于作者

**Mikael Krief**（生于 1980 年）居住在法国，担任 DevOps 工程师。

他喜欢通过各种社区（例如 HashiCorp 用户组）分享他的热情。2019 年，他撰写了本书的第一版，2020 年，他撰写了 _Terraform Cookbook_（Packt Publishing），并且还为许多公共项目做出贡献，撰写博客和书籍，并在会议上发表演讲。

他对 HashiCorp 产品感兴趣，并专精于在多个公司环境中使用 Terraform。

由于他的所有贡献和热情，他获得了微软 **最有价值专家**（MVP）奖，微软已连续 6 年授予他该奖项，并且自 2020 年起他被提名并选为 HashiCorp 大使。

_我要感谢我的家人接受我在家庭时间里需要长时间工作来撰写本书。我要感谢 Meeta Rajani 给我机会撰写本书的第二版，这是一次非常丰富的经历。特别感谢 Romy Dias 和 Vaidehi Sawant 对本书审阅提供的宝贵意见和时间，以及整个 Packt 团队在本书编写过程中的支持。_

# 关于审稿人

**Kevin Bridges** 是 Alegeus 的高级 DevOps 工程师。Kevin 使用了多种支持稳定版本发布和热修复的工具。他相对新接触云领域（Azure），到目前为止很享受，并且于 2021 年 6 月刚刚获得 Azure 基础认证。

Kevin 在陆军服役 20 年，曾担任现役和预备役。他于 2009 年毕业于 Granite State College，获得信息技术理学学士学位，并于 2015 年 5 月毕业于新罕布什尔大学（曼彻斯特分校），获得工商管理硕士学位。

Kevin 喜欢将复杂的事情变得简单。

> _我要感谢我的家人、朋友和同事的所有支持_

**Deb Bhattacharya** 应用敏捷和 DevOps 使组织更加成功。20 多年来，Deb 帮助 4 个国家的 50 多个团队更加敏捷并更加采用 DevOps。Deb 对此充满热情。

Deb 的另一热情是体育。Deb 年轻时曾打过职业乒乓球。他赢得了许多锦标赛，但最重要的是，他指导了多支获奖的乒乓球队。Deb 至今仍喜欢打乒乓球。

Deb 利用其在软件开发方面的实践背景和体育背景的所有经验，来开发高性能的敏捷和 DevOps 团队。这两种热情在这里完美结合。

> _我感谢 Anand Athani，大约 20 年前我担任工程负责人时向他汇报工作的交付经理。当时我正在撰写硕士论文，Anand 建议使用 Rational Unified Process（RUP）。这开启了我的敏捷之旅。学习和贡献仍在继续。_

# 目录

## 前言

## 第 1 部分：DevOps 与基础设施即代码

## 1

### DevOps 文化与基础设施即代码实践

- DevOps 入门 4
- 实施 CI/CD 和持续部署 7
- 持续集成（CI） 7
- 持续交付（CD） 10
- 持续部署 12
- 了解 IaC 实践 13
- IaC 的优势 14
- IaC 语言和工具 14
- IaC 拓扑 18
- DevOps 文化的演进 24
- 总结 24
- 问题 25
- 进一步阅读 25

## 2

### 使用 Terraform 配置云基础设施

- 技术要求 28
- 安装 Terraform 28
- 手动安装 29
- 通过脚本安装 29
- 将 Terraform 与 Azure Cloud Shell 集成 34
- 为 Azure 配置 Terraform 36
- 创建 Azure SP 36
- 配置 Terraform 提供程序 38
- 用于本地开发和测试的 Terraform 配置 39
- 编写 Terraform 脚本以部署 Azure 基础设施 40
- 遵循一些 Terraform 最佳实践 45

## 3 使用 Ansible 配置 IaaS 基础设施

- 技术要求 68
- 安装 Ansible 69
- 使用脚本安装 Ansible 69
- 将 Ansible 集成到 Azure Cloud Shell 71
- Ansible 工件 73
- 配置 Ansible 73
- 创建 Ansible 清单 76
- 清单文件 76
- 在清单中配置主机 78
- 测试清单 79
- 执行第一个 playbook 80
- 编写基本 playbook 81
- 了解 Ansible 模块 82
- 使用角色改进 playbook 83
- 执行 Ansible 85
- 使用预览或干运行选项 87
- 增加日志级别输出 88
- 使用 Ansible Vault 保护数据 89
- 在 Ansible 中使用变量进行更好的配置 89
- 使用 Ansible Vault 保护敏感数据 92
- 为 Azure 基础设施使用动态清单 95
- 总结 99
- 问题 100
- 进一步阅读 100

## 4 使用 Packer 优化基础设施部署

- 技术要求 103
- Packer 概述 103
- 安装 Packer 104
- 使用脚本为 Azure VM 创建 Packer 模板 109
- Packer 模板的结构 110
- 使用 Packer 模板构建 Azure 镜像 116
- 在 Packer 模板中使用 Ansible 120
- 编写 Ansible playbook 120
- 将 Ansible playbook 集成到 Packer 模板中 121
- 执行 Packer 123
- 配置 Packer 以进行 Azure 身份验证 123
- 检查 Packer 模板的有效性 124
- 运行 Packer 生成 VM 镜像 124
- 使用 HCL 格式编写 Packer 模板 127
- 将 Packer 镜像与 Terraform 一起使用 131
- 总结 132
- 问题 133
- 进一步阅读 133

## 5 使用 Vagrant 编写开发环境

- 技术要求 136
- 安装 Vagrant 136
- 在 Windows 上手动安装 136
- 在 Windows 上通过脚本安装 Vagrant 138
- 在 Linux 上通过脚本安装 Vagrant 139
- 编写 Vagrant 配置文件 140
- 使用 Vagrant Cloud 获取 Vagrant Boxes 140
- 编写 Vagrant 配置文件 142
- 使用 Vagrant CLI 创建本地 VM 145
- 创建 VM 146
- 连接到 VM 147
- 总结 148
- 问题 148
- 进一步阅读 149

## 第 2 部分：DevOps CI/CD 管道

## 6 使用 Git 管理源代码

- 技术要求 154
- Git 及其主要命令行概述 154
- Git 安装 157
- Git 配置 166
- 有用的 Git 词汇 166
- Git 命令行 168
- 了解 Git 流程和 Gitflow 模式 171
- 开始使用 Git 流程 172
- 使用分支隔离代码 181

