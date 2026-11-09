

![](img/9aa003afabe05fdb07da46ad71325de6_141_0.png)

Scraper 根据一个示例用户名查找所有相似项目。

- 4. 在窗口底部，有一个标记为 **Export to Google Docs...** 的按钮。请注意，根据您的设置，您可能需要点击同意以允许 Scraper 访问 Google Docs。

## 第三步 – 对数据列进行最终清理

一旦我们从页面中提取出所有数据元素并存放在单独的 Google Docs 中，我们就需要将它们合并到一个文件中并进行最终清理。以下是行号在提取后但尚未清理时的示例：

|  | A | B |
|---|---|---|
| 1 | 链接 | 网址 |
| 2 | # | #1574618 |
| 3 | # | #1574619 |
| 4 | # | #1574620 |
| 5 | # | #1574621 |
| 6 | # | #1574622 |

我们对列 A 完全不感兴趣，也不关心前导的 # 符号。用户名和行消息数据类似——我们想要大部分内容，但希望删除一些符号并将所有内容合并到单个 Google 电子表格中。

使用我们在第 3 章《干净数据的 workhorses – 电子表格和文本编辑器》中介绍的查找和替换技术（即删除 #、< 和 > 符号并将行粘贴到单个工作表中），我们最终得到一个干净的数据集，如下所示：

|  | A | B | C |
|---|---|---|---|
| 1 | 1574618 | petisnake | 我不知道这一点 |
| 2 | 1574619 | dshap | FunkyBob：啊，嗯，我想知道是否可以在我的 QuerySet 中做到这一点 |
| 3 | 1574620 | petisnake | 从 1.5 版以来没有使用过 Django |
| 4 | 1574621 | FunkyBob | petisnake：真的吗？它可能是 1.7 中最大的单一新功能 |
| 5 | 1574622 | FunkyBob | renlo：顺便说一下——你可以做一个更聪明的查询 |

Scraper 是一个从网页中提取少量数据的不错工具。它具有方便的 Google 电子表格界面，如果你不想编写程序来完成这项工作，它可以是一个快速解决方案。在下一节中，我们将处理一个更大的项目。它可能复杂到足以让我们必须将本章中的一些概念实现到一个解决方案中。

## 示例项目 – 从电子邮件和网络论坛中提取数据

Django IRC 日志项目非常简单。它旨在向你展示三种常用的、可靠的技术之间的差异，这些技术常用于从 HTML 页面中提取干净数据。我们提取的数据包括行号、用户名和 IRC 聊天消息，所有这些都很容易找到，并且几乎不需要额外的清理。在这个新的示例项目中，我们将考虑一个概念上相似的案例，但它将要求我们将数据提取的想法从 HTML 扩展到网络上发现的另外两种半结构化文本：托管在网络上的电子邮件消息和基于网络的讨论论坛。

## 项目背景

我最近正在进行一项关于如何使用社交媒体提供软件技术支持的研究。具体来说，我试图发现某些制作 API 和框架的软件开发组织是否应将对开发者的技术支持转移到 Stack Overflow，还是应继续使用较旧的媒体，例如电子邮件和网络论坛。为了完成这项研究，我比较了（其中之一）开发者通过 Stack Overflow 获得 API 问题答案所需的时间，与在较旧的社交媒体如网络论坛和电子邮件组上所需的时间。

在这个项目中，我们将处理这个问题的其中一小部分。我们将下载代表旧社交媒体的两种原始数据：来自网络论坛的 HTML 文件和来自 Google Groups 的电子邮件消息。我们将编写 Python 代码来提取发送到这两个支持论坛的消息的日期和时间。然后我们将找出哪些消息是对其他消息的回复，并计算每个消息获得回复所需时间的一些基本汇总统计数据。

![](img/9aa003afabe05fdb07da46ad71325de6_143_0.png)

