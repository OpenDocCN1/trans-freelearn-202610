# Spring Boot 和 Angular 全栈 Web 开发：构建你的全栈 Web 应用实用指南






谨以此书献给我的父亲 Alberto Duldulao，我想你，tay（爸爸）。失去你的那一天，我失去了一位父亲、一位朋友，还有我敬仰的偶像。你永远在我心中，tay。我每天都想念你。再次感谢你的爱、支持和启迪。

—— Devlin Duldulao (Utoy)

献给我的母亲 Arceli Villafranca，她为我付出了一切，也是我在此的原因。献给我的妹妹 Sheila Villafranca，她始终支持我，以及我的伴侣 Shey Cadavero，她每天都给我力量和灵感。

—— Seiji Villafranca

## 关于审稿人

**Rajeshkumar Muthaiah** 在一家国际银行担任技术主管。自2010年以来，他在使用各种技术设计和构建大规模应用方面拥有丰富的经验。他涉足多种技术，包括 Java、Dart、Flutter、Angular、React、Spring、Hibernate 和 Sass。Rajeshkumar 曾在航空、银行和金融等多个行业工作。他对学习新技术充满热情。

不写代码时，他喜欢阅读书籍、解谜和旅行。他非常喜欢魔方，如5x5、五魔方、金字塔魔方等。目前，他与妻子 Ranju 居住在新加坡。




## Part 3: 前端开发

### 10

| Section | Page |
|---|---:|
| **设置我们的 Angular 项目和架构** | **209** |
| **技术要求** | **209** |
| **组织功能和模块** | **209** |
| 生成一个 Angular 项目 | 210 |
| Angular 功能 | 214 |
| 创建 Angular 文件夹结构 | 220 |
| 实现结构 | 222 |
| **结构化组件** | **227** |
| 智能组件和哑或展示组件 | 228 |
| 在项目中实现智能和哑组件 | 232 |
| **添加 Angular Material** | **236** |
| 配置 Angular Material | 237 |
| 实现 Angular Material | 238 |
| **总结** | **248** |

### 11

| Section | Page |
|---|---:|
| **构建响应式表单** | **249** |
| **技术要求** | **249** |
| **理解响应式表单** | **250** |
| 模板驱动方法 | 250 |
| 响应式方法 | 253 |
| **基本表单控件** | **255** |
| 表单控件方法 | 256 |
| 初始化表单控件 | 257 |
| 重置表单控件 | 258 |
| 监听事件 | 259 |
| **分组表单控件** | **260** |
| 创建表单组 | 260 |
| 创建嵌套表单组 | 261 |
| 创建表单数组 | 263 |
| **使用 FormBuilder 服务生成控件** | **265** |
| **验证表单输入** | **267** |
| 内置验证器 | 267 |
| 自定义验证器 | 268 |
| 在我们的项目中实现响应式表单 | 269 |
| **总结** | **273** |

### 12
### 使用 NgRx 管理状态 275

- 技术要求 275
- 理解大型应用中管理状态的复杂性 276
  - 无状态管理的数据流 276
  - 有状态管理的数据流 277
- 状态管理与全局状态 278
  - 全局状态/存储 278
  - 动作 278
  - 归约器 279
  - 选择器 279
  - 效果 280
- 开始使用并设置 NgRx 283
  - 将 NgRx 添加到我们的 Angular 项目 283
  - 安装 NgRx DevTools 284
- 编写一个动作 286
  - 在项目中实现动作 286
- 编写一个效果 287
- 编写一个归约器 290
  - 状态接口 290
  - 初始状态 290
  - 归约器函数 291
- 编写选择器并在组件中使用选择器并分派它 292
  - 选择根状态 292
  - 选择功能状态 293
  - 在我们的项目中实现选择器 293
- 配置存储 295
  - 运行时检查 295
- 总结 300

### 13
### 使用 NgRx 保存、删除和更新 301

- 技术要求 301
- 使用 NgRx 无副作用地删除一项 301
  - 创建删除动作 302
  - 创建删除归约器 302
  - 分派动作 303
- 使用 NgRx 有副作用地删除一项 305
  - 创建新的动作类型 305
  - 创建删除效果 306
  - 修改分派 306
- 使用 NgRx 有副作用地添加一项 307
  - 创建动作 307
  - 创建效果 308
  - 创建归约器 309
  - 分派动作 310
- 使用 NgRx 有副作用地更新一项 311
  - 创建动作 311
  - 创建效果 312
  - 创建归约器 313
  - 分派动作 314
- 总结 317

### 14
### 在 Angular 中添加身份验证 319

- 技术要求 319
- 添加用户身份验证 319
  - 身份验证 API 320
  - HTTP 拦截器 321
  - 身份验证模块 322
  - 路由模块 325
- 保护路由 332
  - 项目实现 336
- 调用 API 340
  - 创建动作 341
  - 创建效果 341
  - 创建归约器 343
  - 创建选择器 343
  - 在本地存储中同步 344
  - 在组件中分派和选择 345
- 总结 347

### 15
### 在 Angular 中编写测试 349

