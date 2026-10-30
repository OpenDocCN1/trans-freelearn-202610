# Django 生产实战：编写可扩展且可维护的 Django 代码的专家技巧、策略和必备框架




To my “Maa,” Anima Saha, and to the memory of my “Baba,” for their constant support in helping me reach where I am today. To my partner, Parul, for being a constant inspiration and motivation.

– Arghya Saha

# Contributors

## About the author

**Arghya (argo) Saha**, is a software developer with 8+ years of experience and has been working with Django since 2015. Apart from Django, he is proficient in JavaScript, ReactJS, Node.js, Postgres, AWS, and several other technologies. He has worked with multiple start-ups, such as *Postman* and *HealthifyMe*, among others, to build applications at scale. He currently works at *Abnormal Security* as a senior Site Reliability Engineer to explore his passion in the infrastructure domain.

In his spare time, he writes tech blogs. He is also an adventurous person who has done multiple Himalayan treks and is an endurance athlete with multiple marathons and triathlons under his belt.

This book is dedicated to my Maa and memories of my Baba. I would like to thank my partner, Parul, and all my friends who supported me throughout this journey. I am grateful to Ganesh, Rahul, and everyone who helped me with my book.

## About the reviewers

**Abdul-Rahman Mustafa Saber Abdul-Aziz** is an Egyptian Python backend developer, senior AI instructor, capstone projects lead, and IBM consultant. He graduated from Assiut University in Upper Egypt. He believes that to be great, you must take great responsibility.

*I want to thank my family, my friends, and my future wife, *who I haven’t met yet*. I love you so much. I want to thank my father once again; everything I am now is thanks to him. This is the second time that my name has appeared on the Contributors’ list of a book. I want to thank Dr. Rania Hafez, Dr. Huda Goda, Dr. Abdul-Rahman Eliwa, and Mr. Ehab, who always pushed me toward progress, and my close friends, Muhammad Adly, Ahmed Fouad, and many others. Thank you all for everything you gave me; you are my family.*

**Ruben Atinho**, a software engineer specializing in backend engineering, explores the vast realms of technology. Proficient in Python, Django, and PostgreSQL, and experienced with Golang, he combines technical expertise with a passion for reading open source code. Beyond the code base, Ruben finds fulfillment in the harmonies of music, insightful articles, captivating books, and the imaginative narratives of anime.

**Md Enamul Hasan** is a seasoned full stack Python developer with over a decade of professional experience in software and web application development. Based in New York, United States, Enamul is renowned for his expertise in Python, Django, React, and AWS. He has successfully led the development of various software products, including ERP, e-commerce, and data-driven applications.

In addition to his technical proficiency, Enamul has excelled in leadership roles, contributing significantly to project success. He actively shares his knowledge, participating in forums and conferences and showcasing a passion for problem-solving in the ever-evolving software development landscape.

Enamul extends gratitude to the author for the opportunity to review *Django in Production*. His extensive experience in Python Django development adds depth to the review, highlighting the book’s value in bridging theory and practice in the dynamic tech industry.

# Preface

Hey there! As the name suggests, Django in Production is a book to help developers put their application code into production. In today’s world, coding has become a profession that people get into after joining a 3–6 month boot camp. With the start-up boom, most of these developers are able to land a job after their boot camp course, since they are able to write code and hack any product together. However, a couple of months into the job, they want to learn about the best practices and understand all the aspects that senior developers in the industry know and perform, but most start-ups don’t have many senior developers due to budget and talent scarcity. This book is going to give them a too long; didn’t read (TLDR) version of software development best practices, which they need to know to get to the next level.

# Who this book is for

*This book is for any software developer who understands the basic concepts of Django but now needs some help putting their code to production using the right tools, or someone who does not have enough guidance to know how to do the work the right way. We are assuming you have a basic understanding of how to write code in Django and now want to improve **your skills.*

# What this book covers

