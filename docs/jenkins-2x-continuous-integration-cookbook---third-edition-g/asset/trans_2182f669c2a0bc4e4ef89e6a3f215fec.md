

## 本书适合谁

_Jenkins Continuous Integration Cookbook_ 适合初学者和高级用户。本书面向参与应用程序开发生命周期的开发人员和系统管理员，并希望对其进行自动化。开发人员、技术负责人、测试人员和运维专业人员是本书的目标读者，可快速启动 Jenkins。读者应了解开发和运维团队面临的问题，因为他们是应用程序生命周期管理流程中的利益相关者。启动 Jenkins 的原因是为了了解持续集成（CI）、持续测试和持续交付（CD）对有效应用程序生命周期管理所做的重要贡献。

## 约定

在本书中，您会找到多种文本样式来区分不同类型的信息。以下是这些样式的示例及其含义。

正文中出现的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号显示如下："We can include other contexts through the use of the include directive."

代码块设置如下：

```
@charset "utf-8";
#test {
background-image: url(/userContent/camera.png);
}
#main-table{
background-image: url(/userContent/camera.png)
!important;
}
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
http://maven.apache.org/maven-v4_0_0.xsd">
<modelVersion>4.0.0</modelVersion>
<groupId>nl.uva.berg</groupId>
```

任何命令行输入或输出都写成如下形式：

```
---Project and session
Project: class org.apache.Maven.model.Model
Session: class org.apache.Maven.execution.MavenSession
longname: SuperGood
```

**新术语**和**重要词语**以粗体显示。您在屏幕上看到的词语，例如菜单或对话框中的词语，在文本中显示如下："Clicking the Next button moves you to the next screen."

警告或重要说明以如下框显示。

提示和技巧以如下方式显示。

## 读者反馈

我们非常欢迎读者的反馈。请告诉我们您对本书的看法——您喜欢什么或可能不喜欢什么。读者反馈对我们开发您真正受益的书名非常重要。

如需发送一般反馈，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中提及书名即可。

## 客户支持

现在您已成为 Packt 书籍的自豪拥有者，我们提供许多帮助您充分利用购买的服务。

## 下载示例代码

您可以从 http://www.packtpub.com 在您的账户中下载本书的示例代码文件。如果您在其他地方购买了本书，您可以访问 http://www.packtpub.com/support 并注册以便直接将文件发送到您的邮箱。

您可以通过以下步骤下载代码文件：

- 1. 使用您的电子邮件地址和密码登录或注册我们的网站。
- 2. 将鼠标指针悬停在顶部 SUPPORT 选项卡上。
- 3. 点击 Code Downloads & Errata。
- 4. 在搜索框中输入书名。
- 5. 选择您要下载代码文件的书籍。
- 6. 从下拉菜单中选择您购买本书的渠道。
- 7. 点击 Code Download。

下载文件后，请务必使用最新版本解压缩或提取文件夹：

- • WinRAR / 7-Zip for Windows
- • Zipeg / iZip / UnRarX for Mac
- • 7-Zip / PeaZip for Linux

本书的代码包也托管在 GitHub 上，地址为 https://github.com/PacktPublishing/Jenkins-Continuous-Integration-Cookbook-Third-Edition。我们还有来自我们丰富的图书和视频目录的其他代码包，地址为 https://github.com/PacktPublishing/。请查看！

## 下载本书的彩色图片

我们还为您提供一份 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图片。彩色图片将帮助您更好地理解输出中的变化。您可以从 https://www.packtpub.com/sites/default/files/downloads/JenkinsContinuousIntegrationCook 下载此文件

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误难免。如果您在我们的书籍中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。通过这样做，您可以拯救其他读者免受挫折，并帮助我们改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择您的书籍，点击 Errata Submission Form 链接，并输入勘误的详细信息。一旦您的勘误得到验证，您的提交将被接受，勘误将被上传到我们的网站或添加到该标题的勘误部分中的现有勘误列表中。

要查看之前提交的勘误，请访问 https://www.packtpub.com/books/content/support 并在搜索字段中输入书名。所需信息将显示在勘误部分下。

