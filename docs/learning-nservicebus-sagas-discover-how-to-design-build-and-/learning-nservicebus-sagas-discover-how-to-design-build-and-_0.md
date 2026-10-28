# NServiceBus Sagas 学习指南：探索如何设计、构建和测试 NServiceBus Sagas 和消息传递


## Credits

- **Author:** Rich Helton
- **Project Coordinator:** Mary Alex
- **Reviewers:**
  - Neil Bourgeois
  - Prashant Brall
  - Mark Huber
- **Proofreaders:**
  - Samuel Redman Birch
  - Maria Gould
  - Bernadette Watkins
- **Commissioning Editor:** Usha Iyer
- **Indexer:** Monica Ajmera Mehta
- **Acquisition Editor:** Kevin Colaco
- **Graphics:** Valentina D'silva
- **Content Development Editors:**
  - Akshay Nair
  - Priya Singh
- **Production Coordinator:** Nilesh R. Mohite
- **Technical Editor:** Edwin Moses
- **Cover Work:**
  - Arvindkumar Gupta
  - Nilesh R. Mohite
- **Copy Editors:**
  - Sarang Chari
  - Puja Lalwani
  - Veena Mukundan

## About the Reviewers

**Neil Bourgeois** is a software engineer at Pentair Technical Solutions, where he leads the Engineering Software team. Solutions he has architected and implemented include a high-volume metering and billing system for the utilities industry and an industry-leading 3D engineering tool for the industrial heat-tracing field. He applies the discipline of Agile software development to his work and believes that great software comes from great team cultures. He strives to lead his teams to great cultures.

**Prashant Brall** is a principal consultant and senior software architect/developer who uses Microsoft technologies. He works for Veritec Pty Ltd (www.veritec.com.au) in Canberra, Australia, and has been developing software for the past 18 years. He enjoys writing about his experiences on his blog at https://prashantbrall.wordpress.com.

Prashant has also reviewed the book *Instant AutoMapper*, Packt Publishing.

In his leisure time, he enjoys watching movies with his wife and playing musical instruments.

> I would like to thank my fantastic wife, Jhumur, for being my best friend and for all the love and support she has given me. I would also like to thank my parents Mr. Hem Brall and Mrs. Prabha Brall for all the guidance, love, and care they have given me.

- Workflow retries: Messages, or endpoints, can be told to retry a number of times until they completely fail and send an error. The error is automatically returned to an error queue. For instance, a web service message can be sent to a bank, and it can be set to retry the web service every 5 minutes for 20 minutes before giving up completely. This is useful during any network or server issues.
- Monitoring: NSB ServicePulse can keep a finger on the pulse of its services. Other monitoring can easily be done on the NSB queues to report on the number of messages.
- Encryption: Messages between services and endpoints can be easily encrypted.
- High availability: Multiple services or subscribers could be processing the same or similar messages from various services that are living on different servers. When one server, or service, goes down, others could be made available to take over those that are already running.

Sagas are at the heart of the NServiceBus (NSB) workflow. Sagas save the message state in the form of saga data. They can retrieve the data as it is related to a message to update the message or data. This allows the NSB workflow to control the flow of data and messaging from end to end and correlate saved data to messages in the saga.

Messages are the means to transfer interaction and data between services in a service-oriented architecture (SOA). Sagas correlate, save, route, and manage processes that are started by these messages between services. Sagas even provide timeouts to ensure that messages do not live forever in a system.

Sagas provide decoupling between frontend websites and backend processes. It allows workflow to transfer so that a website can continue to do its work without having to wait for processes to return, such as the dreaded message "Please do not refresh this website as we bill your credit card".

NSB doesn't stop at development on Windows servers and desktops, but plays a big part in cloud development, for example with Microsoft Azure and the Microsoft cloud. NSB uses Service Bus as well as storage queues, SQL Server, and other storage containers. As the cloud is used more and more, NSB plays a key part in Software as a Service, as it is the premier framework for ESB in the C# clouds.

Even if your cloud solution doesn't end up being a C#-compatible cloud, as many might hide the software running behind the cloud as being proprietary, NSB is a component for those that will do hybrid solutions, such as keeping data resident on-site. The connection to the cloud then will likely be via web services, and NSB sagas might likely provide the workflow to those web services.

