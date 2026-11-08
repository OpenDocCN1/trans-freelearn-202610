
## 使用 Angular 和 GraphQL 进行全栈开发

学习使用 Apollo、Lerna 和 GraphQL 构建可扩展的 monorepo 和完整的 Angular 应用

## 全栈
开发
使用 Angular 和 GraphQL

学习使用 Apollo、Lerna 和 GraphQL 构建可扩展的 monorepo 和完整的 Angular 应用

Ahmed Bouchefra

![](img/c798729e4c42765fcb1c98950164d0ba_1_0.png)

## 使用 Angular 和 GraphQL 进行全栈开发

版权所有 © 2022 Packt Publishing

*保留所有权利。* 本书的任何部分，未经出版商事先书面许可，不得以任何形式或任何手段复制、存储于检索系统或传播，但用于评论性文章或评论中嵌入的简短引文除外。

在编写本书时已尽最大努力确保所提供信息的准确性。然而，本书所含信息按“原样”出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成或被声称造成的任何损害承担责任。

Packt Publishing 已努力通过适当使用大写字母为本书中提及的所有公司和产品提供商标信息。然而，Packt Publishing 不能保证此信息的准确性。

- **产品组经理**: Pavan Ramchandani
- **出版产品经理**: Bhavya Rao
- **高级编辑**: Hayden Edwards
- **内容开发编辑**: Abhishek Jadhav
- **技术编辑**: Simran Udasi
- **校对编辑**: Safis Editing
- **项目协调员**: Ajesh Devavaram
- **校对员**: Safis Editing
- **索引员**: Sejal Dsilva
- **制作设计师**: Shankar Kalbhor
- **市场协调员**: Anamika Singh

首次出版：2022年2月

生产参考：1280222

由 Packt Publishing Ltd. 出版。
Livery Place
35 Livery Street
Birmingham
B3 2PB, 英国。

ISBN 978-1-80020-246-7

www.packt.com

致我的母亲，以及我父亲的记忆。
- Ahmed Bouchefra

## 贡献者

## 关于作者

**Ahmed Bouchefra** 是软件开发人员和技术作者，拥有软件开发工程学位。他是使用现代技术（如 Angular 和 Node.js）进行 Web 开发的专家，同时也使用 Python 和 Django 等传统技术。他是开源贡献者、开发者以及多个开源 Angular 库（如 ngx-qrcode2）的维护者。他目前还在 techiediaries.com 及其他行业领先网站上撰写关于现代 Web 开发的教程。

## 关于审阅者

**Peter Eijgermans** 是一名长期从事软件开发的开发者，也是 Ordina Netherlands 的一名充满冒险精神和热情的 Codesmith 前端开发者。他喜欢骑自行车环游世界，始终在寻找意想不到和未知的事物。作为工作的一部分，他尝试最新的技术和框架。他喜欢通过在世界各地的会议上演讲以及为荷兰 Java 杂志和 DZone 撰稿来分享自己的经验。他认为前端开发者是网络中的蜘蛛，将用户、项目团队和产品联系在一起。

_我谨感谢以下同事在审阅本书时提供的帮助：_

- _Corneel Eijsbouts_
- _Jeroen Verhoeven_
- _Michael Awad_

## 目录

## 前言

## 第 1 部分：设置开发环境、GraphQL 服务器和数据库

### 1 应用架构和开发环境
- 技术要求 4
- 架构和技术 4
- 全栈架构 4
- 开发技术 6
- 使用 Node.js 在服务器上运行 JavaScript 6
- 使用 npm 安装包 7
- 使用 Express.js 运行 Web 服务器 7
- GraphQL 8
- 使用 Apollo 集成前端和后端 9
- 使用 TypeORM 和 MySQL 保存数据 9
- 使用 TypeScript 和 Angular 构建前端 10
- 安装 MySQL 11
- 配置 MySQL Server 11
- 测试 MySQL Server 12
- 安装和配置 Node.js 13
- 使用 nvm 安装 Node.js 13
- 摘要 15

