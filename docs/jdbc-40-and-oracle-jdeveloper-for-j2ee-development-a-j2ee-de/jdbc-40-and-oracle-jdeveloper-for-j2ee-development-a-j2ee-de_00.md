# J2EE 开发的 JDBC 4.0 和 Oracle JDeveloper：J2EE 开发人员的 Oracle JDeveloper 集成数据库功能使用指南构建数据驱动应用

## 致谢

| | |
|---|---|
| **作者** | **编辑团队负责人** |
| Deepak Vohra | Mithil Kulkarni |
| **审阅者** | **项目经理** |
| Frank Nimphius | Abhijeet Deobhakta |
| **策划编辑** | **开发编辑** |
| Shayantani Chaudhuri | Ved Prakash Jha |
| **技术编辑** | **索引** |
| Akshara Aware | Monica Ajmera |
| Rashmi Balachandran | **校对** |
| **代码测试** | Chris Smith |
| Rashmi Balachandran | Camille Guy |
| **文案编辑** | **制作协调员** |
| Sumathi Sridhar | Shantanu Zagade |
| **封面工作** | |
| Shantanu Zagade | |

## 关于作者

**Deepak Vohra** 是 NuBean 软件公司的顾问兼主要成员。Deepak 是 Sun 认证的 Java 程序员和 Web 组件开发人员，并在 XML 和 Java 编程以及 J2EE 领域工作了五年以上。Deepak 是 APress 出版的书籍 *Pro XML Development with Java Technology* 的合著者，并担任 OReilly 出版的书籍 *WebLogic: The Definitive Guide* 的技术审阅者。Deepak 还担任 Course PTR 出版的书籍 *Ruby Programming for the Absolute Beginner* 的技术审阅者，以及 Manning Publications 出版的书籍 *Prototype and Scriptaculous in Action* 的技术编辑。

## 关于审阅者

**Frank Nimphius** 自 1999 年以来一直担任 Oracle Corporation 应用开发工具的主管产品经理。Frank 驻德国，积极参与 Oracle JDeveloper 和 Oracle 应用开发框架 的开发。作为会议演讲者，Frank 代表 Oracle J2EE 开发团队在世界各地的 J2EE 会议上发言，包括各种 Oracle 用户组和 Oracle Open World 会议

## 索引



## 前言

数据检索和存储是 J2EE 应用中最常见的组件之一。JDBC（Java DataBase Connectivity）是用于访问结构化查询语言（SQL）关系数据库并在数据库中添加、检索和更新数据的 Java API。JDBC 3.0 规范是大多数应用服务器当前实现的标准。JDBC 4.0 规范于 2006 年 12 月发布，但尚未被大多数应用服务器实现，例如 Oracle Application Server、WebLogic Server、JBoss Application Server 和 WebSphere Application Server。

要连接 SQL 关系数据库，需要 JDBC 驱动程序；大多数数据库都提供 JDBC 4.0 驱动程序。我们将使用 Oracle JDeveloper 10.1.3 IDE 来开发书中大部分的 J2EE 应用。我们选择 JDeveloper，因为它包含一个嵌入式 J2EE 服务器，即 Oracle Containers for J2EE（OC4J）服务器，用于运行 J2EE 应用。JDeveloper 还提供内置支持，可连接到常用的应用服务器，如 Oracle Application Server、WebLogic Server、JBoss Application Server、Tomcat 服务器和 WebSphere Application Server，并将应用部署到这些服务器。另一个使用 JDeveloper 的优点是 JDeveloper 提供了对 JDBC 的内置支持。JDeveloper 还提供对 JSF 的支持；JSF 用户界面组件可以从组件调色板中选择并添加到 J2EE 应用中。

## 本书涵盖内容

第 1 章我们讨论 JDBC 4.0 规范。我们讨论 JDBC API 中常用的接口和类。我们还讨论 JDBC 4.0 规范中的新特性。

