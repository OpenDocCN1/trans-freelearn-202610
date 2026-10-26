

## 将 .NET 网页上传到通配符主机

在本节操作中，我们将为 ssl.v2mysite.com 通配符主机站点上传网站应用程序页面。这些网站应用程序页面是使用 .NET Framework 4.0 开发的。

我们还将一些简单的 HTML 网页上传到 demo.v2mysite.com 通配符主机，以便进行演示。

### 准备工作

要完成本节操作，你需要一个正在运行的 IIS 10.0 实例。你应该已经创建并配置好了 ssl.v2mysite.com 和 demo.v2mysite.com 通配符主机。你还应具有管理员权限。

### 操作步骤...

- 1. 登录 **Windows Server** 2016，然后转到 **服务器管理器**。
- 2. 在 Windows Server 2016 上打开服务器管理器。单击 **工具** 菜单并打开 IIS 管理器。
- 3. 展开 IIS 服务器（**WIN2016IIS**）。我们会看到列出的 **应用程序池** 和 **网站**。在站点文件夹中会看到三个网站——**mysite.com**、**v2mysite.com** 和 **v4mysite.com**——以及两个通配符主机，**ssl.v2mysite.com** 和 **demo.v2mysite.com**。现在，我们将为 **ssl.v2mysite.com** 和 **demo.v2mysite.com** 通配符主机站点上传网页。
- 4. 单击通配符主机 **ssl.v2mysite.com**，转到 IIS 管理器控制台的右侧，你将看到 **操作** 面板。在 **操作** 面板中，有一个名为 **浏览** 的选项：

  ![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_188_0.png)

- 5. 单击 **浏览** 选项；它将打开所选通配符 **ssl.v2mysite.com** 的内容目录，或者你也可以直接指定通配符主机文件夹的物理文件夹路径，如下所示：

  ![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_188_1.png)

- 10. 现在关闭该文件夹。我们使用了两种类型的通配符主机。ssl.v2mysite.com 主机是应用程序通配符主机，第二个是 demo.v2mysite.com，这是一个简单网站类型的通配符主机。

### 工作原理...

在本节操作中，我们将用 ASP.NET 编写的网站应用程序页面上传到了通配符主机 ssl.v2mysite.com。我们检查了 default.aspx 应用程序页面的属性。

我们还将简单的 HTML 网页上传到了通配符主机 demo.v2mysite.com。

## 测试上传的网站页面

在本节操作中，在开始测试这些站点之前，务必验证通配符主机 ssl.v2mysite.com 和 demo.v2mysite.com 的默认文档设置。我们将测试使用 Framework 4 版本开发的 Web 应用程序页面，并将它们上传到通配符主机 ssl.v2mysite.com。我们还将使用已上传网页的通配符主机 demo.v2mysite.com 来测试我们的 HTML 网页。

### 准备工作

要完成本节操作，你需要一个正在运行的 IIS 10.0 实例。你应该已经为通配符主机 ssl.v2mysite.com 和 demo.v2mysite.com 创建、配置并上传了网页。你必须具有管理员权限。

### 操作步骤...

- 1. 登录 Windows Server 2016，然后转到 **服务器管理器**。
- 2. 单击 **工具** 菜单并打开 IIS 管理器。
- 3. 展开 IIS 服务器（**WIN2016IIS**）。你会看到列出的 **应用程序池** 和 **网站**。在站点文件夹中会看到三个网站——**mysite.com**、**v2mysite.com** 和 **v4mysite.com**——以及我们已经创建的两个通配符主机，**ssl.v2mysite.com** 和 **demo.v2mysite.com**。
- 4. 单击通配符主机 **demo.v2mysite.com**，转到 IIS 管理器控制台的右侧。你将看到 **功能视图**。在 **功能视图** 中，我们有 **默认文档** 设置：

  ![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_191_0.png)

- 5. 打开 **默认文档** 设置，检查 **iisstart.htm** 是否可用：

  ![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_192_0.png)