NServiceBus is the C# platform of choice for those that require workflows, sagas, high availability, high performance, monitoring, encryption, rapid deployment, and many more features that can only be found in this framework when building C# solutions.

## What this book covers

Chapter 1, Introduction to Sagas, discusses NServiceBus and a basic design pattern that it uses, known as sagas, which is used to save states of messages. We will discuss the benefits of sagas and what it brings to the table with regard to software design.

Chapter 2, NServiceBus Saga Architecture, expands on the uses of sagas for persistence, timeouts, message durability, and message handling. We will discuss various message exchange patterns through examples to include gateway and cluster managing. These are important concepts as they drive the high availability and high performance that NSB brings to the table.

Chapter 3, The Particular Service Platform, is an overview of the Particular website-associated tools for NServiceBus. We will discuss building sagas through the ServiceMatrix tool, which is a Visual Studio extension tool for visually designing NSB endpoints, messages, and services. The other tools that we will discuss at length as they apply to sagas are the ServicePulse to monitor the endpoint availability during production, and ServiceInsight to take a deep dive into the functionality and properties of endpoints, services, and messages as they execute.

Chapter 4, Saga Development, focuses on various useful constructions of sagas and message handlers. The purpose of sagas will be discussed as the discussion goes into the need for extending and coordinating transactional integrity using sagas. The chapter then morphs into a discussion of NServiceBus, using integrated pre-built WCF bridges. While some might consider it unusual to discuss WCF in a saga chapter, sagas become an intermediate for coordinating WCF and NServiceBus work. We can decouple the workflow from the frontend for interaction with backend processes through message handling. Sagas provide the means to persist the state information of the messages. This discussion will also handle other queuing sources as well to include RabbitMQ and ActiveMQ.





## 本书读者对象

本书面向希望学习如何使用 NServiceBus 开发，并探索 NSB  Saga（即 NSB 的工作流核心）的初级或中级 C# 开发者。本书将讨论许多入门知识，并深入探讨 Saga，同时使用多种 Microsoft 框架，这也会为你提供使用 WPF、WCF、ASP.NET MVC 和 Entity Framework 的一些基础知识。

## 约定

在本书中，你会发现多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义说明。

文本中的代码字、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 用户名显示如下：“`RequestTimeout()` 函数可能需要几秒、几分钟、几小时、几天，也可以在一天中的特定时间执行。”

代码块如下所示：

```
using System;
using System.IO;
using ServiceControl.Plugin.CustomChecks;
using ServiceControl.Plugin.CustomChecks.Messages;
using ServiceControl.Plugin.CustomChecks.Internal;
namespace PaymentEngine.ECommerce
{
public class MyCustomCheck : CustomCheck
{
public MyCustomCheck()
: base("ECommerce SubmitPayment check", "ECommerce")
{
ReportPass();
}
}}
```

任何命令行输入或输出均如下书写：

```
PM> Get-NserviceBusLocalMachineSettings
```

## 勘误

尽管我们已竭尽全力确保内容的准确性，但错误仍在所难免。如果你在我们的某本书中发现了错误——无论是文字错误还是代码错误——我们将不胜感激，请你向我们报告。这样做可以帮助其他读者免受困扰，并帮助我们改进本书的后续版本。如果你发现任何勘误，请访问 http://www.packtpub.com/submit-errata，选择你的图书，点击“Errata Submission Form”链接，并输入勘误详情。一旦你的勘误得到验证，你的提交将被接受，勘误信息将上传到我们的网站，或添加到该图书勘误部分下的现有勘误列表中。

要查看之前提交的勘误，请访问 https://www.packtpub.com/books/content/support，并在搜索字段中输入书名。所需信息将显示在勘误部分下。

## 盗版

互联网上对受版权保护材料的盗版是所有媒体持续存在的问题。在 Packt，我们非常重视对我们版权和许可的保护。如果你在互联网上以任何形式发现我们作品的任何非法副本，请立即向我们提供位置地址或网站名称，以便我们采取补救措施。

请通过 copyright@packtpub.com 联系我们，并附上涉嫌盗版材料的链接。

我们感谢你帮助保护我们的作者和我们为您提供有价值内容的能力。

## 问题

如果你对本书的任何方面有问题，可以通过 questions@packtpub.com 联系我们，我们将尽最大努力解决问题。
