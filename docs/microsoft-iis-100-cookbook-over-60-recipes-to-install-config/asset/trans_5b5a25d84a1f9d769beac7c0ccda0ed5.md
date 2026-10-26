

- 3. 默认情况下，所有网站文件都将存储在 `wwwroot` 文件夹中。路径为 `C:\inetpub\wwwroot`。以下截图显示了默认的 IIS 10.0 网站文件：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_51_0.png)

- 4. 现在，我们来看 `logs` 文件夹。它位于 `C:\inetpub\logs`：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_51_1.png)

- 5. 启动 IIS 管理器并选择 **Default Web Site**。你将在右侧获得 **Actions** 窗口。在 **Actions** 列表中，我们有 **Edit Site** 选项。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_52_0.png)

- 6. 点击 **Basic Setting...** 文本按钮。你将获得以下基本设置窗口：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_52_1.png)

- 7. 我们选择了 **Default Web Site**；现在你可以看到 **Site name**、**Application pool**、**Physical path** 和 **Connect as...** 身份验证选项。

- 8. 在 **Edit Site** 窗口中，你可以更改 **Application pool** 并定义你的网站文件夹路径或默认的身份验证类型。最后，你可以测试默认设置。

- 9. 让我们看一些绑定选项。在 **Actions** 列表中，你会看到有一个 **Bindings...** 选项。绑定用于定义你的网站参数：**Type** `http`、**Host Name** `www.xyz.com`、**Port** `80`，以及 **IP Address** `*`，例如，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_53_0.png)

- 10. 现在，为网站访问设置 **Limits**。在 IIS 管理器右侧可用的 **Actions** 列表中，进入 **Default Web Site** 属性。

## Hosting static web page

在本教程中，我们将托管我们的简单 HTML 网页。

### Getting ready

我们需要创建一个 HTML 网页并运行 IIS 10.0。你还需要用户管理员权限来托管站点。

### How to do it...

- 1. 打开记事本并输入一些内容，如以下截图中所示。你可以写任何你想要的内容。
- 2. 将记事本文件保存在桌面上，名称为 index.html。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_55_0.png)

- 3. 接下来，你需要打开 IIS 管理器，选择 Default Web Site，然后右键单击它：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_56_0.png)

- 4. 从弹出菜单中点击 Explore 选项。

- 5. 它将打开 Inetpub 文件夹，从 C:\inetpub\wwwroot 默认网站文件夹位置开始，在那里你将保存你的 Web 应用程序或网站文件。wwwroot 文件夹是你的托管目录。

- 9. 点击 **Default Web Site**。你将看到属性页面弹出：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_58_0.png)

- 10. 打开 **Default Document**。你会找到 Default.htm、Default.asp、Index.html 等多个名称列出。我们的文件名是 Index.html，默认文档中已包含相同的名称。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_59_0.png)

### How it works...

一旦我们检查默认文档配置并发现 index.html 名称已列出，我们需要打开网页，默认主页将会打开。默认文档设置对于启动你的网站非常有帮助，因为你会选择它，并且需要告诉 IIS 服务器你的主页是什么。

## Testing a static-page website

我们已经将静态 index.html 页面上传到 wwwroot 文件夹，并检查了默认主页的默认文档设置。现在让我们看看如何测试静态网页。

### Getting ready

我们需要测试网页是否正常工作。你需要一个正在运行的 IIS 10.0 服务器实例和一个静态 HTML 网页。

我们已经创建了 `index.html` 页面并将其上传到 IIS `wwwroot` 目录。

### How to do it...

- 1. 打开 Internet Explorer。
- 2. 你需要在地址栏中提供你的网页 URL。我们在 localhost（本地服务器）上安装了 IIS 10.0。
- 3. 输入 URL `http://localhost/` 并按 **Enter**。你的页面将如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_60_0.png)

### How it works...

一旦你通过 IIS 访问网页，页面将通过 HTTP 端口 80 显示。你可以尝试上传更多页面。

应用程序池的好处是，如果其中一个应用程序池出现问题，它只会影响相关的应用程序池。

应用程序池使我们能够隔离 Web 应用程序，以获得更好的安全性、可靠性和可用性。

有两种类型的应用程序管道。我们可以选择集成模式（支持 IIS 7.5 或更高版本）或经典模式（IIS 6.0）。

## Understanding application pools in IIS 10.0

在本教程中，我们将了解应用程序池。我们可以简单地说，应用程序池是 IIS 10.0 的核心。应用程序池是 Web 应用程序的逻辑分组，它们将在一个公共进程中执行，从而允许对哪些程序在单个进程中聚类进行更大的粒度控制。例如，如果你需要每个 Web 应用程序都在单独的进程中执行，你只需为每个不同框架版本的应用程序创建一个应用程序池即可。

假设我们有多个版本的网站，一个支持框架 2.0，另一个支持框架 4.0 或其他不同的应用程序，如 PHP 或 WordPress。所有这些网站进程都通过应用程序池进行管理。

### Getting ready

要逐步完成本教程，你需要一个正在运行的 IIS 10.0 实例。你还应具有管理员权限。

- 3. 点击 **DefaultAppPool**，然后你将获得 **DefaultAppPool** 的 **Actions** 面板。在这里，你将看到右侧高亮显示的 **Application Pool Tasks** 选项，你可以 **Start**、**Stop** 和 **Recycle** IIS 10.0 的服务。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_64_0.png)

- 5. 在 **Edit Application Pool** 部分，你可以更改应用程序池的设置到 **Basic Settings...** 和 **Advanced Settings...**、**Rename** 应用程序池，并执行 **Recycling....**

### How it works...

让我们回顾一下我们在 IIS 管理器和应用程序池中探索的内容。我们了解了应用程序池的基础知识以及可以在其中进行更改的属性。默认的 IIS 10.0 应用程序池框架是 v4.0，支持到 v4.6，但我们还有更多安装不同版本应用程序池的选项。我们可以轻松自定义应用程序池，这有助于我们满足典型的 Web 应用程序需求。

在操作窗格中，我们有多个应用程序池选项。我们可以添加新的应用程序池，并且可以启动、停止和回收应用程序池任务。我们也可以执行编辑和自动回收。

在下一个教程中，我们将学习更多关于应用程序池的内容，以安装较低的框架版本。

## Installation of lower framework version

在本教程中，我们将在 Windows Server 2016 上安装框架 3.5。默认情况下，IIS 10.0 具有框架版本 4.0。我们将安装较低版本的框架，它支持使用 .NET 框架版本 2.0 到 3.5 的 Web 应用程序。

例如，如果你有一个几年前创建的 Web 应用程序，它是在 v2.0 .NET 框架中开发的，你可以使用本教程使其在 IIS 10.0 上运行。让我们开始吧。

### Getting ready

要逐步完成本教程，你需要安装框架 v3.5，它基于 v2.0 框架。你需要一个 Windows Server 2016 实例。你应该有 Server 2016 操作系统媒体用于框架 3.5 文件或互联网连接。你还需要管理员权限。

### How to do it...

- 1. 在 Windows Server 2016 上打开服务器管理器，并点击突出显示的 Add roles and features 选项。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_66_0.png)