- 开始使用 Cypress 349
  - 安装 350
  - 更新 Cypress 配置文件 354
- 编写 Cypress 测试 355
- 模拟 HTTP 响应和拦截 HTTP 请求 358
- 总结 369

## Part 4: 部署

### 16
### 使用 Maven 打包后端和前端 373

- 技术要求 373
- 什么是 frontend-maven-plugin？ 374
- 添加 Spring Boot 和 Angular 集成的配置 374
  - 将 Angular 应用添加到 Spring Boot 项目 374
  - 使用 frontend-maven-plugin 376
- 使用 maven-antrun-plugin 377
- 实现 WebMvcConfigurer 378
- 打包 Spring Boot 379
- 运行 JAR 文件 381
- 总结 382

## Preface

Spring Boot 为 JavaScript 用户提供了一个平台，只需几行代码即可启动应用程序。同时，Angular 是一个基于组件的框架，使构建 Web 应用的前端变得简单。本书解释了 Spring Boot 和 Angular 如何协同工作，帮助您快速有效地创建全栈应用程序。

在本书中，您将首先探索 Spring Boot 和 Angular 为何是备受需求的框架，然后由专家解决方案和最佳实践指导您构建自己的 Web 应用程序。关于后端，您将看到 Spring Boot 如何通过让 Spring Framework 和 Spring Boot 扩展来承担繁重工作，从而高效地构建应用程序，同时在项目中使用 Spring Data JPA 和 Postgres 依赖项将数据保存或持久化到数据库中。对于前端，您将使用 Angular 构建项目架构、构建响应式表单，并添加身份验证以防止恶意用户窃取应用程序数据。

最后，您将看到如何使用 Mockito 测试服务，使用持续集成和持续部署部署应用程序，并将 Spring Boot 和 Angular 集成以创建一个单一包，以便到本书结束时，您将能够构建自己的全栈 Web 应用程序。

## Who this book is for

本书面向忙碌的 Java Web 开发者和 TypeScript 开发者，他们在开发 Angular 和 Spring Boot 应用方面经验较少，希望学习构建全栈 Web 应用的最佳实践。

## What this book covers

*第 1 章，Spring Boot 和 Angular – 全面概览*，作为关于 Spring Boot 和 Angular 当前状态的简短回顾，为您提供 Java Spring Boot 和 Angular Web 开发未来的一个 glimpse。您还将看到 Vue.js 作为应用的稳定性和可靠性，以及编写和维护 Vue.js 框架的团队。

*第 2 章，设置开发环境*，教您如何设置计算机的开发环境以构建后端和前端 Web 应用程序。我们将转向不同的 IDE 和文本编辑器来编写代码，并确保在继续应用开发之前一切都已设置好。

*第 3 章，进入 Spring Boot*，揭示了 Spring Boot 的内部工作原理以及如何使用 Spring Initializr 启动项目。本章还将教您依赖注入和 IoC 容器的概念。本章还将讨论 Bean 和注解的工作方式。

*第 4 章，设置数据库和 Spring Data JPA*，帮助您将 Java Spring Boot 连接到数据库。本章将描述 Spring Data JPA 以及如何在项目中添加 Spring Data JPA 和 Postgres 依赖项。本章还将展示如何使用配置文件将 Java Spring Boot 连接到 Postgres 数据库实例。

*第 5 章，使用 Spring 构建 API*，向您展示如何启动和运行 Java Spring Boot 应用程序。本章还将展示如何为应用程序添加模型，并在编写路由器和控制器时使用它们。之后，本章将解释如何使用 Redis 进行缓存以提高应用程序的性能。

*第 6 章，使用 OpenAPI 规范记录 API*，涵盖 Java Spring Boot 应用程序 API 的文档部分。本章还将向您展示如何在应用程序中包含 Swagger UI，以在 API 文档中提供图形界面。

*第 7 章，使用 JWT 添加 Spring Boot 安全*，详细说明 CORS 是什么以及如何在 Spring Boot 应用程序中添加 CORS 策略。本章描述了 Spring 安全、身份验证和授权。本章还将演示 JSON Web Token 的工作原理以及 **身份即服务 (IaaS)** 是什么。

*第 8 章，在 Spring Boot 中记录事件*，解释日志是什么以及实现日志的流行包。本章还将教您在哪里保存日志以及对日志做什么。

*第 9 章，在 Spring Boot 中编写测试*，全部关于为 Java Spring Boot 应用程序编写测试。本章描述了 JUnit 和 AssertJ。本章还将教您如何编写测试，如何测试存储库，以及如何使用 Mockito 测试服务。

*第 10 章，设置我们的 Angular 项目和架构*，重点介绍如何组织功能和模块，如何结构化组件，以及如何添加 Angular Material。

*第 11 章，构建响应式表单*，演示如何构建响应式表单、基本表单控件和分组表单控件。本章还将解释如何使用 FormBuilder 和验证表单输入。

*第 12 章，使用 NgRx 管理状态*，涵盖复杂应用程序中的状态管理。本章还将介绍 NgRx 以及如何在 Angular 应用程序中设置和使用它。