第 2 章我们在 JDeveloper IDE 中配置 JDBC。JDeveloper 为 Oracle 数据库提供内置库，也可配置第三方数据库。我们使用 MySQL 的 JDBC 4.0 驱动程序连接 MySQL 数据库。我们还讨论连接池和数据源以及 JDBC 4.0 中的新连接池和语句池特性。我们在嵌入式 OC4J 服务器中运行 Web 应用以连接 MySQL 数据库并从数据库检索和显示数据。

第 3 章我们讨论 JavaServer Pages 标准标签库（JSTL）SQL 标签。JDeveloper 10.1.3 为 JSTL SQL 标签提供组件调色板。我们使用 JSTL SQL 标签库创建数据库表、向数据库表添加数据、更新数据库表并查询数据库表。

第 4 章，我们讨论在 JBoss Application Server 中配置 JDBC，JBoss 是最常用的开源 J2EE 应用服务器之一。我们在 JDeveloper 中开发 Web 应用以连接到 MySQL 数据库并从数据库检索和显示数据，并将 Web 应用部署到 JBoss 服务器。我们还讨论该 Web 应用的 JDBC 4.0 版本。我们在 JBoss 中使用 Oracle 数据库、MySQL 数据库、DB2 数据库、PostgreSQL 数据库和 SQL Server 配置数据源。

第 5 章我们在 BEA 的 WebLogic Server 9.x 中配置 JDBC。WebLogic Server 9 提供了一些新的 JDBC 特性，例如额外的连接池属性，可提高数据源性能，并支持多数据源。我们使用 Oracle 数据库配置数据源。我们还在 JDeveloper 中开发 Web 应用以从 Oracle 数据库检索和显示数据，并将 Web 应用部署到 WebLogic 服务器。我们还讨论该 Web 应用的 JDBC 4.0 版本。

第 6 章我们在 IBM 的 WebSphere 应用服务器中配置 JDBC。WebSphere 内置支持使用常用数据库配置 JDBC Provider 和数据源。我们使用 IBM 的 DB2 UDB 数据库配置 JDBC Provider 和数据源。我们还在 JDeveloper 中开发 Web 应用以连接 IBM 的 DB2 UDB 数据库并从数据库检索和显示数据。我们在 WebSphere Application Server 中运行 Web 应用。我们还讨论该 Web 应用的 JDBC 4.0 版本。

第 7 章，我们讨论 Oracle 的 XML SQL 实用程序（XSU）将 XML 文档映射到数据库表以及将数据库表映射到 XML 文档。我们还使用 XML SQL 实用程序在数据库中更新和删除数据。我们使用 Oracle 数据库的 JDBC 4.0 驱动程序在 JDeveloper 中开发 XSU 应用。

第 8 章，我们讨论 JDeveloper 10.1.3 中对 XSQL Pages 发布框架的支持。我们使用 XSQL 通过 SQL 查询生成 XML 文档。我们还使用可扩展样式表语言转换（XSLT）转换 XSQL 查询的输出。我们还讨论可在 XSQL 应用中使用的 JDBC 4.0 特性。

第 9 章，我们讨论 Oracle JDBC 对 JDBC 3.0 规范中一项新特性 Web RowSet 的实现。Web RowSet 对象是 RowSet 的 XML 表示形式，RowSet 是 ResultSet 数据的容器，可在不连接到数据源的情况下操作。Oracle Web RowSet 是 Oracle 数据库 10g 驱动程序对 Web RowSet 的实现。我们在 JDeveloper 中开发 Oracle Web RowSet Web 应用以在 Oracle 数据库中创建、读取、更新和删除一行。我们还讨论 Oracle Web RowSet Web 应用的 JDBC 4.0 版本。

第 10 章我们在 JDeveloper 中从 Oracle 数据库创建 JSF 数据表。我们使用静态 SQL 查询和动态指定的 SQL 查询在 JSF 数据表中显示数据库数据。我们还讨论创建数据表的 JSF 应用的 JDBC 4.0 版本。

第 11 章，我们讨论另一个 JSF UI 组件 Panel Grid，用于显示和更新数据库数据。我们还使用 JSF 验证器和转换器来转换和验证存储在数据库中的输入数据。我们在 JDeveloper 中开发 JSF Panel Grid 应用，并讨论 Panel Grid 应用的 JDBC 4.0 版本。