*Chapter 1*, *Setting Up Django with DRF*, covers the basic project setup of Django and **Django Rest Framework** (**DRF**). It will also help you to understand the fundamentals of APIs and how to design a REST API.

*Chapter 2*, *Exploring Django ORM, Models, and Migrations*, covers how to integrate Django with a database. Django ORM and migrations are powerful features; we learn about the core concepts and how to use them efficiently in this chapter.

*Chapter 3*, *Serializing Data with DRF*, explores the concept of serialization and how developers can use DRF serializers to write better application code.

*Chapter 4*, *Exploring Django Admin and Management Commands*, covers the core concepts of Django admin. This chapter covers all the best practices on how to use Django admin and create custom Django management commands.

*Chapter 5*, *Mastering Django Authentication and Authorization*, covers the key concepts of authentication and authorization. Django provides authentication and authorization out of the box, and we will explain in detail how developers can use the built-in features of Django and DRF for authentication.





*第 6 章*，*缓存、日志记录和节流*，涵盖了缓存的所有概念以及如何将 Redis 与 Django 集成用于缓存。日志记录是生产环境中任何 Web 应用程序的关键组件，在本章中，我们将学习如何将日志记录集成到 Django 应用程序中。

*第 7 章*，*使用分页、Django 信号和自定义中间件*，涵盖了 Django 的所有高级概念。开发人员可以使用 Django 信号编写解耦的代码。Django 还提供了创建自定义中间件的灵活性，这可以帮助开发人员改进功能。

*第 8 章*，*使用 Celery 与 Django*，展示了如何为 Web 应用程序处理异步任务。在本章中，开发人员将了解如何将 Celery 集成到 Django 项目中。

*第 9 章*，*在 Django 中编写测试*，涵盖了为 Django 编写测试用例的核心概念。在本章中，你将学习编写测试用例时应遵循的最佳实践，并了解为项目编写测试用例的重要性。

*第 10 章*，*探索 Django 中的约定*，展示了在与 Django 协作时使用的所有最佳实践和约定。本章涵盖了许多有主观性的概念，你应将本章作为大纲阅读，并根据自己的判断挑选/学习相关概念。

*第 11 章*，*Docker 化 Django 应用程序*，涵盖了如何将 Docker 与 Django 应用程序集成。

*第 12 章*，*使用 Git 和 CI 管道与 Django 协作*，涵盖了版本控制的概念以及如何在 Django 项目中高效使用 Git。在本章中，你将学习如何集成 GitHub Actions 来创建 CI 管道。

*第 13 章*，*在 AWS 中部署 Django*，涵盖了如何使用不同的 AWS 服务在生产环境中部署 Django 应用程序。在本章中，你将学习如何在生产环境中部署和扩展 Django 应用程序。

*第 14 章*，*监控 Django 应用程序*，涵盖了如何在生产环境中监控 Django 应用程序。生产环境中需要不同类型的监控，例如错误监控、应用程序性能监控、正常运行时间监控等等。在本章中，你将学习如何集成市场上可用的不同工具来监控 Django 应用程序。

# 为了充分利用本书

*你需要对 Django 有基本的了解，并且能够熟练编写基本的 Django 应用程序代码。在本书中，我们将学习 Django 的许多核心概念，你需要能够跟随这些代码示例。我们将介绍许多第三方工具/平台，可能为付费/免费，你需要在此类平台上创建账户并将它们集成到 `Django project` 中。*

