

## 情感分析

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_291_0.png)

图 11.9：正负值的绘图

在本节完成了一些数据和解析后，可以说语录中找到的大多数记录本质上是中性的。

> **重要说明**
> 
> 使用 `nltk` 进行情感分析比 `textblob` 和 `vaderSentiment` 涉及更多步骤。此外，它目前拥有最丰富的方法和属性集合。请访问 `https://www.nltk.org/` 了解更多详情。

在本章中，我们学习了机器学习的一些基础知识，以及当提供从网络抓取的高质量数据时，机器学习如何通过其各种技术在各种情境中提供帮助并发挥重要作用。

## 小结

Python 编程在人工智能和机器学习相关领域做出了巨大贡献。在本章中，我们仅仅看到了它的冰山一角。高质量数据在机器学习中扮演着非常重要的角色。无论是通过网络抓取收集数据并存储，还是将抓取到的数据即时提供给机器学习模型，经过处理的数据都是必需的。我们提供给机器学习算法用于绘图的数据质量越高、越精确，能期待的准确结果、可视化和描述性图表就越多。

现在，我们已经通过探索学习了机器学习的概念及其各个方面。我们还学习了如何实现机器学习模型，并在需要时从各种过程中收集结果。总而言之，我们现在已经了解了如何使用 scikit-learn 以及如何进行情感分析。机器学习是数据驱动的，高质量数据是机器学习模型提供准确性的基本要求。

在下一章中，我们将学习一些对网络抓取数据有益且适用的进一步步骤和主题，这些内容可以用于网络抓取过程。

## 延伸阅读

- **机器学习：**
  - https://www.ibm.com/topics/machine-learning
  - https://www.mygreatlearning.com/blog/what-is-machine-learning/
  - https://www.run.ai/guides/machine-learning-engineering/machine-learning-automation
- **机器学习——Python：**
  - https://scikit-learn.org/
  - https://www.w3schools.com/python/python_ml_getting_started.asp
  - https://realpython.com/tutorials/machine-learning/
  - https://machinelearningmastery.com/machine-learning-in-python-step-by-step/
- **回归分析：**
  - https://www.alchemer.com/resources/blog/regression-analysis/
  - https://www.investopedia.com/terms/r/regression.asp
- **自然语言处理：**
  - https://www.nltk.org/
  - https://www.ibm.com/topics/natural-language-processing
  - https://hbr.org/2022/04/the-power-of-natural-language-processing
- **情感分析：**
  - https://monkeylearn.com/sentiment-analysis/
  - https://huggingface.co/blog/sentiment-analysis-python
  - https://aws.amazon.com/what-is/sentiment-analysis/

## 第5部分：结论

在本部分中，你将了解一些精选的高级概念，这些概念可以结合抓取的数据或数据集实现，或在其之上实现，以完成高级处理和功能。你还将了解与网络抓取和数据科学相关的、在全球范围内需求很高的工作和职业发展方向。

本部分包含以下章节：

- *第12章，抓取之后——后续步骤与数据分析*

## 12

## 抓取之后——后续步骤与数据分析

到目前为止，我们已经学习了如何抓取网页、分析收集的数据，并使用 Python 编程应用**机器学习（ML）**算法。

本章将简要介绍一些在该领域日益重要的新兴概念和技术。从数据中获取洞察和知识的能力需求旺盛，甚至相关供给领域也在呈指数级增长。从研究领域到商业领域，数据的重要性迅速提升。由机器学习逻辑驱动的**人工智能（AI）**系统将继续消耗并生成更多数据。

在本章中，我们将学习以下主题：

- 抓取之后会发生什么？
- 网络请求
- 数据处理
- 工作与职业

## 技术要求

需要使用网络浏览器（*Google Chrome* 或 *Mozilla Firefox*）来浏览所提供的资源。

## 抓取之后会发生什么？

网络抓取是一种收集和提供高质量数据的方法，并应用各种依赖数据的技术。任何涉及数据的学科或领域都提供了学习和成长的机会，因为通过抓取获得的数据被用于分析、报告、数据集创建和数据挖掘等许多其他任务。随着基于网络的技术的发展，与网络抓取相关的技术一直是动态的且充满挑战。