*第 13 章，使用 NgRx 保存、删除和更新*，描述如何使用 NgRx 删除一项，如何使用 NgRx 添加一项，以及如何使用 NgRx 更新一项。

*第 14 章，在 Angular 中添加身份验证*，探讨如何添加用户登录和注销、检索用户配置文件信息、保护应用程序路由以及调用受保护端点的 API。





第 15 章《在 Angular 中编写测试》介绍了如何编写基本的 Cypress 测试以及如何在测试中模拟 HTTP 请求。

第 16 章《使用 Maven 打包后端和前端》举例说明了如何利用 Maven frontend 插件将 Angular 和 Spring Boot 集成到一个包中。

第 17 章《部署 Spring Boot 和 Angular 应用》描述了 CI/CD 和 GitHub Actions。本章还将向您展示如何为 Spring Boot 和 Angular 应用创建 CI 工作流或流水线。

## 如何充分利用本书

您应确保对 HTML、CSS、JavaScript、TypeScript、Java 和 REST API 有基本了解。您不需要对上述要求具备中级或高级知识。

| 书中涵盖的软件/硬件 | 操作系统要求 |
|---|---|
| Node.js（LTS 版本） | Windows、macOS 或 Linux |
| Angular | Windows、macOS 或 Linux |
| Java 17 | Windows、macOS 或 Linux |
| Visual Studio Code | Windows、macOS 或 Linux |
| IntelliJ IDEA | Windows、macOS 或 Linux |
| Google Chrome | Windows、macOS 或 Linux |

如果您在安装运行时、SDK 或开发应用程序时遇到的任何软件工具时遇到问题，不要失去希望。错误很常见，但在排查某些问题时，在 Google 上搜索错误信息会极大地帮助开发者。

如果您使用的是本书的数字版，我们建议您自己输入代码或从本书的 GitHub 仓库访问代码（链接在下一节提供）。这样做将有助于您避免因复制和粘贴代码而产生的任何潜在错误。

在 stackblitz.com 或 codesandbox.io 上尝试使用 Angular，以在不安装任何计算机软件的情况下查看 Angular 的外观和感觉。

## 下载示例代码文件

您可以从 GitHub 下载本书的示例代码文件，地址为 https://github.com/PacktPublishing/Spring-Boot-and-Angular。如果代码有更新，将在 GitHub 仓库中更新。

我们还有来自我们丰富的图书和视频目录的其他代码包，可在 https://github.com/PacktPublishing/ 获取。快来看看吧！

## 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。您可以在此处下载：https://packt.link/pIe6D。

## 使用的约定

本书中使用了以下文本约定。

代码中的代码：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。示例：“Spring Boot 只需要 spring-boot-starter-web，即 Spring Starter，即可运行我们的应用程序。”

代码块设置如下：

```
@Configuration
public class AppConfig
{
    @Bean
    public Student student() {
        return new Student(grades());
    }
    @Bean
    public Grades grades() {
        return new Grades();
    }
}
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    runtimeOnly 'com.h2database:h2'
    runtimeOnly 'org.postgresql:postgresql'
}
```

任何命令行输入或输出都写成如下形式：

```
rpm -ivh jdk-17.interim.update.patch_linux-x64_bin.rpm
```

**粗体：** 表示新术语、重要词语或您在屏幕上看到的词语。例如，菜单或对话框中的词语以**粗体**显示。示例：“选择 **Spring Initializr**，这将打开一个具有相同 Web 界面的表单。”

**提示或重要说明**
显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**盗版：** 如果您在互联网上以任何形式发现我们作品的非法拷贝，我们将不胜感激您提供位置地址或网站名称。请通过 `copyright@packt.com` 联系我们并附上材料链接。

**如果您有兴趣成为作者：** 如果您在某个主题上具有专业知识，并且有兴趣撰写或为一本书做出贡献，请访问 `authors.packtpub.com`。

## 分享您的想法

一旦您阅读了 *Spring Boot 3 and Angular*，我们很想听听您的想法！请为本书选择 `https://www.amazon.in/review/create-review/?asin=180324321X` 并分享您的反馈。

您的评论对我们和技术社区都很重要，并将帮助我们确保提供优质内容。

## 下载本书的免费 PDF 副本

感谢您购买本书！

您喜欢随时阅读但无法随身携带印刷书籍吗？

您的电子书购买与您选择的设备不兼容？

不用担心，现在每本 Packt 书籍都免费提供一本无 DRM 的 PDF 版本。

在任何地方、任何地点、在任何设备上阅读。直接从您喜爱的科技书籍中搜索、复制和粘贴代码到您的应用程序中。

福利还不止于此，您还可以每天通过电子邮件获得独家折扣、新闻通讯和精彩免费内容

按照以下简单步骤获取福利：

+   1. 扫描二维码或访问以下链接

![](img/7c3ddd2e5893c9b5a0220148ed886548_20_0.png)

```
https://packt.link/free-ebook/9781803243214
```

+   2. 提交您的购买证明

+   3. 就这样！我们会直接将您的免费 PDF 和其他福利发送到您的电子邮件