- 6. 接下来，我们将交叉检查 **默认文档** 设置，并验证一切是否已准备好用于测试通配符主机 URL **demo.v2mysite.com** 和 **ssl.v2mysite.com**。在这里，列表顶部有 **iisstart.htm** 条目。这是 **demo.v2mysite.com** 通配符主机网页的文件名，即 **iisstart.htm**。默认文档必须在我们网站的默认页面名称的默认文档设置中定义，并带有扩展名。

- 7. 让我们转到通配符主机 ssl.v2mysite.com 的默认文档设置。我们发现 default.aspx 已经在列表中，并且位于顶部：

  ![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_193_0.png)

### 让你的通配符主机启动并运行

- 8. 关闭 IIS 管理器，打开 Internet Explorer，并输入通配符主机 URL `demo.v2mysite.com`，如下所示：

  ![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_194_0.png)

- 9. 你可以看到通配符主机 **demo.v2mysite.com** 运行正常。

- 10. 现在，在 Internet Explorer 中打开通配符主机 URL 并检查结果：

  ![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_195_0.png)

  通配符主机 ssl.v2mysite.com 也运行正常。你可以使用自己的父域名托管多个通配符主机，并为不同类型的网站使用不同的应用程序池。

### 工作原理...

首先，我们检查了通配符主机 ssl.v2mysite.com 和 demo.v2mysite.com 的默认文档设置。我们测试了使用 Framework 4 版本开发并上传到通配符主机 ssl.v2mysite.com 的 Web 应用程序页面。我们还使用 HTML 网页测试了通配符主机 demo.v2mysite.com。

## 在 Nano Server 上部署 IIS 10.0

在本章中，我们将介绍以下操作：

- 了解 Nano Server 上的 IIS 10.0
- 在 Nano Server 上安装 IIS 10.0
- 管理 Nano Server 上的 IIS 10.0
- 在 Nano Server 上创建 IIS 10.0 网站
- 在 Nano Server 上配置 IIS 10.0 网站
- 上传网站页面
- 测试上传的网页

### 简介

Nano Server 是一种远程管理的服务器操作系统，针对私有云以及本地或远程数据中心进行了优化。Nano Server 类似于 Windows Server 核心模式操作系统。它没有任何 GUI 界面，并且比 Windows Server 的核心模式更小。

Nano Server 没有本地登录功能。它仅支持 64 位应用程序、工具和代理。

与 Windows Server 相比，Nano Server 占用的磁盘空间非常少，安装速度快，需要的更新更少，重启时间也更短。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_198_0.png)

- 3. 在 Hyper-V 管理器下，我们有四台虚拟机：DC，即域控制器；Nano Server 2016；Server 2016，即我们的 IIS 服务器；以及 Windows Server 2016。

- 4. 我们选择了 Nano Server 2016。你可以看到在虚拟机列表的“已分配内存”标题下，服务器有 4000 MB RAM，并且虚拟机状态为“正在运行”。

- 5. Nano Server 2016 是 Nano Server 虚拟机的显示名称，服务器名称是 Nano，其 IP 地址是 172.16.15.63，在上图中已突出显示。这是我的情况，但在你的情况下，IP 可能会不同。这一点需要注意。

- 6. 选择 **Nano Server 2016** 虚拟机。转到 **Hyper-V 管理器** 中的 **操作面板**，你会发现两种属性：**HV1-Host** 属性，可以在其中对 Hyper-V 主机服务器进行更改；以及 **Nano Server 2016**，可以在其中更改 Nano Server 虚拟机的属性。在 Nano Server 2016 的 **操作** 窗格中，单击右侧面板中选定服务器名称下方的 **连接**，你将打开 **Nano Server 2016** 控制台：

  ![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_199_0.png)

- 7. 如屏幕截图所示，你可以看到 Nano Server 的登录屏幕。我们已经将 IIS 10.0 安装到了 Nano Server 2016 上，并且确认了 IP 地址 `172.16.15.63` 已分配给 Nano Server 2016。
- 8. 让我们打开 Nano Server 2016 上的默认 IIS 网页。
- 9. 转到 Internet Explorer，输入 `http://172.16.15.63`（Nano Server IP），然后按 *Enter*。如果你已经在 Nano Server 2016 上安装了 IIS，它将显示 IIS 默认主页，如下所示。