基于数据的系统以图像和视频的形式返回原始数据、处理后的数据和可视化图表，正通过多种形式吸引全球受众而不断发展。**信息技术（IT）**驱动的系统是所有行业都在使用的核心应用，Python 编程在数据相关系统的开发和处理中发挥了重要作用。

本书前面的章节介绍了几个步骤，这些步骤整合了如下一些主要概念和操作：

- 数据需求
- 数据收集
- 数据分析（信息生成）
- 数据使用（ML/AI）

大量框架和编程语言正在被开发。与数据、信息和知识管理相关的应用在商业和研究领域中使用最为广泛。

虽然我们已经讨论了网络抓取和数据收集的各种特性和技术，但鉴于各种实体不断被创建和演变，能够调整我们的方法和技术非常重要。一些因素，例如系统的有效性、时间消耗、内容量以及适用技术的可用性，会影响整个网络抓取过程，甚至影响数据收集后执行的步骤。

在下一节中，我们将介绍一些处理与网络相关请求的高级技术和 Python 库。

## 网络请求

在本书的各个章节中，一直使用 `requests` Python 库在代码和网络之间建立通信。如果在 https://pypi.org/ 上搜索与 `requests` 类似的库，可以找到大量 Python 库。

以下小节列出了一些 Python 库和技术，并对其进行了简要介绍。

## pycurl

pycurl Python 库（http://pycurl.io/）是基于流行的 libcurl 库的封装。libcurl 是最早用于与互联网网站通信的 Python 库之一，它基于 **curl** 工具（也称为 **cURL**）。

`curl (https://curl.se/)` 是一个用于在网络上连接和传输数据的命令行工具。`curl` 是网络通信的基础；它是一个核心实现，借助封装器，可用于不同的**操作系统（OS）**、浏览器以及与互联网通信的机器。curl 命令与机器无关。Python、PHP、C、R、Java 和 Rust 等编程语言会将原始 curl 命令转换为其首选格式，以便在其代码或库中使用。

pycurl 使用 C 编程语言开发，为 Python 代码提供基于 curl 的功能。与其他库相比，requests 库通常被认为易于使用。pycurl 提供了与 curl 几乎相同的特性和功能，包括其属性和方法。

以下是 pycurl 相对于 requests 的一些主要优势：

- 支持大量通信协议（HTTPS、POP3、FTP、LDAP、IMAP 等）
- 与 requests 和其他类似 Python 库相比更快

现代浏览器甚至提供原始 curl 命令，可以使用基于浏览器的开发者工具找到，如前面第 2 章所述。这些 curl 命令可以被复制并用于以下场景：

- **命令行界面（CLI）**应用程序（Bash shell 或脚本）或终端
- 基于 Web 的终端和 API 工具，例如 **Postman**（https://www.postman.com/）
- 带有请求头的 Python 库，例如 requests（https://curlconverter.com/python/）
- 将命令转换为其他编程语言

图 12.1 展示了使用“Copy as cURL (bash)”选项从基于浏览器的开发者工具中复制得到的用于加载 https://curl.se/ 的 curl 命令。

```
curl 'https://curl.se/' \
  -H 'authority: curl.se' \
  -H 'accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7' \
  -H 'accept-language: en-US,en;q=0.9' \
  -H 'cache-control: max-age=0' \
  -H 'if-modified-since: Thu, 01 Jun 2023 10:05:02 GMT' \
  -H 'if-none-match: "2153-5fd0e9e9a8fc-gzip"' \
  -H 'sec-ch-ua: "Not.A/Brand";v="8", "Chromium";v="114", "Google Chrome";v="114"' \
  -H 'sec-ch-ua-mobile: ?0' \
  -H 'sec-ch-ua-platform: "Windows"' \
  -H 'sec-fetch-dest: document' \
  -H 'sec-fetch-mode: navigate' \
  -H 'sec-fetch-site: none' \
  -H 'sec-fetch-user: ?1' \
  -H 'upgrade-insecure-requests: 1' \
  -H 'user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36' \
  --compressed
```

图 12.1：curl 命令

