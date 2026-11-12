# Spring Roo 1.1 秘籍：60多个配方助你使用Spring Roo开发工具加速Java Web应用开发




![](img/b8c379a2ed921ead3aa2b01811dc29fa_0_0.png)

常见问题快速解答

## 致谢

**作者**
阿希什·萨林

**项目协调员**
乔尔·戈维亚

**评审员**
托比亚斯·吕蒂克
约翰·约瑟夫·瑞安 III

**校对员**
阿伦·纳什
丽莎·布拉迪
琳达·莫里斯

**获购编辑**
乌沙·艾耶

**编索引员**
赫曼吉妮·巴里

**开发编辑**
妮哈·马利克
雷什玛·桑达雷桑

**制作协调员**
尼莱什·R. 莫海特
阿尔温·罗伊

**技术编辑**
万吉特·D'souza
梅林·谢赫

**封面设计**
尼莱什·R. 莫海特

## 关于作者

阿希什·萨林拥有 11 多年的经验，使用 Java EE 技术架构和开发应用程序。他是 Java EE 平台的 Sun 认证企业架构师。他撰写了许多关于使用 Liferay、DWR、JSF 和 Spring Portlet MVC 的门户和富 Internet 应用程序的文章。

阿希什也是 *门户组件行动* (`http://manning.com/sarin/`) 由 *曼宁出版公司* 出版。

编写 Spring Roo 1.1 Cookbook 是一次非常满意的经历，因为它涵盖了众多技术。

我要感谢托比亚斯·吕蒂克在本书审阅期间提供出色的反馈。这对改进本书的技术内容和呈现方式大有帮助。

本书在很大程度上要归功于约翰·约瑟夫·瑞安 III，他出色地确保每一个配方都经过良好测试并很好地呈现给读者。随着本书内容从 Spring Roo 1.1.1 移至 1.1.3，约翰确保我们没有遗漏任何相关的配方和技术细节。这让我有很大信心，本书涵盖的配方将按描述工作。

特别感谢万吉特·D'souza 和梅林·谢赫，他们出色地确保本书符合 Packt 的标准。

感谢妮哈·马利克在改善本书结构和呈现方式方面所做的出色工作。还要感谢乌沙·艾耶给我为 Packt 撰写的机会。

## 关于评审员

**托比亚斯·吕蒂克**在软件领域拥有 11 多年的经验。作为顾问和软件架构师，他为德国蓝筹公司及其当前雇主塑造了各种关键任务应用程序。他的背景还包括项目管理和软件开发最佳实践的教学。职业生涯早期，托比亚斯对开源和敏捷开发方法论产生了热情，这至今仍在驱动他的工作。他很幸运参与了两个基于 Roo 的重大项目的成功交付；其中一个是几乎完全由开源组件构建的面向公众的地理信息系统。

托比亚斯是认证的 Scrum Master、项目管理专业人士 (PMP)，并持有德国卡尔斯鲁厄理工学院（卓越大学）的计算机科学学位。

目前，他作为新西兰一家政府实体的高级应用解决方案架构师工作，在那里他架构企业应用并领导开发团队将他的解决方案付诸实施。

托比亚斯喜欢写作，并通过他在各种杂志上发表的文章以及他关于 OpenSSH 的书分享他在软件开发领域的经验。

**约翰·约瑟夫·瑞安 III** 是 Principigram™ LLC 的创始人兼系统工程总监。他专精于基于 Java 技术的门户 Web 开发和系统集成。他在广泛的技术栈和实现语言的数据中心系统方面拥有丰富经验。约翰拥有得克萨斯大学阿灵顿分校的计算机科学与工程学士学位，以及南卫理公会大学的系统工程硕士学位。

约翰说：“不要根据一个人能背诵什么来衡量其技能。要衡量他们掌握新技能或定义新问题的能力。生活和商业在于解决新问题，而不是背诵技术诗句。”

除了对本出版物进行技术审阅外，约翰还参与了多本基于 Spring 和 Portal 的图书评审，并认为自己只在一个领域是专家，能够在任何领域迅速发挥作用。

> 我要感谢我的妻子南西的支持。我也要感谢我的爸爸乔教会我努力工作的价值，感谢我的妈妈安向我展示积极态度的力量。最后，我要感谢 Yahshua 赋予我生命和世间的一切美好。

## www.PacktPub.com

## 支持文件、电子书、折扣优惠等

你可以访问 www.PacktPub.com 获取与本书相关的支持文件和下载。

你知道吗？Packt 为每本出版的图书提供电子书版本，可供下载 PDF 和 ePub 文件。你可以在 www.PacktPub.com 升级为电子书版本，作为印刷书客户，你有权获得电子书副本的折扣。如需了解更多详情，请通过 service@packtpub.com 与我们联系。

