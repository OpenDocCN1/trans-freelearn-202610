# 前言

Azure Cosmos DB 是微软面向个人和企业的托管数据库解决方案，提供行业标准的服务级别协议。Azure Cosmos DB 提供多模型数据库支持，可跨全球 30 多个区域进行扩展。Azure Cosmos DB 提供一站式分发，使数据库管理员能够通过几次点击即可扩展和复制其数据库。

本书涵盖了 Azure Cosmos DB 的主要概念和功能。本书还涵盖了使用 Cosmos DB 开发 Web 应用程序，以及使用 Azure Cosmos DB 提供的软件开发工具包进行集成。

## 适读人群

本书适合任何想要精通微软新型 NoSQL 数据库 Azure Cosmos DB 的人阅读。您应具备处理任何 SQL 或 NoSQL 数据库的经验。

## 本书内容

**第 1 章**，*Microsoft Azure 云与 Cosmos DB 简介*，旨在介绍 Microsoft Azure 平台。读者将了解 Microsoft Azure 云提供的各种界面和模块。本章的另一部分介绍了 Cosmos DB，这是本书的主题。



### 章节

- 第 2 章，《*Cosmos DB 全球交钥匙分布*》—— 涵盖 Cosmos DB 的关键功能之一，即其全球交钥匙分布系统。您可以使用非常简单的界面，将数据库复制到遍布全球的 30 多个区域。
- 第 3 章，《*Cosmos DB 多模型数据库*》—— 提供了四种（本书出版时为五种）可供选择的数据库模型。本章详细介绍这些数据库模型。
- 第 4 章，《*查询 Cosmos DB*》—— 涵盖多模型数据库所需的不同查询类型。本章按 CRUD（创建、读取、更新和删除）方式对这些查询进行分类，以便更好地解释。
- 第 5 章，《*将 Cosmos DB 与 Node.js 集成*》—— 涵盖与流行的后端运行时引擎 `Node.js` 的所有集成。我们介绍了如何使用 `Node.js` 连接到 Cosmos DB 的数据库模型。
- 第 6 章，《*使用 Cosmos DB 构建高性能链接缩短与追踪系统*》—— 涵盖使用 Cosmos DB 和 `Node.js` 开发一个简单且高性能的链接缩短与追踪系统，该系统非常类似于 `bit.ly`。读者将学习并了解如何使用 Cosmos DB 托管数据库解决方案构建此类复杂系统。
- 第 7 章，《*使用 Cosmos DB 构建高可用实时邮件追踪系统*》—— 涵盖开发一个处理数百万用户邮件打开率追踪并将其存储在数据库中以供进一步分析的系统。
- 第 8 章，《*Cosmos DB 部署与管理*》—— 涵盖将您的自定义应用程序和 Cosmos DB 数据库部署到生产环境的所有内容。

## 如何充分利用本书

安装最新版本的 `Node.js` 并学习 `MongoDB` 查询基础知识，以充分利用本书。

## 下载示例代码文件

您可以从 `www.packtpub.com` 的帐户中下载本书的示例代码文件。如果您在其他地方购买了本书，可以访问 `www.packtpub.com/support` 并注册，以便将文件直接发送到您的邮箱。

您可以按照以下步骤下载代码文件：

1. 在 `www.packtpub.com` 登录或注册。
2. 选择 **支持** 选项卡。
3. 点击 **代码下载与勘误**。
4. 在搜索框中输入书名，然后按照屏幕上的说明操作。

文件下载完成后，请务必使用最新版本的以下工具解压或提取文件夹：

- `WinRAR/7-Zip` (Windows)
- `Zipeg/iZip/UnRarX` (Mac)
- `7-Zip/PeaZip` (Linux)

本书的代码包也托管在 GitHub 上：`https://github.com/PacktPublishing/Learning-Azure-Cosmos-DB`。我们还在 `https://github.com/PacktPublishing/` 上提供了来自我们丰富图书和视频目录的其他代码包。快去看看吧！

## 下载彩色图片

我们还提供了一个 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图片。您可以在此处下载：`http://www.packtpub.com/sites/default/files/downloads/LearningAzureCosmosDB_ColorImages.pdf`。

## 约定用法

全书使用了许多文本约定。

`CodeInText`：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如："Let's perform an `UPDATE` and `DELETE` operation."（让我们执行 `UPDATE` 和 `DELETE` 操作。）

代码块设置如下：

```
{ 
    "id": "10", 
    "name": "Shahid", 
    "location": "Mumbai", 
    "job": "Engineer" 
} 
```

任何命令行输入或输出均按如下方式编写：

```
SELECT *  
FROM Families f  
WHERE f.id = "100" 
```

**粗体**：表示新术语、重要单词或您在屏幕上看到的单词。例如，菜单或对话框中的单词会在文本中这样显示。例如："Click on **New Database** and choose the **Gremlin** as API."（点击 **新建数据库** 并选择 **Gremlin** 作为 API。）

警告或重要提示显示如下。提示和技巧显示如下。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：发送邮件至 `feedback@packtpub.com`，并在邮件主题中注明书名。如果您对本书的任何方面有疑问，请发送邮件至 `questions@packtpub.com`。

**勘误**：虽然我们已尽一切努力确保内容的准确性，但错误难免发生。如果您在本书中发现错误，我们将不胜感激您能向我们报告。请访问 `www.packtpub.com/submit-errata`，选择您的书籍，点击勘误提交表单链接，并填写详细信息。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激您能向我们提供该位置地址或网站名称。请通过 `copyright@packtpub.com` 联系我们，并附上相关材料的链接。

**如果您有兴趣成为作者**：如果您在某个主题上有专长，并有兴趣撰写或为书籍做贡献，请访问 `authors.packtpub.com`。

## 评论

请留下评论。读完并使用本书后，为什么不在购买网站上留下评论呢？潜在读者可以看到并利用您的公正观点做出购买决定，我们 Packt 可以了解您对我们产品的看法，我们的作者也能看到您对他们书籍的反馈。谢谢！

有关 Packt 的更多信息，请访问 `packtpub.com`。