| **本书涵盖的软件/硬件** || **操作系统要求** |
| --- | --- |
| Python 3.10 及以上 || Windows、macOS 或 Linux |
| Django 4.x、Django 5.0 及以上 ||  |
| Python 包如 `celery`、`django-fsm`、`factory-boy`、`freezetime`、`django-json-widget`、`rest_framework` ||  |
| Docker ||  |
| `Amazon Web Services` (`AWS`)、ElephantSQL、Neon ([`neon.tech`](https://neon.tech))、Redis ||  |
| 工具如 Rollbar、NewRelic、Better Uptime。 ||  |

**如果你使用的是本书的数字版，我们建议你自己输入代码或从本书的 GitHub 仓库访问代码（链接在下一节提供）。这样做将有助于你避免与复制和粘贴代码相关的任何潜在错误。**

# 下载示例代码文件

你可以在 GitHub 上从 [`github.com/PacktPublishing/Django-in-Production`](https://github.com/PacktPublishing/Django-in-Production) 下载本书的示例代码文件。如果代码有更新，将在 GitHub 仓库中更新。

我们还有来自我们丰富的图书和视频目录的其他代码包，可在 [`github.com/PacktPublishing/`](https://github.com/PacktPublishing/) 获取。快来看看吧！

# 使用的约定

本书通篇使用了多种文本约定。

**正文中的代码**：表示正文中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。示例：“由于我们指定了 `DemoViewVersion` 类，此视图将只允许 URL 路径中的 `v1`、`v2` 和 `v3` 版本；路径中任何其他版本将得到 `404` 响应。”

代码块如下所示：
```
urlpatterns = [
    ...
    path('apiview-class/', views.DemoAPIView.as_view())
]
```

当我们希望引起你对代码块中特定部分的注意时，相关行或项目以粗体显示：
```
urlpatterns = [
    path('hello-world/', views.hello_world),
    path('demo-version/', views.demo_version),
    path('custom-version/', views.DemoView.as_view()),
    path('another-custom-version/', views.AnotherView.as_view())
]
```

任何命令行输入或输出均如下编写：
```
celery –-app=config beat -–loglevel=INFO
```

**粗体**：表示新术语、重要词语或你在屏幕上看到的词语。例如，菜单或对话框中的词语以**粗体**显示。示例：“单击 **Create New ****Instance** 按钮。”

提示或重要说明

显示如下。

# 联系我们

我们始终欢迎读者的反馈。

**一般反馈**：如果您对本书的任何方面有疑问，请发送电子邮件至 **customercare@packtpub.com**，并在邮件主题中提及书名。

**勘误**：尽管我们已尽一切努力确保内容的准确性，但难免会出错。如果您在本書中发现错误，我们将不胜感激您向我们报告。请访问 [www.packtpub.com/support/errata](http://www.packtpub.com/support/errata) 并填写表格。

**盗版**：如果您在互联网上以任何形式发现我们的非法复制品，我们将不胜感激您提供位置地址或网站名称。请通过 **copyright@packt.com** 联系我们并附上材料链接。

**如果您有兴趣成为作者**：如果您在某个主题方面有专业知识，并且有兴趣撰写或为一本书做出贡献，请访问 [authors.packtpub.com](http://authors.packtpub.com)。

# 分享您的想法

读完 *Django in Production* 后，我们很想听听您的想法！请访问 [`packt.link/r/1804610488`](https://packt.link/r/1804610488) 分享您的反馈。

您的评论对我们和技术社区都很重要，将帮助我们确保提供优质内容。

# 下载本书的免费 PDF 副本

感谢您购买本书！

您喜欢随时阅读但无法随身携带印刷书籍吗？

您的电子书购买与您选择的设备不兼容？

不用担心，现在购买每本 Packt 书籍，您都将免费获得一本无 DRM 的 PDF 版本。

在任何地方、任何地点、在任何设备上阅读。直接从您喜爱的 technical 书籍中搜索、复制和粘贴代码到您的应用程序中。

福利还不止于此，您还可以每天通过收件箱获得独家折扣、新闻通讯和精彩免费内容

按照以下简单步骤获取福利：

1. 扫描二维码或访问以下链接 ![](img/B18867_QR_Free_PDF.jpg)
[`packt.link/free-ebook/9781804610480`](https://packt.link/free-ebook/9781804610480)
1. 提交您的购买证明





2. 搞定！我们会直接把免费的 PDF 和其他福利发送到你的邮箱
