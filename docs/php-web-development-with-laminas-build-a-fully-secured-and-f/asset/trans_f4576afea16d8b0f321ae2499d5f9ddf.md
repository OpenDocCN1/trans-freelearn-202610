


![](img/e6cf5550804ccae9d1859270da8688f5_44_0.png)

图 2.11 – 菜单中的“创建 Laminas 项目”命令

单击**创建 Laminas 项目**将打开一个对话框，如以下屏幕截图所示。将`mysecondlaminas`作为项目名称，然后单击“确定”：

![](img/e6cf5550804ccae9d1859270da8688f5_44_1.png)

图 2.12 – “创建 Laminas 项目”命令的对话框

名为**控制台**的视图将在底部打开，并显示您在终端中运行该命令时会看到的相同输出。当`<terminated>`出现在**控制台**视图的顶部时，即表示完成，如以下屏幕截图所示：

![](img/e6cf5550804ccae9d1859270da8688f5_44_2.png)

图 2.13 – 控制台视图

要打开新项目，您必须访问以下屏幕截图中显示的“文件 | 新建 | PHP 项目”菜单。将**项目名称**字段填写为`mysecondlaminas`，然后单击**完成**：

![](img/e6cf5550804ccae9d1859270da8688f5_45_0.png)

图 2.14 – 新 PHP 项目屏幕

`mysecondlaminas`项目现在将在**项目资源管理器**视图中打开。如果您单击项目名称，它将显示其内容，如以下屏幕截图所示：

![](img/e6cf5550804ccae9d1859270da8688f5_46_0.png)

图 2.15 – mysecondlaminas 项目

该项目不是由 Eclipse 创建的，而是因为它已存在而与 Eclipse 关联。现在，Eclipse 将 mysecondlaminas 文件夹识别为 PHP 项目，并启用与该编程语言相关的所有功能。

现在您知道如何在 Eclipse 中创建 Laminas 项目了，您可以删除 mysecondlaminas 项目。为此，您必须右键单击项目名称并选择**删除**。这将打开以下对话框：

![](img/e6cf5550804ccae9d1859270da8688f5_47_0.png)

图 2.16 – 删除资源对话框

现在冷静点。等等。如果您单击“确定”，将取消该项目作为 Eclipse 项目的关联。它将继续存在于文件系统中。要真正删除项目并删除所有文件，您必须选中**删除磁盘上的项目内容（此操作无法撤销）**选项。是的，这是一条令人生畏的消息，但您可以选中它并删除`mysecondlaminas`项目，因为我们不再需要它了。是的，这次您可以单击“确定”——无需担心。

如果我们需要在 Eclipse 与其他程序之间进行更多集成，我们将适当地进行这些集成。目前，只需知道您能够将程序集成到 Eclipse 工具栏中即可。此外，您可能也迫不及待地想要做点什么——我是说，一些可以运行的代码。保持冷静——在开始编码之前，我们需要为练习定义数据模型。我们开始吧！

## 管理 MySQL

如前所述，XAMPP 自带一个用于管理 MySQL 数据库的 phpMyAdmin 实例，可通过 `https://localhost/phpmyadmin` 访问。在 XAMPP 中，默认情况下 MySQL 超级用户 `root` 没有密码。因此，当您第一次访问 phpMyAdmin 时，将进入 phpMyAdmin 主页，如以下屏幕截图所示：

![](img/e6cf5550804ccae9d1859270da8688f5_48_0.png)

图 2.17 – phpMyAdmin 主页

此页面包含两个框架：左侧显示数据库树，右侧显示环境概览。

如您所见，已经存在一些系统数据库，例如 information_schema 和 performance_schema。还有一个名为**新建**的节点。单击**新建**，您将在右侧看到以下屏幕截图所示的框架：

![](img/e6cf5550804ccae9d1859270da8688f5_48_1.png)

图 2.18 – phpMyAdmin 主页（续）

