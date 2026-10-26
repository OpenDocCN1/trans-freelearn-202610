# Ionic 2 精通指南

## 关于作者

开源爱好者，使用 Node.js、AngularJS、electron 和 Ionic 的 JavaScript 全栈开发者。目前正在使用 BLE 信标进行物联网项目。Gaurav 在业余时间研究 VOIP 技术、物联网和 Material Design。他还与 Apache 软件基金会有关联，在 Apache Roller、Apache Fineract 和 Apache OFBiz 项目中担任贡献者和提交者。他曾被选为并受邀在多个会议上担任演讲者，包括 Mifos Techdays、ApacheCON、Mifos Summit 以及许多当地学院/机构。他曾于 2014 年担任 Google 夏季代码学生实习生，并在 2016 年与 Mifos Initiative 合作指导，使用基于 AngularJS 的社区应用。他于 2014 年和 2016 年在旧金山被选为 GSOC 峰会代表。他过去曾在 Google Adwords 和 Analytics 工作，并于 2012 年成为 Google 在线营销挑战赛的半决赛选手。

*我谨向我的父母和已故的叔叔 Raj 表示感谢，没有他们，我很难走到今天并写下这本书。此外，在困难时期，我最好的朋友 Seema 全力支持我继续努力工作。*

*特别感谢我的实习生和内容编辑，他们在本书创作过程中提供了大量帮助。与你周围的优秀人才一起工作是一种乐趣。*

*最后，我想感谢大家对本书的关注，我希望我能传递我加入 Ionic 社区的热情，并将你们的精彩旅程推向新的高度。*

## www.PacktPub.com

有关本书的支持文件和下载，请访问 [www.PacktPub.com](www.PacktPub.com)。

您知道 Packt 提供每本出版书籍的电子书版本，文件格式为 PDF 和 ePub 吗？您可以在 [www.PacktPub.com](www.PacktPub.com) 升级到电子书版本，作为印刷书籍客户，您有权享受电子书的折扣。请通过 service@packtpub.com 与我们联系了解更多详情。

在 [www.PacktPub.com](www.PacktPub.com)，您还可以阅读大量免费技术文章，订阅多种免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_13_0.png)

[https://www.packtpub.com/mapt](https://www.packtpub.com/mapt)

使用 Mapt 获得最受欢迎的软件技能。Mapt 让您完全访问 Packt 的所有书籍和视频课程，以及帮助您规划个人发展和推进职业生涯的行业领先工具。

## 为什么订阅？

- 在 Packt 发表的每本书中完全可搜索
- 复制、粘贴、打印和书签内容
- 按需访问，可通过网络浏览器访问

## 客户反馈

感谢您购买 Packt 书籍。在 Packt，质量是我们编辑流程的核心。为了帮助我们改进，请在本书的 Amazon 页面 https://www.amazon.com/dp/1785286056 上留下诚实的评价。

如果您想加入我们的常驻审阅者团队，您可以发送电子邮件至 customerreviews@packtpub.com。我们会用免费的电子书和视频作为对您宝贵反馈的回报。帮助我们无情地改进我们的产品！

## 本书涵盖内容

第 1 章，*使用 Ionic 3 快速开始*，我们将介绍 Ionic 的所有基础知识，这将有助于构建大规模和企业级应用程序。

第 2 章，*Ionic 组件*，将指导您了解 Ionic 组件以及如何根据我们的应用程序对其进行自定义。

第 3 章，*Ionic Native 和插件*，涵盖了几乎所有关于 Cordova 插件、Ionic Native 以及一些常见和高级插件的内容。

第 4 章，*Ionic 平台和服务*，涵盖了所有 Ionic 平台服务以及如何在您的应用程序中以最佳方式使用它们。

第 5 章，*身份验证、授权和安全*，将处理身份验证、保护 Ionic 应用程序和演示授权。

第 6 章，*使用 Firebase 的 TasteBite 应用*，重点是从头到尾使用后端即服务平台 Firebase 构建整个应用程序。

第 7 章，*Ionic、物联网和信标*，我们将讨论物联网、BLE 和物理网络。此外，还将演示基于 iBeacon 的邻近应用程序，当您远离笔记本电脑时，您的笔记本电脑将自动屏幕锁定。

第 8 章，*Ionic + PWA = 魔法*，我们将研究渐进式 Web 应用及其与 Ionic 的支持，并演示离线优先的货币转换器应用程序。





## 本书所需的准备

你需要使用 Sublime 或 Visual Studio Code 进行代码编辑。这两款软件均可免费下载。对于 BLE 演示，你需要一台支持蓝牙 4 的笔记本电脑或蓝牙 BLE 信标。

## 本书的读者对象

本书面向具备一定 Ionic 基础知识的中级应用程序开发者。对于那些希望将应用从 Ionic 1 迁移到 Ionic 3 的开发者，本书将非常有帮助；同时，想要获取最新技术更新（例如 Ionic 对 PWA 的支持）的开发者也能从中获益。

## 排版约定

在本书中，你会看到多种文本样式，用以区分不同类型的信息。以下是一些样式示例及其含义说明。

文中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入以及 Twitter 用户名将以如下格式显示：“为了设置 API 端点，我们创建了常量文件，并在其中声明了常量 `API_URL`”

代码块如下所示：

```json
{
  "name": "ionic2-auth",
  "app_id": "",
  "v2": true,
  "proxies": [
    {
      "path": "/api",
      "proxyUrl": "http://yourwebsite.com/api"
    }
  ]
}
```

当我们希望引起你对代码块中某一部分的注意时，相关行或项目会以**粗体**显示：

```typescript
// src/providers/constants.ts
// 部署应用时，我们切换到真实 URL

export const API_URL: string = 'http://localhost:8100/api';
```

任何命令行输入或输出将按如下方式书写：

```shell
$ chromium-browser --disable-web-security
```

**新术语和重要词汇**以粗体显示。你在屏幕上看到的文字，例如菜单或对话框中的文字，在文中会以如下形式呈现：“Cancel 和 OK 按钮。我们在这里使用它们来根据相关性、价格或其他排序值对产品进行排序。”

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_32_0.png)

