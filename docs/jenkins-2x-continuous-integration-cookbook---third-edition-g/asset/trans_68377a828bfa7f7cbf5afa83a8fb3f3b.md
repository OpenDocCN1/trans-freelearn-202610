

# 最新测试结果（无失败）

## 固定链接

- 上次构建（#20），3小时33分钟前
- 上次稳定构建（#20），3小时33分钟前
- 上次成功构建（#20），3小时33分钟前
- 上次失败构建（#19），3小时50分钟前

|  | 准备 | SonarQube 分析 | 构建 | 部署 | 功能测试 | 负载测试 |
|---|---|---|---|---|---|---|
| 平均阶段时间： | 11秒 | 1分13秒 | 1分58秒 | 1分13秒 | 2分5秒 | 7秒 |
| #20 6月4日 11:58 无变更 | 9秒 | 1分2秒 | 2分11秒 | 2分38秒 | 2分1秒 | 12秒 |
| #19 6月4日 11:41 无变更 | 4秒 | 52秒 | 1分39秒 | 52秒 | 2分8秒 | 1秒 失败 |
| #18 6月4日 11:35 无变更 | 19秒 | 1分44秒 | 2分4秒 | 8秒 失败 |  |  |

## 准备工作

你将在本章后面使用 Blue Ocean，但这里先安装它：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_524_0.png)

在 Jenkins 仪表板中验证插件是否安装成功：

| 插件 | 状态 |
|---|---|
| Common API for Blue Ocean | 成功 |
| Variant Plugin | 成功 |
| Jackson 2 API Plugin | 成功 |
| Metrics Plugin | 成功 |
| Web for Blue Ocean | 成功 |
| REST API for Blue Ocean | 成功 |
| JWT for Blue Ocean | 成功 |
| Favorite | 成功 |
| REST Implementation for Blue Ocean | 成功 |
| Pipeline REST API for Blue Ocean | 成功 |
| Pub-Sub "light" Bus | 成功 |
| GitHub Pipeline for Blue Ocean | 成功 |
| Git Pipeline for Blue Ocean | 成功 |
| Config API for Blue Ocean | 成功 |
| Server Sent Events (SSE) Gateway Plugin | 成功 |
| Events API for Blue Ocean | 成功 |
| Personalization for Blue Ocean | 成功 |
| BlueOcean Display URL plugin | 成功 |
| Blue Ocean Pipeline Editor | 成功 |
| Autofavorite for Blue Ocean | 成功 |
| i18n for Blue Ocean | 成功 |
| Dashboard for Blue Ocean | 成功 |
| Blue Ocean | 成功 |

现在你将在 Jenkins 中创建你的第一个流水线。

## 操作步骤...

现在让我们看看你的流水线在 Blue Ocean 用户界面中的样子。进入你创建的 FirstPipeline 流水线任务。点击 Jenkins 仪表板顶部栏中的 Blue Ocean 链接。

点击成功的流水线 20：

## Jenkins

## FirstPipeline

| 状态 | 运行 | 提交 | 消息 | 时长 | 完成时间 |
|---|---|---|---|---|---|
| ✓ | 20 | 7262ae6 | - | 8分19秒 | 3小时前 |
| ✕ | 19 | 7262ae6 | - | 5分44秒 | 4小时前 |
| ✕ | 18 | 7262ae6 | - | 4分18秒 | 4小时前 |

1.0.1 · Core 2.61 · 9b77619 · (无分支) · 2017年4月12日 02:47 AM

它将在 Blue Ocean 仪表板中提供每个阶段执行状态的详细信息。日志可在同一页面上获取：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_529_0.png)

点击顶部栏上的“Artifacts”链接，验证此流水线中可用的所有构件：

## FirstPipeline #20

流水线

变更

测试

构件

分支：-

8分19秒

无变更

提交：7262ae6

4小时前

| 名称 | 大小 |
|---|---|
| pipeline.log | - | ↓ |
| dashBoard_Test.xml | 452 字节 | ↓ |
| target/petclinic.war | 40 MB | ↓ |

全部下载

在本章中，你使用了 Build Pipeline 插件以及 Jenkins 2 及更高版本中的流水线即代码功能创建了一个流水线。

## Jenkins 界面定制

在本章中，我们将介绍以下操作指南：

- 使用 simple themes 插件为 Jenkins 换肤
- 使用 WAR 覆盖层为 Jenkins 换肤并配置资源
- 生成主页
- 创建 HTML 报告
- 高效使用视图
- 使用 Dashboard View 插件节省屏幕空间
- 利用 HTML5 浏览器发出提醒
- 为接待区提供极限视图

## 引言

本章探讨如何通过 Jenkins 进行沟通，并认识到存在不同的目标受众。

Jenkins 是一位才华横溢的沟通者。其主页显示所有任务的状态，让你能够快速做出决策。你可以轻松设置多个视图，自然地优先展示信息。Jenkins 拥有大量插件，可通过电子邮件、仪表板和 Google 服务通知你。它通过移动设备向你呼喊，当你走过大屏幕时向外辐射信息，还会用 USB 海绵导弹发射器向你开火。