### 2 使用 Node.js、Express.js 和 Apollo 设置 GraphQL
- 技术要求 18
- 设置 monorepo 项目 19
- 初始化我们的服务器项目 21
- 生成 package.json 文件 21
- 安装 Express.js 和开发依赖项 22
- 创建服务器 25
- 监视并重新编译我们的代码 26
- 创建 GraphQL API 29
- 安装必要的库 29
- 暴露简单的 GraphQL API 30
- 使用 Apollo Studio 发送查询 32
- 为我们的社交网络创建 GraphQL 模式 33
- 模拟我们的 GraphQL API 38
- 链接模拟数据 42
- 配置 CORS 44
- 摘要 45

### 3 使用 TypeORM 连接数据库
- 技术要求 48
- 创建 MySQL 用户和数据库 49
- 设置 TypeORM 和 MySQL 50
- 创建 TypeORM 实体 54
- 填充测试数据 62
- 使用 Apollo 与 TypeORM 68
- 摘要 86

### 4 使用 Apollo Server 实现身份验证和图像上传
- 技术要求 88
- 什么是 JWT？ 89
- 实现身份验证 89
- 保护 GraphQL 查询和变更 97
- 实现图像上传 108
- 练习 111
- 摘要 112

### 5 使用 Apollo Server 添加实时支持
- 技术要求 114
- 理解 GraphQL 订阅 114
- 实现 GraphQL 订阅 115
- 使用订阅实现 JWT 身份验证 120
- 摘要 121

## 第 2 部分：使用实时支持构建 Angular 前端

### 6 Angular 应用架构和路由
- 技术要求 126
- 安装 Angular CLI 128
- Angular CLI 命令 129
- 初始化 Angular 项目 130
- 理解应用架构 132
- 什么是模块？ 133
- 什么是组件？ 134
- 理解服务和依赖注入 136
- 创建模块、服务和组件 138
- 创建模块 138
- 创建组件和服务 140
- 理解 Angular 路由 142
- Angular Router 的工作原理 142
- <base href> 标签 143
- 路由模块 144
- 路由器出口 145
- 导入路由模块 145
- 理解并添加路由 146
- 路由匹配策略 147
- 路由参数 148
- 添加路由 148
- 添加导航 150
- 调试 Angular 应用 152
- 添加 Angular Material 154
- 摘要 155

### 7 添加用户搜索功能
- 技术要求 158
- 介绍并安装 Apollo Client 159
- 导入 Angular Material 组件 161
- 用户注册和登录 163
- 定义 GraphQL 文档 163
- 为用户身份验证定义类型 166
- 定义 Apollo 服务 168
- 实现用户身份验证服务 170
- 构建和设置样式的注册 UI 180
- 构建和设置样式的登录 UI（练习） 189
- 实现页眉组件 191
- 实现页脚和未找到页面组件 195
- 实现用户搜索对话框组件 197
- 摘要 201

### 8 保护路由和测试身份验证
- 技术要求 204
- 保护路由 204
- 发送 JWT 206
- 改进搜索功能（练习） 207
- 使用 GraphQL 片段 208
- 使用 Apollo Client 进行本地状态管理 209
- 使用 OnPush 变更检测策略 219
- 测试 auth 服务和组件 220
- 测试 auth 服务 221
- 测试 auth 守卫 230
- 测试页眉组件 233
- 测试搜索对话框组件 240
- 测试其他组件 240
- 摘要 241

### 9 上传图像和添加帖子
- 技术要求 244
- 使用 Angular 和 Apollo 上传图像 244
- 设置 GraphQL Code Generator 245
- 添加片段和变更 248
- 生成类型和服务 252
- 实现帖子服务 253
- 实现个人资料服务 255
- 实现基础组件 256
- 实现个人资料组件 265
- 定义字段策略 273
- 创建个人资料 UI 274
- 设置个人资料组件样式 277
- 实现创建帖子组件 282
- 测试 289
- 摘要 290

### 10 获取帖子并添加评论和点赞
- 技术要求 292
- 改进身份验证系统 293
- 添加查询并生成类型和服务 294
- 获取分页帖子 299
- 实现评论和点赞服务 302
- 实现展示型帖子组件 305
