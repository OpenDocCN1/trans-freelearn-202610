
## 安装 Visual Studio 2005

Visual Studio 2005 可以与 Visual Studio.NET 2003 并行安装。完成升级后，如果我们只想使用 SP2 和 ASP.NET 2.0 开发 MCMS 应用程序，可以移除 Visual Studio.NET 2003。

- 1. 插入 Visual Studio 2005 DVD，并在启动画面上点击 **安装 Visual Studio 2005**。
- 2. 在 **欢迎使用 Microsoft Visual Studio 2005 安装向导** 页面，点击 **下一步**。
- 3. 在 **开始页**，选中 **我接受许可协议条款** 复选框，输入您的 **产品密钥** 和 **姓名**，然后点击 **下一步**。
- 4. 在 **选项页**，选择 **自定义** 单选按钮，输入所需的 **产品安装路径**，然后点击 **下一步**。

![](img/18a0aa8d36427a4c069b30410f998b8e_20_0.png)

- 5. 在第二个选项页中，在“语言工具”部分选中 Visual C# 和 Visual Web Developer 复选框，并在“.NET Framework SDK”部分选中 Tools 复选框。点击 安装。

![](img/18a0aa8d36427a4c069b30410f998b8e_21_0.png)

您可以随意安装可能想要使用的任何附加功能。以上选择足以跟随本书中的示例。

- 6. 等待（或休息一下喝杯咖啡）Visual Studio 2005 安装完成。当 **完成页** 出现时，点击 **完成**。

![](img/18a0aa8d36427a4c069b30410f998b8e_22_0.png)

- 7. 在 **Visual Studio 2005 安装** 对话框中，如有需要，您可以选择安装产品文档（MSDN 库）。
- 8. 在 **Visual Studio 2005 安装** 对话框中，点击 **检查 Visual Studio 服务发布** 以安装可能可用的任何更新。
- 9. 点击 退出。

## 安装 MCMS 2002 Service Pack 2

接下来，我们将安装 MCMS Service Pack 2。

- 1. 从 **开始菜单**，点击 **运行...**
- 2. 在 **打开** 文本框中输入 **IISRESET /STOP** 并点击 **确定**。等待 IIS 服务停止。
- 3. 双击 SP2 安装包。
- 4. 在 **欢迎使用 Microsoft 内容管理服务器 2002 SP2 安装向导** 页面，点击 **下一步**。

![](img/18a0aa8d36427a4c069b30410f998b8e_23_0.png)

### 安装

- 5. 选择 **我接受此许可协议条款** 单选按钮，然后点击 **下一步**。

![](img/18a0aa8d36427a4c069b30410f998b8e_24_0.png)

- 6. 在 **准备开始安装** 页面，点击 **下一步**。

![](img/18a0aa8d36427a4c069b30410f998b8e_24_1.png)

等待 Service Pack 2 安装完成。

- 7. 安装过程中可能会提示插入 MCMS 2002 SP1a CD-ROM。
- 8. 在 **安装向导已完成** 页面，点击 **完成**。
- 9. 如果提示，点击对话框上的 **是** 重新启动计算机，以完成安装。
- 10. 否则，从 **开始菜单** 点击 **运行...**
- 11. 在 **打开** 文本框中输入 **IISRESET /START** 并点击 **确定** 以重新启动 IIS 服务。

> 在安装 SP2 之前停止 IIS 可避免安装过程中替换锁定文件时出现潜在问题，并可防止需要重新启动。

## 配置开发环境

在继续之前，需要执行一些额外步骤来配置开发环境。我们将：

- 配置打开 **站点管理器** 的快捷方式以绕过 **连接到** 对话框。
- 在 Visual Studio 中安装 MCMS 网站和项目模板。

## 站点管理器快捷方式

在 SP2 安装期间，**站点管理器** 开始菜单快捷方式将被覆盖。要配置 **站点管理器** 以绕过 **连接到** 对话框，请执行以下步骤：

- 1. 选择 **开始 | 所有程序 | Microsoft Content Management Server**。
- 2. 右键点击 **站点管理器** 快捷方式并点击 **属性**。
- 3. 在 **目标** 文本框中，将 "C:\Program Files\Microsoft Content Management Server\Client\NRClient.exe" http://NR/System/ClientUI/login.asp 替换为 "C:\Program Files\Microsoft Content Management Server\Client\NRClient.exe" http://localhost/NR/System/ClientUI/login.asp。
- 4. 点击 **确定**。

## Visual Studio 模板