*警告或重要说明会出现在这样的框中。*

![](img/c54e3488d7687ea0d26e8c2ce3cdfae3_32_1.png)

*提示与技巧以这样的形式呈现。*

## 客户支持

既然你已经是 Packt 图书的拥有者，我们提供了多项服务，帮助你充分利用所购内容。

### 下载示例代码

你可以从你的账户中下载本书的示例代码文件，网址为 http://www.packtpub.com。如果你在其他渠道购买了本书，可以访问 http://www.packtpub.com/support 并注册，我们会将文件直接通过电子邮件发送给你。

你可以按照以下步骤下载代码文件：

1. 使用你的电子邮件地址和密码登录或注册我们的网站。
2. 将鼠标指针悬停在顶部的 SUPPORT 选项卡上。
3. 点击 Code Downloads & Errata。
4. 在搜索框中输入书名。
5. 选择你要下载代码文件的那本书。
6. 在下拉菜单中选择你购买本书的渠道。
7. 点击 Code Download。

文件下载完成后，请确保你使用最新版本的解压软件解压或提取文件夹：

- Windows 使用 WinRAR / 7-Zip
- Mac 使用 Zipeg / iZip / UnRarX
- Linux 使用 7-Zip / PeaZip

本书的代码包也托管在 GitHub 上，地址为 https://github.com/PacktPublishing/Hybrid-Mobile-Development-with-Ionic。我们还有其他来自丰富图书和视频目录的代码包，可在 https://github.com/PacktPublishing/ 获取。欢迎查看！

### 下载本书的彩色图片

我们还为你提供了一份 PDF 文件，其中包含本书所使用的截图/图表的彩色图片。这些彩色图片将帮助你更好地理解输出结果的变化。你可以从 https://www.packtpub.com/sites/default/files/downloads/HybridMobileDevelopmentwithIonic_ColorImages.pdf 下载该文件。

### 勘误

尽管我们已尽最大努力确保内容的准确性，但错误仍在所难免。如果你在我们的某本书中发现了错误——可能是文字或代码中的错误——我们将非常感激你能向我们报告。这样做可以帮助其他读者免于受挫，也有助于我们改进本书的后续版本。如果你发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择你的图书，点击 Errata Submission Form 链接，并填写勘误详情。一旦你的勘误得到验证，我们会接受你的提交，并将勘误上传到我们的网站，或添加到该图书现有勘误列表的 Errata 部分。

要查看之前提交的勘误，请访问 https://www.packtpub.com/books/content/support，在搜索字段中输入书名，所需信息将出现在 Errata 部分。

### 盗版

互联网上对受版权保护内容的盗版是所有媒体面临的一个持续问题。在 Packt，我们非常重视版权和许可的保护。如果你在互联网上发现任何形式的、我们作品的非法副本，请立即向我们提供地址或网站名称，以便我们采取补救措施。

请通过 `copyright@packtpub.com` 联系我们，并附上疑似盗版材料的链接。

我们感谢你帮助保护我们的作者以及我们为您带来优质内容的能力。

### 问题

如果你对本书的任何方面有问题，可以通过 `questions@packtpub.com` 联系我们，我们将尽最大努力解决问题。
