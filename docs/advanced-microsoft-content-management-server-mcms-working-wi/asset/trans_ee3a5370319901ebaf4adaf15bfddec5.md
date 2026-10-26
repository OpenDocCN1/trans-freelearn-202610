
## 在 MCMS 和 SharePoint 之间发布内容

当 SharePoint 和 MCMS 位于同一台物理服务器上时，您可以选择在独立的虚拟服务器中运行它们，也可以在同一虚拟服务器中运行它们。

> 在 Microsoft Internet Information Services 中，虚拟服务器也称为网站。

如您所见，对于这两个产品的部署方式，您有多种选择：

- 1. 将 MCMS 和 SharePoint 部署在同一台物理服务器和同一个虚拟服务器上
- 2. 将 MCMS 和 SharePoint 部署在同一台物理服务器但不同的虚拟服务器上
- 3. 将 MCMS 和 SharePoint 部署在不同的物理服务器上，因而使用不同的虚拟服务器

由于选项众多，您需要首先决定的事项之一是：这两个产品中哪一个将作为解决方案的主要入口点。如果 SharePoint 和 MCMS 位于不同的物理服务器或虚拟服务器上，则两者均可充当主要入口点。但是，如果 MCMS 和 SharePoint Portal Server 配置为在同一虚拟服务器上运行，并且用户访问站点根目录（例如 http://www.TropicalGreen.net），则 SharePoint 将是主要入口点。

> 在同一虚拟目录中运行 MCMS 和 SPS 需要在 SharePoint 管理中进行一些手动配置，以排除某些 URL 路径不被 SharePoint ISAPI 筛选器管理。这允许请求由 MCMS ISAPI 筛选器处理。遗憾的是，SharePoint 不允许排除门户的根目录。这意味着任何发送到虚拟服务器根目录的 HTTP 请求（如 http://www.TropicalGreen.net/default.aspx）都将由 SharePoint ISAPI 筛选器处理。只有根目录以下的子目录（例如 http://www.TropicalGreen.net/TropicalGreen/default.aspx）可以排除在 SharePoint 之外，从而由 MCMS 管理。
> 
> 有关如何在同一虚拟服务器上设置 MCMS 和 SharePoint 的更多信息，请参阅附录 A《_在同一虚拟服务器上设置 MCMS 和 SPS_》。

本章将介绍可覆盖两种情况的解决方案：MCMS 和 SharePoint 位于同一虚拟服务器上，以及位于不同服务器（虚拟或物理）上。如果您选择在不同的物理服务器上运行这两个产品，则需要实现一个自定义解决方案来在计算机之间发布内容。最常见的方法是使用 Web 服务。

> SharePoint 附带一长串 Web 服务，可公开其大部分内容。这些 Web 服务位于站点根目录下的 `_vti_bin` Web 应用程序中，该应用程序为每个 SharePoint 门户和 Windows SharePoint Services 站点自动创建。有关为 MCMS 创建 Web 服务的更多信息，请参阅第 11 章《_与 Web 服务结合的 InfoPath_》，该章节涵盖了 MCMS 的 Web 服务。在该章节中创建的 Web 服务由 Microsoft Office InfoPath 使用，但也很容易被自定义代码使用。

- Tropical Green 使用 MCMS 和 SharePoint 来提供完整的 Intranet 解决方案。SharePoint 主要用于托管较小的团队站点，并为整个 Intranet 提供集中式搜索引擎，而 MCMS 则在 Intranet 中内容密集的部分被使用。

在上述场景中，您会注意到一个共同点：解决方案从一个环境（例如 SharePoint 或 MCMS）提取内容，以在另一个环境中显示。请注意，内容不会被复制，并且是只读的（SharePointDocumentLibrary 占位符除外，MCMS 模板使用它来显示 SharePoint 文档库中的内容）；内容只是被拉取进来，从而减少了数据重复。限制数据重复可以减少错误以及管理、更新两个产品中信息的成本。然而，解决方案的整体性能将会下降，因为如果它拥有数据的副本而不是从另一个产品实时拉取数据，性能会更好。上述三个场景虽然相当简单，但应该能让您了解在 SharePoint 和 MCMS 之间共享内容的一些可能性。