它的主要受众是开发者，但不要忘记那些想要使用正在开发的软件的更广泛受众。看到 Jenkins 以一致的视图和企业风格进行定期构建，会增强人们对软件路线图的信心。本章包含一些操作指南，帮助你触及这些更广泛的受众。

在制定一致的沟通策略时，有许多 Jenkins 特有的细节需要配置。以下是在本章中将考虑的几个方面：

- **通知**：当出现问题时，开发者需要迅速知晓。Jenkins 有许多插件；你应该选择几个适合团队精神的插件。
- **页面装饰**：页面装饰器是一种为每个页面添加内容的插件。你可以通过添加自己的样式表和 JavaScript 来低成本地打造企业外观和风格。
- **覆盖 Jenkins**：使用 Maven WAR 插件，你可以在 Jenkins 之上覆盖自己的内容。你可以利用它添加自定义内容并预置主页等资源，从而增强企业外观和风格。
- **优化视图**：首页视图是以标签页形式显示的任务列表。受众使用首页快速决定选择哪个任务进行查看。插件扩展了视图类型的选项，并优化信息消化。这有可能避免进一步查看的需要，节省宝贵时间。
- **路过通知**：在视觉上辐射信息的极限视图在大显示器上看起来很棒。如果你在接待处或咖啡机等聚集地放置一台显示器，路人便会吸收任务状态变化的起起伏伏。这种视图巧妙地暗示了你公司的专业性和产品路线图的稳定性。
- **跟踪受众**：如果你公开沟通，那么应该跟踪使用模式，以便改进服务。考虑将 Jenkins 页面连接到 Google Analytics 或 Piwik（一个开源分析应用）。

## 使用 simple themes 插件为 Jenkins 换肤

本节操作指南通过 themes 插件修改 Jenkins 的外观和风格。

themes 插件是一个页面装饰器；它会用额外的 HTML 标签装饰每个页面。该插件允许你上传样式表和 JavaScript 文件。之后可通过本地 URL 访问这些文件。然后，每个 Jenkins 页面都会使用这些 URL 的 HTML 标签来加载你上传的文件。虽然简单直接，但若设计得当，视觉效果会非常强大。

## 准备工作

1. 安装 themes 插件（https://wiki.jenkins-ci.org/display/JENKINS/Simple+Theme+Plugin）。
2. 点击“安装而不重启”：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_534_0.png)

3. 等待插件安装成功：

# 正在安装插件/升级

准备

- 检查互联网连接
- 检查更新中心连接
- 成功

Static Analysis Utilities 成功
Android Lint Plugin 成功
Simple Theme Plugin 成功

返回顶部页面
（你可以立即开始使用已安装的插件）

当安装完成且没有任务在运行时，重启 Jenkins

4. 转到 Jenkins 仪表板，点击 管理 Jenkins | 配置系统 | 主题：

## 主题

主题 CSS 的 URL

指定主题 CSS 的 URL。

主题 JS 的 URL

指定主题 JS 的 URL。

你需要在此处提供自定义 CSS 和自定义 JS 文件，以更改 Jenkins 仪表板的外观和风格。

## 操作步骤...

1. 在 Jenkins 的 userContent 目录下，创建一个名为 my.js 的文件，包含以下代码行：
   
       document.write("<h1 id='test'>Example Location</h1>")
2. 在 Jenkins 的 userContent 目录下创建一个 mycss.css 文件，包含以下代码行：
   
       @charset "utf-8";
       #test {
       background-image: url(/userContent/camera.png);
       }
       #main-table{
       background-image: url(/userContent/camera.png)
       !important;
3. 从 http://sourceforge.net/projects/openiconlibrary/files/0.11/open_icon_library-standard-0.11.tar.bz2/download 下载并解压图标存档，查看可用的图标。或者，你可以使用本书网站（www.packtpub.com/support）下载中包含的图标。将图标添加到 userContent 目录，并将其重命名为 camera.png。
4. 访问 Jenkins 主配置页面：/configure。在“主题”部分，填写 CSS 和 JavaScript 文件的位置：
   - 主题 CSS 的 URL：/userContent/mycss.css
   - 主题 JS 的 URL：/userContent/myjavascript.js
5. 点击“保存”。
6. 返回 Jenkins 主页并检查你的成果，如下图所示：

![](img/2a4d2e09d9af2c678ea8f43ea344059d_536_0.png)

另一种更改 Jenkins 主题的方法是在 CSS 字段中使用以下任一 URL 来更改外观和风格：

- https://jenkins-contrib-themes.github.io/jenkins-material-theme/dist/material-red.css
- https://jenkins-contrib-themes.github.io/jenkins-material-theme/dist/material-pink.css
- https://jenkins-contrib-themes.github.io/jenkins-material-theme/dist/material-purple.css

