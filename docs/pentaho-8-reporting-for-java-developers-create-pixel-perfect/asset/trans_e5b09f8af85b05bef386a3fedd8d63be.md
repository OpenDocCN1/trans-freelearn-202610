
如您所见，该公式是对 JavaScript 源代码的动态计算，并用包含结果的字符串进行提醒。

另一个使用动态 URL 的交互的有趣示例，可通过点击 PRODUCT 字段（每行）看到。在这种情况下，会在另一个窗口中打开 Google 页面，搜索所选产品。在下面的屏幕截图中，您可以看到点击 1958 Chevy Corvette Limited Edition ROW 的结果示例：

![](img/9337ac94537bf7e092e1ea6531dce8fd_626_0.png)

也在这种情况下，on-click 属性的开发实现了魔术。在下面的公式中，我们可以看到从报表元素（PRODUCTNAME 的文本字段）生成的 JavaScript 代码的动态部分： = CONCATENATE(
"window.open('','http://www.google.com/search?q=";
[PRODUCTNAME];
"','_blank');")

## 操作 Pentaho 报表的內容

为了展示元素操作的示例，在本节中您将看到如何更改标题（一个普通的 `label` 元素）的内容，使其显示当前时间的动态内容。再次从同一报表开始，点击 CHANGE TITLE 按钮，您将看到类似以下屏幕截图的结果：

![](img/9337ac94537bf7e092e1ea6531dce8fd_627_0.png)

如您所见，报表的标题相应地发生了变化。

该示例的开发遵循相同的原则，但应用略有不同：`title` 元素的 `xml-id` 属性已定义，而 `button` 元素的 `on-click` 属性包含以下作为静态文本的 JavaScript：

```
var currentdate = new Date();
document.getElementById('title').innerHTML = 'Title changed at ' +
  currentdate.getHours() + ':' + currentdate.getMinutes() + ':' +
  currentdate.getSeconds();
```

## 主/明细报表交互示例

最后，让我们看看如何开发报表之间最常见的交互之一：从一个报表导航到另一个报表，其中目标报表的内容取决于一个参数。要开发此示例，您需要第二个报表，在本例中命名为 `interactive_report_2.prpt`。第二个报表显示一行中包含的所有产品。正如您所预期的那样，行值是必填参数。

为了开发此交互，Java 项目包含另一个名为 `/reporting2` 的端点，连接到第二个报表。实现定义在 `PentahoServlet2` 类中，包含以下 Java 代码：

```
// Getting the report.
ResourceManager manager = new ResourceManager();
manager.registerDefaults();
Resource res = manager.createDirectly(
new URL("file:resources/interactive_report_2.prpt"),
MasterReport.class);
MasterReport report = (MasterReport) res.getResource();

// Mandatory parameter.
report.getParameterValues().put("LINE", request.getParameter("line"));

// Conversion to HTML and rendering.
response.setContentType("text/html");
HtmlReportUtil.createStreamHTML(report, response.getOutputStream());
```

现在第二个报表已准备好被调用，让我们看看如何在第一个报表 `interactive_report_1.prpt` 中开发此交互。源报表中所需开发始终相同：对目标 URL 的动态调用，定义报表元素的 `on-click` 属性。在本例中，`on-click` 属性定义如下：

```
=CONCATENATE(
"window.open('','";
"/reporting2?line=";
[PRODUCTLINE];
"','_self');"
```

在下面的屏幕截图中，用户体验展示了第一个报表（interactive_report_1.prpt）和第二个报表（interactive_report_2.prpt）之间的交互，当点击 LINE 字段的经典汽车行时：

![](img/9337ac94537bf7e092e1ea6531dce8fd_630_0.png)

![](img/9337ac94537bf7e092e1ea6531dce8fd_630_1.png)

如果您计划在复杂用例中开发交互式报表，请始终记住考虑与仪表板框架或 OLAP 客户端更相关的其他解决方案。建议始终记住，报表解决方案很棒，但其主要目标是在静态或参数化内容中显示详细或汇总数据。如果您强迫使用报表解决方案来开发仪表板或真正动态和交互的内容，失败的风险很高。请注意！

## 摘要

本章中，您学习了开发交互式报表的基础知识，并看到了一些可在未来开发中使用的实用示例。在发现技术解决方案后，已向您提出适度使用交互式功能的建议。

