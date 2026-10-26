

11. 从模块菜单中，选择 **编辑XML/XSL选项**（或点击 **编辑XML/XSL选项** 链接）。

12. 提供以下链接信息：

    - **链接类型：URL（指向外部资源的链接）**
    - **位置：** `http://feeds.digg.com/digg/container/technology/popular.rss`

    ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_321_0.png)

13. 在 **XSL转换设置** 下：

    - **链接类型：文件（站点上的文件）**

    ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_321_1.png)

14. 点击 **上传新文件**，然后点击 **浏览** 按钮。
15. 浏览到您在临时文件夹中创建的 `news_feed.xsl` 文件。
16. 点击 **上传所选文件**。
17. 点击 **更新** 以保存更改。

    ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_322_0.png)

18. XML 源现在将按照 XSLT 文件中描述的格式进行显示。

## 控制友好 URL 重写

自版本 3 起，DNN 提供了一种机制，可自动将用于 DNN 页面的真实 URL 转换为对用户和搜索引擎更友好的“友好”URL。例如，由于搜索引擎往往忽略使用参数的页面，URL http://www.dotnetnuke.com/default.aspx?tabid=527 可以被转换为更友好的 URL http://www.dotnetnuke.com/tabid/527/default.aspx。

此 URL 重写是通过遵循一组规则自动完成的，这些规则保存在位于 DNN 安装根文件夹中的名为 SiteUrls.config 的特殊文件中。但在某些情况下，您可能希望更改自动重写的执行方式。

在本教程中，我们将展示如何覆盖 URL 重写，以创建我们自己的指向特定 DNN 页面的友好 URL 映射。

### 准备工作

对于本教程，假设我们创建了一个 DNN 页面，其 URL 为 http://localhost/dotnetnuke/default.aspx?tabid=123。由于这是一个不友好的 URL，我们将创建一个指向同一位置的友好“别名” http://localhost/dotnetnuke/SamplePage.aspx。

> URL 重写规则使用正则表达式编写。您可以在 http://www.regular-expressions.info 了解更多关于正则表达式语法的信息。

3. 要创建我们自己的 URL 重写，请添加以下规则：

   ```xml
   <?xml version="1.0" encoding="utf-8" ?>
   <RewriterConfig>
       <Rules>
           <RewriterRule>
               <LookFor>.*/SamplePage.aspx</LookFor>
               <SendTo>~/default.aspx?tabid=123</SendTo>
           </RewriterRule>
           <RewriterRule>
               <LookFor>.*DesktopDefault.aspx(.*)</LookFor>
               <SendTo>~/Default.aspx$1</SendTo>
           </RewriterRule>
       </Rules>
   </RewriterConfig>
   ```

4. 保存该文件。

5. 使用此规则后，任何访问 http://www.yoursite.com/SamplePage.aspx 的用户都将自动重定向到 http://www.yoursite.com/default.aspx?tabid=123。

### 另请参阅

创建您自己的重定向 URL 时有一条警告：如果您重定向到站点调用的另一个位置中存在的类似名称的文件，可能会得到意外结果。因此，请始终在开发站点上测试您的更改，然后再在生产站点上进行修改。

## 使用 Google Analytics 跟踪您的站点

Google Analytics 是 Google 提供的一种工具，用于跟踪您网站上的活动。它的工作原理是在您的页面上放置一个跟踪 ID，当用户访问您的站点时，Google 可以记录并报告该 ID。

要在 DNN 中使用 Analytics，您需要配置 Google Analytics 模块，并且需要一个可公开访问的 DNN 门户，以便跟踪访问者。您还需要注册一个 Analytics 帐户，不过如果您有 Google Gmail 帐户，也可以使用它。

### 准备工作

在本教程中，我们将使用一个示例 Gmail 帐户登录 Google Analytics，并获取一个可在 DNN 站点中使用的跟踪编号。

要跟随本教程操作，您必须具备以下条件：

- 一个 Analytics 或 Gmail 帐户（您可以访问 http://mail.google.com 并点击 **创建帐户** 按钮免费获取）。
- 您帐户的用户名和密码。

### 操作步骤...

1. 第一步是从 Google 获取 Analytics 跟踪 ID。如果您已有跟踪 ID，请跳到步骤 13，否则请先打开您常用的网络浏览器，转到 Google Analytics 主页（`http://www.google.com/analytics`）。

   ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_325_0.png)