在 www.PacktPub.com，你还可以阅读一系列免费技术文章，订阅各种免费新闻通讯，并获取 Packt 图书和电子书的独家折扣和优惠。

![](img/b8c379a2ed921ead3aa2b01811dc29fa_7_0.png)

http://PacktLib.PacktPub.com

你需要针对 IT 问题的即时解决方案吗？PacktLib 是 Packt 的在线数字图书馆。在这里，你可以访问、阅读并搜索 Packt 的整个图书库。

## 为什么订阅？

- 可在 Packt 出版的每本书中全文检索
- 复制、粘贴、打印和书签内容
- 按需获取，可通过 Web 浏览器访问

## Packt 账户持有者的免费访问权限

如果你在 www.PacktPub.com 上拥有 Packt 账户，可以使用它立即访问 PacktLib，并查看九本完全免费的图书。只需使用你的登录凭据即可立即访问。

## 前言

Spring Roo 是一种易于使用的生产力工具，可用于使用 Spring、Hibernate、AspectJ、Spring Web Flow、Spring Security、GWT 等广泛认可的框架快速开发 Java 企业级应用程序。Spring Roo 负责创建启用 Maven 的项目、基于所选技术的企业应用程序架构、基于所选测试框架的单元和/或集成测试等。简而言之，如果你使用 Spring，那么你应该考虑使用 Spring Roo 来提高生产力。

Spring Roo 1.1 Cookbook 汇集了一系列示例，演示 Spring Roo 开发者工具如何简化使用 JPA、GWT、Spring、Flex、Spring Web Flow、Spring Security 等标准技术和/或框架快速开发企业级应用程序。本书向读者介绍了使用 Spring Roo 工具开发现实世界中的企业级应用程序。本书从基础示例开始，让读者熟悉使用 Spring Roo 工具。随着书籍的深入，读者将在航班预订应用程序的背景下了解 Spring Roo 支持的更高级功能。以循序渐进的方式，每个示例展示如何执行特定活动、执行命令时 Spring Roo 做了什么，以及它在正在开发的应用程序背景下为什么很重要。

起初，你可以通过一些简单的示例快速开始使用 Spring Roo。然后，你将学习 Spring Roo 如何使用 JPA 简化企业级应用程序持久层的创建。你将了解各种 Roo 命令，用于创建 JPA 实体、在 JPA 实体之间创建关系、使用 Spring TestContext 框架创建集成测试等。接着，本书向你展示如何使用 Spring Roo 简化使用 Spring Web MVC、Spring Web Flow 创建企业级应用程序 Web 层，以及如何为控制器对象创建 Selenium 测试。

随后，我们将重点放在使用 Spring-BlazeDS、GWT、JSON 等方面。接下来，将演示用于将电子邮件和/或消息传递功能集成到企业级应用程序中的 Spring Roo 命令。最后，我们将通过一些杂项示例结束，演示如何通过插件扩展 Spring Roo、集成安全性、创建云就绪应用程序、从企业级应用程序中移除 Spring Roo 等。

## 本书涵盖的内容

第 1 章，Spring Roo 入门，涵盖简单示例，向读者介绍 Spring Roo 工具。你将学习如何使用一些基本功能，这些功能使 Spring Roo 成为易于使用的生产力工具。

第 2 章，使用 JPA 持久化对象，涵盖设置 JPA 提供程序、创建 JPA 实体以及创建单元测试和集成测试的 Spring Roo 命令。

第 3 章，Spring Roo 中的高级 JPA 支持，侧重于用于向 JPA 实体添加动态查找器方法、在实体之间创建关系以及使用 Spring Roo 中的数据库逆向工程支持创建 JPA 实体的 Spring Roo 命令。

第 4 章，使用 Spring Web MVC 进行 Web 应用程序开发，涵盖 Spring Roo 中的 Spring Web MVC 支持。本章中的示例展示如何从 JPA 实体构建 Spring Web MVC 应用程序脚手架、对 Web 应用程序进行国际化以及向其添加不同主题。

第 5 章，使用 GWT、Flex 和 Spring Web Flow 进行 Web 应用程序开发，展示了如何使用 Spring Roo 从 JPA 实体构建 GWT 和 Flex 应用程序脚手架。本章还展示了 Spring Roo 如何让你快速开始使用 Spring Web Flow 开发应用程序。

第 6 章，电子邮件、消息传递、Spring Security、Solr 和 GAE，涵盖与添加电子邮件支持、使用 JavaMail API 进行消息传递、使用 Spring Security 集成应用程序安全性、使用 Solr 搜索服务器添加搜索功能以及为 Google App Engine (GAE) 开发应用程序相关的众多主题。