## 用于 SharePoint 技术的 MCMS 连接器

2004 年 2 月，Microsoft 发布了该连接器，其中包含一组 SharePoint Web 部件、MCMS 控件和占位符，以及一些命令行实用程序，开发人员可以利用这些工具在 MCMS 中发挥 SharePoint 的优势，反之亦然。该连接器完全免费、由 Microsoft 提供支持并有完整文档，且包含一个示例项目（带数据），演示了该包中包含的功能。

> 有关从何处获取连接器、安装说明以及其所包含内容的详细信息，请参阅附录 B“用于 SharePoint 技术的 MCMS 连接器”。

我们将要讨论的第一个场景涉及在 SharePoint 中呈现 MCMS 内容。连接器提供了三种使用 Web 部件在 SharePoint 站点中查看 MCMS 内发布项的方法。

- MCMS 页面列表 Web 部件：显示所选 MCMS 频道中的发布项列表。该列表的呈现可通过不同视图进行修改。本章稍后我们将为此 Web 部件创建一个自定义视图。
- MCMS 生产中的页面 Web 部件：显示当前用户正在处理的发布项列表，类似于 MCMS 中的生产经理。
- MCMS 等待审批的页面 Web 部件：显示可由当前用户批准的发布项列表，类似于 MCMS 中的审批助手。

安装连接器后，这三个 Web 部件会自动安装到所有门户站点上。如果您要在安装连接器之后创建门户，则需要手动安装这三个 Web 部件，它们都包含在一个单独的 CAB 文件中。遗憾的是，连接器附带的文档并未说明如何安装 Web 部件。SharePoint 要求您使用命令行实用程序来安装 Web 部件并执行其他管理功能。此实用程序 stsadm.exe 位于目录 C:\Program Files\Common Files\Microsoft Shared\web server extensions\60\BIN\ 中。

## 安装 MCMS 连接器 Web 部件

- 1. 单击“开始”|“运行”（或按 Windows + R），在对话框中输入 cmd 以打开命令行窗口。

- 2. 输入 cd c:\Program Files\Common Files\Microsoft Shared\web server extensions\60\BIN\，将目录更改为 STSADM.EXE 实用程序所在的位置。

- 3. 在安装之前，我们先查看一下我们站点上所有门户中已安装了哪些 Web 部件。我们使用 enumwppacks 选项来执行此操作，该选项将显示每个虚拟服务器上安装的所有 Web 部件的列表。在提示符下输入以下命令：STSADM.exe -o enumwppacks。如果您尚未向门户中安装任何内容，您应该会看到类似下面的信息：

![](img/442e26bf28f2df81fbf8dff3478f88e3_177_0.png)

- 4. 现在，让我们继续将连接器 Web 部件安装到我们服务器上所有已扩展 SharePoint 的虚拟服务器中。您需要找到包含连接器所附带的三个 Web 部件的 CAB 文件。如果您将连接器安装到了默认安装目录，您会在 c:\Program Files\MCMS Connector for SharePoint Technologies\WSS\Bin\CmsWebParts.cab 找到 CAB 文件。在命令控制台中输入以下内容：STSADM.exe -o addwppack -filename "C:\Program Files\MCMS Connector for SharePoint Technologies\WSS\Bin\CmsWebParts.cab"。您应该会看到如下响应：

![](img/442e26bf28f2df81fbf8dff3478f88e3_177_1.png)

- 5. 看起来一切安装成功。让我们再次检查，看看 enumwppacks 选项现在报告了什么（同时确认 Web 部件已安装）。输入 stsadm.exe -o enumwppacks。您应该会看到以下内容：