2. 如果您已经注册过 Analytics，请点击 **访问 Analytics** 按钮。否则，请点击 **立即注册** 链接。

3. 您可以使用 Gmail 地址和密码登录。

4. 如果您是 Analytics 的新用户，您将看到 Google Analytics **注册** 按钮。

   ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_325_1.png)

5. 点击 **注册** 按钮。

   ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_325_2.png)

16. 输入 Analytics 概览页面中的跟踪 ID。

    ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_327_0.png)

17. 点击 **更新** 以保存更改。您应该会看到一条消息，表明您的配置已成功更新。

18. 要确认 Google 正确跟踪了您的站点，请返回 Google Analytics 概览页面。

    ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_327_1.png)

19. 在站点配置文件顶部附近查找 **检查状态** 链接。点击该链接，Google 将检查您的站点是否存在跟踪 ID，并确认跟踪已安装。

### 工作原理...

Google Analytics 的工作原理是将一小段包含唯一跟踪 ID 的 JavaScript 代码插入到您要跟踪的每个页面中。当访问者浏览您的 DNN 页面时，Google 会记录该 ID，并在您的 Analytics 概览页面上提供结果。此外，如果您使用 Google 网站管理员帐户，它还将允许您访问其他 Google 选项。

## 将日历事件发布为 RSS 源

正如我们在*第 3 章，现有模块的简单技巧*中看到的，事件模块以日历格式显示即将发生的事件列表。除了基本的日历功能外，事件模块还提供了用于管理和发布事件的其他功能。

在本教程中，我们将了解如何将日历事件发布为 RSS 源，以便您的用户订阅并在 DNN 站点之外查看。

- 包含天数：365
- 源标题：2010 日历
- 源描述：2010 办公室活动

7. 点击 **更新** 以保存更改。

8. 当您返回事件页面时，您会看到日历顶部出现了一个小的 RSS 图标：

   ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_329_0.png)

9. 点击该图标将允许用户订阅 RSS 并在浏览器中查看事件。例如，在 Firefox 等浏览器中，该源将显示为：

   ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_329_1.png)

### 更多内容...

您可以使用日历事件源做的一件事是通过在您自己的门户内消费该源来创建“即将发生的事件”页面。

## 设计您自己的表单

表单和列表（用户定义表的后继者）是一个功能强大的 DNN 模块，用于创建您自己的表单并将其显示在页面上。作为门户管理员，您可以定义要收集的数据，并授予用户添加、更新和删除记录的权限。

在本教程中，我们将了解如何使用 DNN 提供的数据类型定义一个新表单，以及如何收集数据并将其显示在可搜索的列表中。

为了更容易定义表单，DNN 提供了特殊的数据类型来描述数据。此表显示了本教程中将使用的一些可用类型。

| 数据类型 | 描述 |
| --- | --- |
| 文本 | 用于保存来自编辑框、单选按钮或下拉列表的文本信息。 |
| 货币 | 保存一个四舍五入到 2 位小数的值，并使用当前区域性设置的货币符号进行格式化。 |
| 日期 | 保存日期值。在表单中使用时，DNN 将提供一个弹出式日历。 |
| 下载 | 保存附加到记录的文件。 |
| 电子邮件 | 创建 mailto 链接。 |
| 图片 | 保存具有可选尺寸的图像。 |
| 整数 | 保存整数。 |
| 用户资料链接 | 显示指向用户资料的超链接。 |
| 分隔符 | 用于在表单上对列进行分组。 |
| 是/否 | 显示复选框。 |
| URL | 保存格式化后的 URL。 |

### 准备工作

在本教程中，我们将定义一个部门表单，用于记录公司中的各个部门。我们将存储唯一的部门编号、部门名称以及其他相关信息。

### 操作步骤...

1. 首先以门户管理员身份登录（在我们的示例中为 `sysadmin`）。

2. 将 **表单和列表** 模块添加到新页面或现有页面（如果在模块下拉列表中没有看到表单和列表模块，请参阅*第 1 章，安装与设置*中的*安装标准 DNN 模块*教程）。

3. 要定义一个新表单，请点击 **表单和列表配置** 链接或图标。

   ![](img/65b6b07ef38faa4ed1c549e9eb9aa24b_330_0.png)