第 7 章，开发插件并从项目中移除 Roo，通过一些高级主题结束本书，例如如何创建 Spring Roo 插件、安装插件、使用 push-in 重构从项目中移除 Roo、使用 pull-up 重构将 Roo 支持添加到现有项目，以及升级到较新版本的 Spring Roo。

## 阅读本书所需内容

- Spring Roo 1.1.3 或 1.1.4 或 1.1.5
- Eclipse Helios IDE（或更高版本）或 STS
- Maven 3.x
- Java SE 6 或更高版本
- MySQL 数据库
- Eclipse IDE 的 Google 插件
- Solr 服务器
- GnuPG
- H2 数据库

## 本书适合人群

Spring Roo 1.1 Cookbook 适合初次接触 Spring Roo 工具但具有使用 Spring 框架、AspectJ、JPA、GWT 以及 Spring Roo 支持的技术/框架开发应用程序经验的开发人员。如果你初次接触 Spring 框架，那么在阅读本 Cookbook 之前，建议参考一本涵盖 Spring 的文本。

## 约定

在本书中，你会发现许多文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义：

文本中的代码单词显示如下：

The perform eclipse and perform command 命令由 Spring Roo 的 Maven 插件处理。

代码块设置如下：

```
<filter>
    <filter-name>HttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
```

当我们希望将你的注意力吸引到代码块的特定部分时，相关行或项目将以粗体显示，如下所示：

```
<mvc:annotation-driven
     conversion-service="myConversionService" />
<bean id="myConversionService" class = "..format.FactoryConversionServiceFactoryBean">
    <property name="converters">
        <list>
            <bean class="com.flight.myCustomConverter"/>
        </list>
    </property>
</bean>
```





## Preface

任何命令行输入或输出都写作如下：

```
roo> logging_setup --level DEBUG --package ROOT
Updated SRC_MAIN_RESOURCES\log4j.properties
```

**新术语**和**重要词汇**以粗体显示。您在屏幕上、菜单或对话框中看到的单词，例如，文本中会这样显示：“通过从菜单中选择**创建新飞行描述**选项并输入**源**、**目的地**和**价格**字段的值来创建新飞行描述”。

> 警告或重要说明会像这样出现在一个框中。

> 技巧和窍门会这样出现。

## Reader feedback

读者反馈始终受欢迎。请告诉我们您对本书的看法——您喜欢或不喜欢的地方。读者反馈对我们开发您真正能从中获益的书籍至关重要。

要向我们发送一般反馈，只需发送电子邮件至 <feedback@packtpub.com>，并在邮件主题中注明书名。

如果您有想要出版并希望我们出版的书籍，请通过 `www.packtpub.com` 上的 **建议书名** 表单发送备注，或发送电子邮件至 <suggest@packtpub.com>。

如果您在某个主题上有专长，并有兴趣编写或参与编写书籍，请参阅 `www.packtpub.com/authors` 上的作者指南。

## Customer support

现在您是 Packt 图书的自豪拥有者，我们有多种资源可帮助您充分利用购买的书籍。

## Downloading the example code

您可以从 `http://www.PacktPub.com` 上的帐户下载已购买的所有 Packt 图书的示例代码文件。如果您在其他地方购买了本书，可以访问 `http://www.PacktPub.com/support` 并注册以直接将文件发送到您的邮箱。

## Errata

尽管我们已竭力确保内容的准确性，但错误难免发生。如果您在我们的一本书中发现错误——也许是文本或代码中的错误——如果您能将其报告给我们，我们将不胜感激。这样做，您可以帮助其他读者避免沮丧，并帮助我们改进本书后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/support，选择您的书，单击勘误提交表单链接，并填写勘误详细信息。一旦您的勘误得到验证，您的提交将被接受，勘误将上传到我们的网站，或添加到该书名下“勘误”部分的任何现有勘误列表中。任何现有勘误均可通过从 http://www.packtpub.com/support 选择书名来查看。

## Piracy

互联网上版权材料的盗版是所有媒体中持续存在的问题。在 Packt，我们非常重视版权和许可的保护。如果您在互联网上发现我们作品的任何非法副本，请立即提供位置地址或网站名称，以便我们采取补救措施。

请通过包含涉嫌盗版材料链接的 copyright@packtpub.com 联系我们。

感谢您在保护我们的作者以及我们为您提供有价值内容方面提供的帮助。

## Questions

如果您在本书的任何方面遇到问题，可以通过 questions@packtpub.com 联系我们，我们将尽力予以解答。