第 12 章，我们使用 JasperReports 报表工具通过 JDBC 从数据库检索报表数据，开发 PDF 报表和 Excel 电子表格报表。我们在 JDeveloper 中开发 JasperReports Web 应用，并讨论该 Web 应用的 JDBC 4.0 版本。

第 13 章，我们使用 Apache POI HSSF 库从数据库数据创建 Excel 电子表格。我们使用 JDeveloper 的内置数据库连接支持在 JDeveloper 中创建报表。我们还讨论 Apache POI 应用的 JDBC 4.0 版本。

第 14 章，我们讨论 Oracle 应用开发框架（ADF）的 Business Component for Java（BC4J）层。业务组件基于 JDBC，由视图对象、实体对象和应用程序模块组成。我们在 JDeveloper 中使用 Oracle ADF 模型和业务组件 API 开发 Web 应用。

第 15 章，我们讨论 Hibernate，这是一个面向 Java 的对象/关系持久化和查询服务。我们使用 Hibernate 创建数据库表、向表中添加数据、从表中检索数据、更新表中的数据并删除表数据。我们将在 JDeveloper 中开发 Hibernate 应用。

## 本书适合谁阅读

本书面向 J2EE 开发者。大多数 J2EE 应用都有数据库组件，本书特别适合基于数据库的 J2EE 开发。

## 约定

在本书中，您会发现多种文本样式，用于区分不同类型的信息。以下是这些样式的一些示例及其含义。

代码有三种样式。文本中的代码词显示如下："A managed-data-source element is added to the data-sources.xml file."

代码块将按如下方式设置：

```
<resource-ref>
  <res-ref-name>jdbc/OracleDataSource</res-ref-name>
  <res-type>javax.sql.DataSource</res-type>
  <res-auth>Container</res-auth>
</resource-ref>
```

任何命令行输入和输出都按如下方式编写：

```
CREATE TABLE OE.Catalog(Journal VARCHAR(25), Publisher Varchar(25),
  Edition VARCHAR(25), Title Varchar(45), Author Varchar(25));
```

新术语和重要词语以粗体字体引入。例如，您在屏幕上看到的词语，如菜单或对话框中，会在我们的文本中这样显示："In the Create Project window specify a Project Name and click on Next"。

![](img/553e44bc30e70d0d8e58278a7ece8da8_14_0.png)

重要说明以如下框形式出现。

![](img/553e44bc30e70d0d8e58278a7ece8da8_14_1.png)

技巧和窍门如下所示。

## 读者反馈

我们欢迎读者的反馈。请告诉我们您对本书的看法，您喜欢或可能不喜欢的部分。读者的反馈对我们开发您真正受益的书名非常重要。



发送一般反馈时，只需发送电子邮件至 feedback@packtpub.com，并在邮件主题中注明图书标题。

如果您需要某本书并希望我们出版，请在 www.packtpub.com 上的 SUGGEST A TITLE 表单中给我们留言，或发送电子邮件至 suggest@packtpub.com。

如果您在某个主题方面有专业知识，并且有兴趣撰写或为图书做出贡献，请参阅 www.packtpub.com/authors 上的作者指南。

## 客户支持

现在您已成为 Packt 图书的自豪拥有者，我们提供多项帮助，让您充分利用购买。

## 下载本书示例代码

访问 http://www.packtpub.com/files/code/4305_Code.zip 直接下载示例代码。

可下载文件包含使用说明。

## 勘误

尽管我们已尽一切努力确保内容的准确性，但错误仍可能发生。如果您在我们的某本书中发现错误——可能是文本或代码中的错误——我们将不胜感激您向我们报告。这样做可以让其他读者免于困扰，并帮助改进本书的后续版本。如果您发现任何勘误，请访问 http://www.packtpub.com/support，选择您的图书，点击 Submit Errata 链接，并输入勘误详情。一旦您的勘误得到验证，您的提交将被接受并添加到现有勘误列表中。现有勘误可以通过从 http://www.packtpub.com/support 选择您的书名来查看。

## 问题

如果您在图书的某些方面遇到问题，可以联系 questions@packtpub.com，我们将尽力解决。