MCMS Service Pack 2 的安装会自动在 Visual Studio 2005 中注册 MCMS 开发工具，如 MCMS 模板资源管理器。然而，在使用 Visual Studio 创建 MCMS 应用程序之前，我们需要使网站和项目模板可用。

- 1. 选择 开始 | 所有程序 | Microsoft Visual Studio 2005 | Visual Studio 工具 | Visual Studio 2005 命令提示。
- 2. 执行以下命令，将 MCMS_INSTALL_PATH 替换为 MCMS 的安装位置（通常为 c:\Program Files\Microsoft Content Management Server），将 PATH_TO_MY_DOCUMENTS_FOLDER 替换为“我的文档”文件夹的位置：
  xcopy "MCMS_INSTALL_PATH\DevTools\NewProjectWizards80\Visual Web Developer" "PATH_TO_MY_DOCUMENTS_FOLDER\Visual Studio 2005\Templates\ProjectTemplates\Visual Web Developer" /E
  xcopy "MCMS_INSTALL_PATH\DevTools\NewItemWizards80\Visual Web Developer" "PATH_TO_MY_DOCUMENTS_FOLDER\Visual Studio 2005\Templates\ItemTemplates\Visual Web Developer" /E
- 3. 执行以下命令以将模板注册到 Visual Studio 2005：
  devenv /setup
- 4. 关闭命令提示符。

这样就完成了升级到 SP2 的步骤，我们的环境现已准备好进行开发！我们可以通过在 SCA 中查看版本号、通过站点管理器连接或使用 Web 作者来测试我们的安装。当然，任何现有的 MCMS Web 应用程序此时仍将由 .NET Framework v1.1 主机。

![](img/18a0aa8d36427a4c069b30410f998b8e_26_0.png)

## 为 SQL Server 2005 准备 MCMS 数据库

在将 SQL Server 安装升级到 SQL Server 2005 之前，我们需要准备 MCMS 数据库，使其与 SQL Server 2005 兼容。

- 1. 从 Microsoft 请求以下 MCMS 热修复程序：http://support.microsoft.com/?kbid=913401。
- 2. 运行热修复程序可执行文件，将文件提取到本地文件夹，例如 c:\913401。
- 3. 将两个文件 (_dca.ini 和 _sp1aTop2upgrade.sql) 复制到 MCMS SQL 安装文件夹（通常为 c:\Program Files\Microsoft Content Management Server\Server\Setup Files\SQL Install）。覆盖现有文件。
- 4. 删除临时文件夹。

在此阶段，您也可以选择升级 SQL Server 或继续阅读第 2 章。

此外，使用 Internet Information Services 快捷方式配置 IIS 以使用 ASP.NET 2.0 是不必要的，因为 Visual Studio 2005 会自动在创建的每个 MCMS 网站应用程序上设置此选项。
但是，如果您在 Windows Server 2003 上安装，则必须配置虚拟网站根目录和 MCMS 虚拟目录以使用 ASP.NET 2.0，因为在同一个应用程序池中无法使用两个版本的 ASP.NET。

HtmlPlaceholderControl 的一部分 ActiveX 控件已随 SP2 更新。因此，首次切换到编辑模式时，您将被提示安装此控件。
如果您已使用 regsvr32 或组策略预先安装了控件，如 http://download.microsoft.com/download/4/2/5/4250f79a-c3a1-4003-9272-2404e92bb76a/MCMS+2002-+(complete)+FAQ.htm#51C0CE4B-FC57-45C4-BAAE-12C09421B57B 中所述，您也可能会被提示，并且需要更新控件的分发。

- 5. 选择 开始 | Microsoft Content Management Server | 数据配置应用程序。
- 6. 在启动画面上，点击 下一步。
- 7. 在“停止服务？”对话框中，点击 是。
- 8. 在“选择 MCMS 数据库”页面，点击 下一步。
- 9. 在“需要升级”对话框中，点击 是。
- 10. 在“升级数据库”页面，点击 下一步。
- 11. 在“添加管理员”对话框中，点击 否。
- 12. 在“数据库配置应用程序”页面，取消选中“立即启动 SCA”复选框，然后点击 完成。

## 升级 SQL Server 2005

在继续之前，请确保您拥有现有 SQL Server 服务帐户的详细信息，您将在升级过程中需要它。

- 1. 插入 SQL Server 2005 CD-ROM 或 DVD。
- 2. 在启动画面上，在“安装”部分点击“服务器组件、工具、联机丛书和示例”链接。
- 3. 在“最终用户许可协议”对话框中，选中“我接受许可条款和条件”复选框，然后点击 下一步。
- 4. 在“安装先决条件”对话框中，点击 安装 并等待先决条件安装完成。