您可以使用此框架创建将在我们的练习中使用的数据库。只需填写数据库名称的输入文本，然后单击**创建**。我们需要创建两个数据库：`school` 和 `whatstore`。创建它们。您将能够在左侧看到它们。不用担心表，因为我们将使用脚本来创建它们。这些脚本可在 https://github.com/PacktPublishing/PHP-Web-Development-with-Laminas/tree/main/databases 获取。

`school` 数据库将用于_启动引擎_。它只有两个表：`classes` 和 `students`。它是一个学校系统的精简版，但存在一对多关系。带有字段和数据类型的表结构可以在以下屏幕截图中看到：

![](img/e6cf5550804ccae9d1859270da8688f5_49_0.png)

图 2.19 – school 数据库的数据模型

`whatstore` 数据库将用于我们的挑战性练习——电子商务应用程序。它有 12 个表，分为 2 组。

一组是业务核心，由以下表组成：

- customers
- discounts
- inventory
- order_items
- products
- purchase_orders
- transactions

另一组是管理模块，由以下表组成：

- employees
- employee_roles
- resources
- resources_role
- roles

带有字段和数据类型的表结构可以在以下屏幕截图中看到：

![](img/e6cf5550804ccae9d1859270da8688f5_50_0.png)

图 2.20 – whatstore 数据库的数据模型

`school` 数据库的脚本是 `school.sql` 文件，`whatstore` 数据库的脚本是 `whatstore.sql`。您可以从 GitHub 仓库下载它们。

几段前我告诉过您什么是**统一资源定位符 (URL)**——还记得吗？要从这些脚本创建表，我们可以使用 phpMyAdmin 的导入页面。首先，在左侧选择一个数据库，单击其名称。例如，让我们选择 `school` 数据库。选择该数据库后，单击右侧框架中的**导入**选项卡。然后，您将看到类似于以下屏幕截图所示的页面。要导入脚本，请单击**浏览**按钮并在文件系统中查找文件。对于 `school` 数据库，脚本是 `school.sql`，对吧？

![](img/e6cf5550804ccae9d1859270da8688f5_51_0.png)

图 2.21 – phpMyAdmin 导入页面

选择脚本文件后，您必须单击右侧页面底部的**执行**，如以下屏幕截图所示：

![](img/e6cf5550804ccae9d1859270da8688f5_51_1.png)

图 2.22 – 导入页面底部的“执行”按钮

等待脚本处理完成，您将看到以下页面，其中包含导入结果：

![](img/e6cf5550804ccae9d1859270da8688f5_52_0.png)

图 2.23 – 结构化查询语言 (SQL) 架构导入过程结束

在左侧，我们将看到新的表节点。对于 `school` 数据库，我们将看到 `classes` 和 `students` 节点。现在，对 `whatstore` 数据库执行相同的过程。

我们终于完成了开发环境的配置。我们拥有练习所需的一切。希望您已成功遵循环境指南，以避免在下一章中出现不愉快的惊喜。正如本杰明·富兰克林所说：“未准备即是准备失败。”

## 摘要

在本章中，我们安装了一个通用的 PHP 环境，包含 Web 服务器、PHP 解释器和关系型数据库管理系统。该环境基于 XAMPP，这是一个用于 PHP 开发的免费软件和开源软件包，适用于最常用的操作系统。

我们学习了安装和使用 Laminas 的两种不同方式：作为 PHP 应用程序（无论其结构是 MVC 还是非 MVC）使用的隔离组件；以及作为 MVC 应用程序的容器，将应用程序的控制权委托给框架。

我们还安装了一个免费开源 IDE：Eclipse for PDT。我们演示了如何将带有**命令行界面 (CLI)** 的程序集成到 Eclipse 工具栏中，配置一个在 Eclipse 内创建 Laminas 项目的条目。

最后，在 phpMyAdmin（嵌入到 XAMPP 中）的帮助下，我们创建了本书练习所需的数据库——school 和 whatstore。

在下一章中，我们将通过一些使用 school 数据库的练习来学习使用 Laminas 编程的基础知识。