现在您已阅读本章，您应该能够使用超链接事件和许多不同的 HTML 或 JavaScript 事件生成交互式报表。本章结束了本书中关于 Java 开发和 API 的部分。在下一部分，您将看到如何在 Pentaho Suite 的其他组件中使用 Pentaho 报表。

## 在 Pentaho Business Analytics Platform 中使用报表

本章是关于在 Pentaho Suite 的其他组件中使用 Pentaho 报表的短系列的第一章。特别是在本章中，您将学习如何在 Pentaho Business Analytics Platform 中使用现有的 Pentaho 报表。Pentaho Business Analytics Platform 是一个用于最终用户和开发人员的 Web 应用程序，使他们都能够使用其分析工具，例如仪表板、透视会话，当然还有报表。

在本章中，您将介绍 Pentaho Business Analytics Platform 的基础知识，以及如何安装它并上传 Pentaho 报表的描述。之后，您将学习如何管理权限并使用 Pentaho CTools 直接调用或包含在 Pentaho 仪表板中预览报表。

本章作为开发人员和信息技术人员的教程编写。最好的学习方式是在阅读时在笔记本电脑上按照说明进行。到本章结束时，您将熟悉 Pentaho Business Analytics Platform 与 Pentaho 报表联合使用基础知识。这对于您未来的项目将非常有用，使您能够为最终客户和管理人员提供功能齐全的前端。

## 介绍 Pentaho Business Analytics Platform

作为 Pentaho 产品套件的一部分，Pentaho 提供了一个开源的 Business Analytics Platform。Business Analytics Platform 是一个 Web 应用程序，允许用户在企业商业智能系统中发布和管理报表。Business Analytics Platform 提供了许多功能，包括 Pentaho 报表的管理和执行。通过结合 Pentaho Reporting 和 Pentaho 的 Business Analytics Platform，信息技术人员可以在其环境中使用 Pentaho Reporting，而无需编写任何代码。除了报表的发布和执行之外，开源的 Business Analytics Platform 还允许调度、后台执行、安全性和更多功能。

使用 Pentaho 的企业版 Business Analytics Platform，可获得额外的功能，但在本书中，我们更喜欢使用社区版，因为它可下载且对所有人免费，无需任何费用。

## 安装 Business Analytics Platform

Pentaho Business Analytics Platform 的安装很简单，遵循 *unzip and run* 任务。运行 Pentaho Business Analytics Platform 的先决条件很简单；它需要一个已安装在您环境中的 JRE 8。Windows 操作系统和 Linux 操作系统均受支持。本章中显示的屏幕截图和预览均使用 Ubuntu 16.04.2 LTS 发行版完成。

在安装应用程序之前，您可以直接从官方 Pentaho 社区网站 (http://community.pentaho.com/) 或 Pentaho SourceForge 网站 (https://sourceforge.net/projects/pentaho/) 下载以 ZIP 文件分发的工件。在这两种情况下，您都应下载一个名为类似 `pentaho-server-ce-<version>.zip` 的 ZIP 文件。

一旦 ZIP 文件下载到您的环境中，将其解压到最终路径（在 Linux 操作系统上，建议使用 `/opt` 路径），您将在文件夹 `pentaho-server` 中拥有所有 Pentaho Business Analytics Platform 数据。

虽然您可能不敢相信，但这就是将 Pentaho Business Analytics Platform 安装到您的环境中所需的一切。

Pentaho Business Analytics Platform 主要是部署在 Apache Tomcat Servlet Container (https://tomcat.apache.org/) 中的 Web 应用程序。您可以通过打开终端并执行存储在 `pentaho-server` 文件夹中的 `start-pentaho` 脚本轻松运行 Pentaho Business Analytics Platform（Linux 操作系统的 `.sh` 扩展名文件，Windows 操作系统的 `.bat` 扩展名文件）。

要检查 Web 应用程序的运行情况，您可以查看日志文件 `<pentaho-server>/tomcat/logs/catalina.out`。一旦应用程序启动，您可以使用受支持的 Web 浏览器通过链接 `http://<server>:8080/pentaho` 访问用户界面。使用 Pentaho Business Analytics Platform
