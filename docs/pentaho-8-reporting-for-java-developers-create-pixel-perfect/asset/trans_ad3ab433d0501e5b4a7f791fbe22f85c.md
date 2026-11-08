
|  |  |
|---|---|
|  | 在 PDF 指南中描述。 |
| `build-res` | 包含 Apache Ant 脚本的文件夹 (http://ant.apache.org)。 |
| `lib` | 构成 Pentaho Reporting Engine 的库目录。 |
| `source` | 用于构成报表示例的源文件。 |
| `sql` | 与示例一起使用的基于文件的 HyperSQL 数据库实例。这是前几章中介绍的完全相同的 `SampleData` 模式。 |
| `WebContent` | 一个示例使用的 Web 内容信息（主要是 `WEB-INF/web.xml`）。 |

在继续其他概念之前，请查看每个文件夹的内容以熟悉 SDK 的结构和内容。在前面提到的 PDF 指南中，您将找到有关 Pentaho Reporting SDK 结构的信息。请在继续阅读以下段落之前先阅读该指南。

## 使用包含的 Eclipse 项目

如 PDF 指南所述，如果您使用 Eclipse 或 IntelliJ IDEA IDE，可以使用随 Pentaho Reporting SDK 包含的 Eclipse 项目直接使用示例。在实践中，将解压的 Pentaho Reporting SDK 目录选为工作区即可正常工作。您也可以直接从 Eclipse 中的文件浏览器启动 `Sample1.java` 和 `Sample2.java` 示例应用程序。

我们不会提供关于 Eclipse 项目的更多细节，因为它在指南中已有详细说明。从另一个角度来看，我们更愿意在接下来的章节中介绍最现代的方法，使用 Apache Maven ([https://maven.apache.org](https://maven.apache.org)) 作为软件项目管理和理解工具。

## 介绍包含的示例

Pentaho Reporting SDK 中包含五个有趣的示例。示例在 PDF 指南中有所描述，并附带介绍与 Reporting Engine 交互的概述。PDF 指南中描述的所有源代码都可在 sample 文件夹中使用，且文件名与示例严格相关。以下是每个示例的简要描述供参考：

- `sample0`：基础类。`AbstractReportGenerator` 类被 `sample1` 和 `sample2` 继承。它包含创建报表的基本逻辑，将输入和输出的细节留给继承它的类。
- `sample1`：静态报表定义，JDBC 输入，PDF 输出。最简单的嵌入场景会生成静态报表（关于数据源或查询无用户输入），使用 Pentaho 提供的 `SampleData` HyperSQL 模式进行 JDBC 输入，并在本地文件系统上生成 PDF。
- `sample2`：静态报表定义，JDBC 输入，HTML 输出。此示例生成静态报表（关于数据源或查询无用户输入），使用 Pentaho 提供的 `SampleData` HyperSQL 模式进行 JDBC 输入，并在本地文件系统上生成 HTML 文件。
- `sample3`：动态生成，JDBC 输入，Swing 输出。`Sample3.java` 生成的报表与 `sample1.java` 中创建的相同，但它使用 Reporting Engine 中定义的 Swing 辅助类在 Swing 预览窗口中渲染报表。
- `sample4`：动态生成，JDBC 输入，Java servlet 输出。`Sample4.java` 是 `HttpServlet`，它生成类似于 `sample2` 的 HTML 报表。此示例假定您已安装、配置、运行并且可访问 Java 应用服务器，例如 Tomcat 或 JBoss。

请在继续阅读以下段落的内容之前，先详细阅读 PDF 指南的内容以及五个示例的描述。如果您能在自己的环境中测试这些示例就太好了。如前一节所述，我们将继续介绍使用 Apache Maven 的不同方法。

## 使用 Pentaho Reporting 和 Maven 创建 Web 应用程序

在本节中，您将学习如何从头开始使用 Pentaho Reporting Engine 创建 Web 应用程序。如果您还想了解如何将 Pentaho Reporting Engine 嵌入到现有 Web 应用程序中，此示例很有用。在此示例中，您将看到如何创建一个 Java Web 应用程序来显示您在第 2 章 *使用 Report Designer 入门* 中开发的 Pentaho 报表。正如您可能还记得的，我们看到了如何使用 Report Design Wizard 创建 Pentaho 报表，并将其命名为 `my_first_report`。

与 Pentaho Reporting SDK 中介绍的示例不同，在本书中我们将使用 Apache Maven，这是 Java 最常用的软件项目管理和理解工具。

为了更好地描述任务并让您有机会测试正确的结果，我们将项目拆分为几个子任务：每个任务一个段落，并附带源代码和屏幕截图，以确保您能在自己的环境中成功重现示例。

![](img/9337ac94537bf7e092e1ea6531dce8fd_125_0.png)

*在 GitHub 仓库 https://github.com/fcorti/pentaho-8-reporting 中，您可以找到本书示例中描述的源代码。在 `Chapter 03 - Getting started with Reporting SDK/pentaho-reporting-web-app` 中，您可以找到这些示例的源代码。*

## 环境的前提条件

在第 2 章 *使用 Report Designer 入门* 中，我们介绍了环境应具备的唯一前提条件：Sun 的 JDK 版本 8。考虑到我们将使用 Apache Maven，请确保已在开发环境中安装它。建议的版本为 3 或更新版本。要验证安装，请从命令行运行以下命令：**mvn --version**

如果正确安装，您将看到与以下屏幕截图类似的内容：

![](img/9337ac94537bf7e092e1ea6531dce8fd_126_0.png)

如果尚未安装，Apache Maven 的安装过程很简单，只需解压归档文件并将包含 `mvn` 命令的 `bin` 文件夹添加到 `PATH`。有关如何在环境中安装 Apache Maven 的更多详细信息，请查看官方网页 https://maven.apache.org/install.html。

![](img/9337ac94537bf7e092e1ea6531dce8fd_126_1.png)

> *另一个明显的必要条件是您的机器/环境可访问互联网。Apache Maven 基于公共仓库的理念，命令从中获取库、源代码以及编译和运行应用程序所需的一切。请确保在执行以下任务期间笔记本电脑已连接到互联网。*

## 在 Java 中创建 Web 应用程序

现在您的开发环境已正确设置，下一步是创建一个新的 Java Web 应用程序。如果您已有现有 Web 应用程序并想将 Pentaho Engine 包含在其中，此特定任务可能不感兴趣您。然而，为了完整性，我们更愿意涵盖开发的整个生命周期，假设您将在自己的环境中尝试示例以便更好地理解。

让我们从头开始创建一个 Java Web 应用程序，然后在编译和首次运行之前添加初始页面。

## 使用 Apache Maven 创建 Java Web 应用程序

使用 Apache Maven 从头开始创建 Java Web 应用程序很简单，得益于 Maven Archetypes。简而言之，Archetype 是 Maven 项目模板工具包，定义为一种原始模式或模型，所有同类事物都由此制作。对于我们的需求，我们将使用 `maven-archetype-webapp` 来启动一个简单的 Web 应用程序。

> *在本書中，我们不会详细介绍 Maven Archetypes，并假设您熟悉其使用和行为的基础知识。如果不熟悉也没关系，因为我们将描述逐步的方法。如果您想了解更多，请阅读 Apache Maven 文档 https://maven.apache.org/guides/introduction/introduction-to-archetypes.html。*

回到我们的任务，在机器中确定一个文件夹来放置新的 Java 应用程序（我们将使用 `Desktop`）。打开终端，移动到工作文件夹（在我们的例子中为 `Desktop`），并执行以下命令：

```
mvn archetype:generate
-DarchetypeArtifactId=maven-archetype-webapp
-DinteractiveMode=false
-DgroupId=com.example
-DartifactId=pentaho-reporting-web-app
```

查看命令，`archetypeArtifactId` 参数指定 Maven Archetype，`interactiveMode` 参数告诉 Maven 是否向用户提问（或不使用默认值），`groupId` 参数定义 Java 源代码的根包，`artifactId` 参数定义 Web 应用程序的名称。

> *注意，一个名为 `pentaho-reporting-web-app` 的全新文件夹将被创建。新文件夹将包含整个项目，包括配置文件、库、源代码和资源。此文件夹是存储在版本控制仓库（GitHub、SVN 等）中的自然候选。*