> 如果你想知道为什么我们在这个示例项目中没有为该问题的 Stack Overflow 部分提取数据，只要等到第 9 章，Stack Overflow 项目。那一整章都致力于创建和清理 Stack Overflow 数据库。

该项目将分为两部分。在第一部分中，我们将从托管在 Google Groups 上的项目中提取电子邮件档案数据，在第二部分中，我们将从另一个项目的 HTML 文件中提取数据。

## 第一部分 – 清理 Google Groups 电子邮件数据

许多软件公司传统上使用电子邮件邮件列表或混合电子邮件-网络论坛为其产品提供技术支持。Google Groups 是此服务的热门选择。用户可以向该组发送电子邮件，也可以从网络浏览器中阅读和搜索消息。然而，一些公司已放弃通过 Google Groups 向开发者提供技术支持（包括 Google 自身的产品），而改用 Stack Overflow。数据库产品 Google BigQuery 就是这样一个现在使用 Stack Overflow 的组。

## 第一步 – 收集 Google Groups 消息

为了研究 BigQuery Google Group 上问题的响应时间，我首先创建了该组中所有帖子的 URL 列表。你可以在我的 GitHub 网站上找到我的完整 URL 列表：https://github.com/megansquire/stackpaper2015/blob/master/BigQueryGGurls.txt。

一旦我们有了目标 URL 列表，我们就可以编写一个 Python 程序来下载这些 URL 中驻留的所有电子邮件，并将它们保存到磁盘。在下面的程序中，我的 URL 列表已保存为名为 GGurls.txt 的文件。包含了 time 库，以便我们在向 Google Groups 服务器发出请求之间进行短暂的 sleep() 方法：

```python
import urllib2
import time

with open('GGurls.txt', 'r') as f:
    urls = []
    for url in f:
        urls.append(url.strip())

currentFileNum = 1
for url in urls:
    print("Downloading: {0} Number: {1}".format(url, currentFileNum))
    time.sleep(2)
    htmlFile = urllib2.urlopen(url)
    urlFile = open("msg%d.txt" %currentFileNum,'wb')
    urlFile.write(htmlFile.read())
    urlFile.close()
    currentFileNum = currentFileNum +1
```

该程序导致写入磁盘 667 个文件。

## 第二步 – 从 Google Groups 消息中提取数据

我们现在有 667 条分别存放在不同文件中的电子邮件消息。我们的任务是编写一个程序逐一读取它们，并使用本章中的一种技术来提取我们需要的信息。如果我们查看其中一条电子邮件消息，我们会看到很多 **headers**，它们存储有关电子邮件或其 **metadata** 的信息。我们可以快速看到标识我们需要的元数据元素的三个头：

```
In-Reply-To: <ab71b72a-ef9b-4484-b0cc-a72ecb2a3b85@r9g2000yqd.googlegroups.com>
Date: Mon, 30 Apr 2012 10:33:18 -0700
Message-ID: <CA+qSDkQ4JB+Cn7HNjmtLOqqkbJnyBu=Z1Ocs5-dTe5cN9UEPyA@mail.gmail.com>
```

所有消息都有 Message-ID 和 Date，但 In-Reply-To 头只会出现在是对另一条消息的回复的消息中。In-Reply-To 值必须是另一条消息的 Message-ID 值。

下面的代码展示了基于正则表达式的解决方案，用于提取 Date、Message-ID 和 In-Reply-To（如果可用）值，并创建原始消息和回复消息的一些列表。然后，代码尝试计算一条消息与其回复之间的时间差：

```python
import os
import re
import email.utils
import time
import datetime
import numpy

originals = {}
replies = {}
timelist = []

for filename in os.listdir(os.getcwd()):
    if filename.endswith(".txt"):
        f=open(filename, 'r')
        i=''
        m=''
        d=''
        for line in f:
            irt = re.search('(In\-Reply\-To: <)(.+?)@', line)
            mid = re.search('(Message\-ID: <)(.+?)@', line)
```
