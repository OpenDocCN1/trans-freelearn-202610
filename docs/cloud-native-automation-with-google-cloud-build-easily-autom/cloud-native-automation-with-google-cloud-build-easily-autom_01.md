# Google Cloud Build 云原生自动化：在完全托管、可扩展且安全的平台中轻松自动化任务




为我在这其中的美好，Margaux 和 Benjamin，以及你们帮助我贡献的诗句。

- Anthony

致我的妻子 Lien-ting 和孩子 Mason 与 Madison，感谢你们的支持和耐心，让这一切成为现实。

- Kent

## 贡献者

### 关于作者

**Anthony Bushong** 是 Google 的高级开发者关系工程师。曾任 Kubernetes 和 GKE 的现场工程师和实践负责人，他曾与公司合作在 Cloud Build 中为其 Kubernetes 基础设施实施自动化——从 1.3 版开始！他现在专注于将这些经验提炼成多种技术内容形式——目标都是为了教导和赋能人们，无论其背景或经验如何。

*我想感谢我的妈妈、爸爸、奶奶、家人和朋友——没有你们，我什么都不是。我还要感谢那些在这个行业中给我机会的人，认识到我所取得的许多成就都建立在你们的信任和教导之上——尤其是 Diane Anderson 和 Andrew Milo。*

**Kent Hua** 是一名专注于应用现代化的全球解决方案经理。他在帮助客户现代化企业应用方面拥有多年经验，涵盖本地和公有云上的业务和技术挑战。多年以来，他帮助组织将单体应用分解并在 Kubernetes 上运行的容器中实施微服务模式。在支持这些组织的过程中，他发现文化和流程自动化是其现代化之旅中的关键要素。

*我想感谢我的父母、家人、朋友和同事，他们造就了今天的我。通过我们的互动和经历，几乎每天都有新的学习。*

### 关于审阅者

**Marcelo Costa** 是一位技术爱好者，过去十年在软件和数据领域花费了相当多的时间。在公司工作和个人项目之间，他使用过多种技术和业务领域，始终寻找具有挑战性的问题来解决。

他是云计算 **Google Developer Expert (GDE)**，喜欢通过文章、教程和开源代码与他人分享知识来帮助社区。他目前在数据领域的爱沙尼亚初创公司 Alvin 担任创始工程师。

**Damith Karunaratne** 是 Google Cloud 的产品经理组，专注于推动持续集成和软件供应链安全工作。在加入 Google 之前，Damith 花了十多年时间帮助各种规模的公司解决复杂的软件挑战，包括领导各种 CI/CD 和 DevOps 计划。

Damith 在 McMaster University 获得理学学士学位，主修计算机科学。

## 前言

本书首先讨论托管服务的价值以及它们如何帮助组织专注于手头的业务问题。构建流水线在组织中至关重要，因为它们有助于在代码部署到生产环境之前构建、测试和验证代码。

我们随后直接进入 Cloud Build：基础知识、配置选项、计算选项、构建执行、构建触发和构建安全。

本书的剩余章节以 Cloud Build 在自动化场景中的实际示例作为结尾。虽然 Cloud Build 可以帮助软件构建生命周期，但它也可以协调向诸如无服务器和 Kubernetes 之类的运行时交付。

## 本书适合谁

本书面向管理云环境并希望在完全托管、可扩展且安全的平台中自动化工作流程的云工程师和 DevOps 工程师。假定您了解云基础知识、软件交付和容器化基础知识。

## 本书涵盖内容

第 1 章，介绍 Google Cloud Build。建立无服务器和托管服务的基础，重点关注使用 Cloud Build 的软件构建生命周期。

第 2 章，配置 Cloud Build 工作器。这是一个托管服务，但我们仍然需要计算资源，本章讨论可用的计算选项。

第 3 章，入门 – 我可以获得哪些构建信息？启动第一个构建并发现构建开始后可用的信息，以帮助您了解成功情况或调试问题。

第 4 章，构建配置和架构。您可以快速开始使用 Cloud Build，但了解配置选项可以帮助您节省时间。

