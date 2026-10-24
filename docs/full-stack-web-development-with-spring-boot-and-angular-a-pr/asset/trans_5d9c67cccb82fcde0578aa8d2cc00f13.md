

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

## Spring Boot 和 Angular – 全景图

首先，我们想感谢您购买本书，本书为开发者创建，旨在学习如何使用开发行业中使用的标准构建全栈 Web 应用程序。本书是基于我们在培训和研讨会中开发的应用程序量身定制的。所以，让我们开始我们的冒险吧。

本章将作为 Java Spring Boot 和 Angular 基础的简短回顾，让您对进行它们的 Web 开发将涉及的内容有一个概念。您还将了解到社区规模之大，以及 Angular 可用的支持使其成为开发应用程序的可靠选择。

在本章中，我们将涵盖以下主题：

- 介绍 Spring Boot
- 使用 Spring Boot 的优势
- Java 17 的新特性
- 介绍 Angular
- 使用 Angular 的优势

## 技术要求

我们将构建的应用程序的 GitHub 仓库可以在 https://github.com/PacktPublishing/Spring-Boot-and-Angular 找到。

每个章节都有一个目录，其中包含项目已完成的<|message|>

> **注意**
> 
> 由于我们将涵盖的大多数主题将由理论和一些示例代码组成，因此 *第 1 章 Spring Boot and Angular – The Big Picture* 至 *第 4 章 设置数据库和 Spring Data JPA* 不会有任何目录。实际项目将在 *第 5 章 使用 Spring 构建 API* 开始。

## 介绍 Spring Boot

**Spring Boot** 是 Pivotal 的开源微框架。它是企业级框架，供开发者在 **Java 虚拟机（JVMs）** 上创建独立应用程序。其主要重点是缩短您的代码，以便更轻松地运行应用程序。

该框架扩展了 Spring Framework，为您提供了一种更具主见的配置应用程序方式。此外，它还具有内置的自动配置功能，可根据您的设置配置 Spring Framework 和第三方包。Spring Boot 利用这些知识避免在配置时出现代码错误，因为它在设置应用程序时减少了样板代码。

现在，让我们讨论使用 Spring Boot 的主要优势。

## 使用 Spring Boot 的优势

以下是使用 Spring Boot 开发应用程序的四个主要优势：

- **自动配置：** 当您配置 Spring Boot 应用程序时，它会下载运行应用程序所需的所有依赖项。它还将根据您应用的设置，使用相关第三方包配置您的 Spring Framework。因此，Spring Boot 避免了样板代码和配置错误，您可以直接开始开发 Spring 应用程序。
- **主见的方法：** Spring Boot 使用基于应用程序需求的狭窄方法安装依赖项。它将安装应用程序所需的所有软件包，并取消手动配置的想法。
- **Spring 启动器：** 您可以在初始化过程中选择一系列启动依赖项来定义应用程序的预期需求。Spring Starter 的一个例子是 Spring Web，它允许您在无需配置运行应用程序所需的依赖项的情况下初始化基于 Spring 的 Web 应用程序。相反，它将自动安装 Apache Tomcat Web 服务器和用于身份验证功能的 Spring Security。
- **创建独立应用程序：** Spring Boot 可以运行不依赖外部 Web 服务器的独立应用程序。例如，我们可以嵌入 Tomcat 服务器并运行应用程序。