## 盗版

互联网上版权材料的盗版是一个持续存在的问题，涵盖所有媒体。在 Packt，我们非常重视版权和许可的保护。如果您在互联网上任何形式的非法复制品，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过 <copyright@packtpub.com> 联系我们，并附上疑似盗版材料的链接。

我们感谢您保护我们的作者以及我们为您提供有价值内容的能力。

## 问题

如果您对本书的任何方面有问题，您可以联系我们 questions@packtpub.com，我们将尽力解决问题。

## 快速开始 Jenkins

在本章中，我们将讨论如何安装和配置 Jenkins，以及从 Jenkins 2 及更高版本中可用的新功能或 UI 改进。我们将涵盖以下配方：

- 在 Windows 上安装 Jenkins 2
- 在 CentOS 上安装 Jenkins 2
- 在 Azure 上安装 Jenkins 2
- 在 Windows 上将 Jenkins 安装为服务
- 在 Jenkins 中安装插件
- 在 Jenkins 中上传插件
- 在 Jenkins 中配置代理
- 在 Jenkins 中配置全局设置
- 配置 JENKINS_HOME
- 了解 JENKINS_HOME 目录
- 为 Jenkins 使用不同的端口
- 在 Jenkins 中配置 JAVA_HOME
- 在 Jenkins 中配置 Git
- 在 Jenkins 中配置 ANT_HOME
- 在 Jenkins 中配置 MAVEN_HOME
- 在 Jenkins 中配置 GRADLE_HOME
- 为 Ant 项目创建 Freestyle 作业
- 为 Maven 项目创建 Maven 作业

## 引言

Jenkins 是一个开源自动化服务器，被许多组织广泛用于实施流行的 DevOps 实践，例如持续集成和持续交付。Jenkins 功能丰富，并且可通过插件大幅扩展。此外，Jenkins 及其插件改进迅速。Jenkins 每周都会发布新的次要版本，主要是改进，偶尔会有错误。社区通过使用 Jenkins 的长期支持版本来管理核心稳定性，该版本成熟且与最新版本相比功能较少。对于复杂环境中的稳定系统，您需要监控、清理存储、备份、控制 Jenkins 脚本，并持续清理和打磨。本章包含最常见任务的配方。适当的维护可降低故障风险，例如：

- **新插件导致异常**：有很多优秀的插件以快速的版本变化编写。在这种情况下，很容易意外添加带有新缺陷的插件新版本。在升级期间，曾多次出现插件突然无法工作的情况。为对抗插件异常的风险，请考虑在发布到关键系统之前先使用测试 Jenkins 实例。
- **存储因工件溢出**：如果您保留包含工件的构建历史，例如 WAR 文件、大量 JAR 文件或其他类型的二进制文件和源代码，那么您的存储空间将被惊人的速度消耗。存储成本已大幅下降，但存储使用量相当于更长的备份时间和从从节点到主节点的更多通信。为最大限度降低磁盘溢出的风险，您需要考虑备份和恢复策略以及作业高级选项中表达的关联构建保留策略。
- **脚本意大利面条**：由于作业由各个开发团队编写，所包含脚本的位置和风格各不相同。这使您难以跟踪。请考虑为脚本使用明确定义的位置，并通过插件管理的脚本存储库。
- **资源耗尽**：随着内存被消耗，或密集作业数量增加，Jenkins 会变慢。适当的监控和快速反应可减少影响。
- **由于有机增长导致作业之间普遍缺乏一致性**：Jenkins 易于安装和使用。无缝打开插件的能力令人上瘾。Jenkins 在组织内的采用速度令人瞠目结舌。如果没有一致的策略，您的团队将引入大量插件，也将引入大量执行相同工作的方式。约定可提高作业的一致性和可读性，从而降低维护成本。

Jenkins 社区正在竭尽全力为您服务。如果您看到问题，请报告回来。

加入社区：

要添加社区错误报告或修改 wiki 页面，您需要在以下位置创建账户：https://wiki.jenkins-ci.org/display/JENKINS/Issue+Tracking。