第 5 章，触发构建。这是自动化的关键组件：当源文件发生某些变化时做出反应，或从现有自动化工具触发。

第 6 章，管理环境安全。这是一个托管服务，但仍然存在共享责任，确定谁可以执行流水线、流水线可以访问的内容以及如何安全地与其他服务集成。

第 7 章，使用 Terraform 和 Cloud Build 自动化部署。我们还可以利用 Cloud Build 自动化 Terraform 清单以构建基础设施。

第 8 章，使用 Cloud Build 安全地向 GKE 交付软件。发现 Google Cloud 中用于安全容器交付到 Google Kubernetes Engine 的模式和功能。

第 9 章，使用 Cloud Build 自动化无服务器。虽然是无服务器，但我们仍然需要自动化从源代码到运行中的东西的过程。

第 10 章，在生产环境中运行 Cloud Build 的操作。在为生产环境准备 Cloud Build 时，可能需要做出额外的考虑，并与多个团队协作。

第 11 章，Cloud Build 的未来展望。接下来是什么？例如，我们将了解 Cloud Deploy 如何在内部利用 Cloud Build。

## 如何充分利用本书

您需要具备软件开发、软件交付和流水线的经验，才能充分利用本书和 Cloud Build。

您需要访问一个 Google Cloud 账户。本书中的示例可以在 Google Cloud 的 Cloud Shell 中执行，其中包含本书中提到的大多数工具和二进制文件。如果需要外部资源，则会在各自的章节中注明。

**如果您使用的是本书的数字版，我们建议您自己输入代码或从本书的 GitHub 仓库访问代码（链接在下一节中提供）。这样做可以帮助您避免因复制和粘贴代码而产生的潜在错误。**

## 下载示例代码文件

本书将利用各章节中提到的不同仓库中的示例。

本书中的代码示例可以在 https://github.com/PacktPublishing/Cloud-Native-Automation-With-Google-Cloud-Build 找到。如果代码有更新，将在 GitHub 仓库中更新。

Packt Publishing 还在 https://github.com/PacktPublishing/ 上提供我们丰富的图书和视频目录中的其他代码包。快去看看吧！

## 下载彩色图片

我们还提供一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。您可以在此处下载：https://packt.link/C5G3h。

## 使用的约定

本书在整个过程中使用了多种文本约定。

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: “In this case, we will be creating a private pool of workers that have the e2-standard-2 machine type, with 100 GB of network-attached SSD, and located in us-west1.”

代码块设置如下：

```
```
  # Docker Build
  - name: 'gcr.io/cloud-builders/docker'
    args: ['build', '-t',
           'us-central1-docker.pkg.dev/${PROJECT_ID}/image-repo/myimage',
           '.']
```

当我们希望引起您对代码块中特定部分的注意时，相关行或项目会以粗体显示：

```
```
...
INFO[0002] No cached layer found for cmd RUN npm install
INFO[0002] Unpacking rootfs as cmd COPY package*.json ./
requires it.
...
INFO[0019] Taking snapshot of files...
```

任何命令行输入或输出均按如下方式编写：

```
```
$ project_id=$(gcloud config get-value project)
$ vpc_name=packt-cloudbuild-sandbox-vpc
```

粗体：表示新术语、重要单词或屏幕上显示的单词。例如，菜单或对话框中的单词以粗体显示。以下是一个示例：“Select System info from the Administration panel.”

提示或重要说明
如下所示显示。

## 联系我们

我们始终欢迎读者的反馈。

**一般反馈：** 如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误：** 尽管我们已尽一切努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**盗版：** 如果您在互联网上发现我们作品的任何形式的非法复制品，我们将不胜感激您提供位置地址或网站名称。请通过 `copyright@packt.com` 联系我们，并附上材料的链接。

**如果您有兴趣成为作者：** 如果您在某个主题方面有专业知识，并且有兴趣撰写或为一本书做出贡献，请访问 `authors.packtpub.com`。

## 分享您的想法

在您阅读了 *使用 Google Cloud Build 进行云原生自动化* 后，我们很乐意听取您的想法！请点击此处直接前往本书在 Amazon 的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，并将帮助我们确保提供优质的内容。


