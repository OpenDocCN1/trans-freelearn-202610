
**提示或重要说明**
如下所示。

## 联系我们

读者的反馈总是受欢迎的。

**一般反馈：** 如果您对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**勘误：** 虽然我们已尽一切努力确保内容的准确性，但错误难免。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**盗版：** 如果您在互联网上发现我们作品的任何形式的非法复制，我们将不胜感激您提供位置地址或网站名称。请通过 `copyright@packt.com` 联系我们，并提供相关材料的链接。

**如果您有兴趣成为作者：** 如果您在某个主题上具有专业知识，并且有兴趣撰写或为一本书做出贡献，请访问 `authors.packtpub.com`。

## 分享您的想法

阅读完 *Natural Language Processing with Flair* 后，我们很想听听您的想法！请点击此处直接前往本书在亚马逊的评论页面并分享您的反馈。

您的评论对我们和技术社区都很重要，有助于确保我们提供优质内容。

## 第一部分：
理解
和解决 NLP
使用 Flair

在本部分，您将学习 NLP 的基础知识并了解 Flair 框架的概览。您将设置环境、安装 Flair 并探索其基本功能。您将学习如何从嵌入中提取知识并在 Flair 中使用预训练的序列标注模型。

本部分包含以下章节：

- 第 1 章，*Flair 简介*
- 第 2 章，*Flair 基础类型*
- 第 3 章，*Flair 中的嵌入*
- 第 4 章，*序列标注*

## 1
Flair 简介

There are few **Natural Language Processing** (**NLP**) frameworks out there as easy to learn and as easy to work with as **Flair**。Packed with pre-trained models, excellent documentation, and readable syntax, it provides a gentle learning curve for NLP researchers who are not necessarily skilled in coding; software engineers with poor theoretical foundations; students and graduates; as well as individuals with no prior knowledge simply interested in the topic. But before diving straight into coding, some background about the motivation behind Flair, the basic NLP concepts, and the different approaches to how you can set up your local environment may help you on your journey toward becoming a Flair NLP expert.

在 Flair 的官方 GitHub README 中，该框架被描述为：

> "用于最先进自然语言处理的非常简单的框架"

此描述会引起一些注意。NLP 研究人员会立即有兴趣了解该框架在哪些具体任务中实现了最先进的结果。工程师会被 *非常简单* 的标签所吸引，但会想知道需要哪些步骤才能开始使用以及可以在哪些环境中使用。那些不了解 NLP 的人会想知道他们是否能够掌握理解 Flair 试图解决的问题所需的知识。

在本章中，我们将通过以下部分来回答所有这些问题，涵盖基本 NLP 概念和术语，提供 Flair 的概览，并设置我们的开发环境：

- NLP 简介
- 什么是 Flair？
- 准备工作

## 技术要求

开始之前，您需要一个配备 Python 3.6+ 的开发环境。有关安装 Python 的特定于平台的说明可在 https://docs.python-guide.org/starting/installation/ 找到。

您不需要配备 GPU 的开发机器，但拥有一个将显著加快本书后面描述的一些训练相关练习的速度。

您需要访问命令行。在 Linux 和 macOS 上，只需启动终端应用程序。在 Windows 上，按 Windows + R 打开运行框，输入 cmd 然后单击确定。

Flair 的官方 GitHub 仓库可通过以下链接访问：https://github.com/flairNLP/flair。本章中我们将安装 Flair 0.11 版本。

本章涵盖的代码示例可在本书的官方 GitHub 仓库中的以下 Jupyter 笔记本中找到：https://github.com/PacktPublishing/Natural-Language-Processing-with-Flair/tree/main/Chapter01。

## NLP 简介

在深入了解 Flair 的功能及其如何利用其功能之前，我们将进行 NLP 的简要介绍，为不熟悉 Flair 解决的所有 NLP 技术和任务的读者提供一些背景信息。NLP 是人工智能、语言学和软件工程的一个分支，有助于机器理解人类语言。当我们人类阅读一个句子时，我们的大脑会立即理解许多看似微不足道的问题，例如以下内容：

- 该句子是否使用我能理解的语言书写？
- 如何将句子拆分为单词？
- 单词之间的关系是什么？
- 各单词的含义是什么？
- 这是一个问题还是一个答案？
- 单词被分配到哪些词性类别？
- 句子的抽象含义是什么？

人类大脑擅长共同且通常无缝地解决这些问题，使我们意识不到仅仅通过阅读句子就理解了所有这些内容。

即使现在，机器在同时解决所有这些问题方面仍然不如人类。因此，为了教会机器理解人类语言，我们必须将自然语言的理解拆分为一组更小、机器可理解的任务，以便逐一回答这些问题。

在本节中，您将找到一些重要 NLP 任务的列表，重点介绍 Flair 支持的任务。

## 分词

**分词**是将句子或文档分解为称为 **词元** 的有意义单元的过程。词元可以是段落、句子、搭配或仅仅是一个单词。

例如，词分词器会将句子 *Learning to use Flair* 拆分为词元列表 `["Learning", "to", "use", "Flair"]`。

分词必须遵守特定语言的规则，并且很少是微不足道的任务。例如，对于无空格语言，词边界不以空格定义，很难确定一个词何时结束下一个词何时开始。明确定义的词元边界是大多数旨在处理单词、搭配或句子的 NLP 任务的先决条件，包括本章中解释的以下任务。

## 文本向量化

**文本向量化**是将单词、句子或文档从其书面形式转换为机器可理解的数值表示的过程。

文本向量化最简单的形式之一是 **独热编码**。它将单词映射到长度等于词典中单词数量的二进制向量。向量中的所有元素均为 0，除了表示该单词的元素设置为 1——因此得名 *one-hot*。

例如，取以下词典：

- Cat
- Dog
- Goat

单词 **cat** 将是我们词典中的第一个单词，其独热编码为 `[1, 0, 0]`。单词 **dog** 将是我们词典中的第二个单词，其独热编码为 `[0, 1, 0]`。单词 **goat** 将是我们词典中的第三个也是最后一个单词，其独热编码为 `[0, 0, 1]`。

然而，这种方法存在高维度的問題，因为向量的长度随词典中单词数量线性增长。它也不捕获单词的任何语义含义。为了应对这一问题，大多数现代最先进的方法使用称为词或文档嵌入的表示。每个嵌入通常是一个由实数组成的固定长度向量。虽然这些数字乍一看对人类来说难以理解，但在某些情况下，一些向量维度可能代表单词的某些抽象属性——例如，词嵌入向量的一个维度可以表示单词的总体（积极或消极）情感。给定两个或多个嵌入，我们可以使用称为 **余弦相似度** 的距离度量来计算它们之间的相似性或距离。在包括 **Flair** 在内的许多现代 NLP 解决方案中，嵌入被用作更高级别 NLP 任务（如命名实体识别）的底层输入表示。

早期词嵌入方法的主要问题之一是多义词（多义词）仅限于单一且恒定的嵌入表示。**Flair** 中解决此问题的方法之一是使用上下文字符串嵌入，其中单词由其周围文本进行上下文化，这意味着在不同的周围文本下它们将具有不同的表示。

## 命名实体识别

**命名实体识别**（NER）是一种 NLP 任务或技术，用于在文本中识别命名实体并为其标记相应的类别。命名实体类别包括但不限于 *地点*、*人名*、*品牌*、*时间表达* 和 *货币值*。
