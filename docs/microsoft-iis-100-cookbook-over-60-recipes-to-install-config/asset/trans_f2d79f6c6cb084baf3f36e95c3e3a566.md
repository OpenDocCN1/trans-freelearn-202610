

### 准备工作

要完成本教程，你需要一台运行中的 IIS 10.0 服务器和一个管理员账户，用于在 IIS 10.0 中进行更改。

### 操作步骤...

- 1. 转到 WIN2016IIS，登录 IIS 10.0 服务器。打开服务器管理器，进入“工具”菜单。
- 2. 打开 IIS 管理器，展开 WIN2016IIS，你会看到“应用程序池”和“网站”。单击“网站”，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_302_0.png)

- 3. 转到“**操作**”窗格，单击“**添加网站**”。你会看到“**添加网站**”窗口：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_303_0.png)

- 4. 在此处输入网站名称 `mysite.com`，并选择“**物理路径**”。在绑定信息中，我们选择了 `https`（SSL 无法在 `http` 端口上运行，必须使用 `https`）。输入网站的 IP 地址，并输入 HTTPS 默认端口 `443`。选择我们之前创建的 `mysite.com` 证书。单击“**确定**”。

## 使用 PowerShell 命令创建 SSL 证书

在本教程中，我们将在 WIN2016IIS 上打开 IIS 管理器。我们将打开 PowerShell 并创建一个名为 v3mysite.com 的 SSL 证书。我们还将通过 IIS 管理器验证该证书是否已创建成功。

### 准备工作

要完成本教程，我们将为 v3mysite.com 创建一个 SSL 证书。我们将使用 PowerShell 创建自签名证书。你需要一台运行中的 IIS 10.0 服务器和一个管理员账户，以便在 IIS 10.0 中进行更改。

### 操作步骤...

- 1. 登录 Windows Server 2016。IP 为 172.16.15.60，服务器名称为 WIN2016IIS。
- 2. 按 Windows + R 键，会弹出“运行”窗口。使用管理员权限打开 PowerShell ISE，在运行命令窗口中输入 `RunAs /user:Administrator PowerShell_ISE.exe`，然后按“确定”。
- 3. 此时系统会要求输入服务器 WIN2016IIS 的管理员密码。输入管理员密码并按回车。PowerShell 窗口将打开。现在你需要输入 `New-SelfSignedCertificate -DnsName v3mysite.com -CertStoreLocation cert:\LocalMachine\My`，如下所示：

- 4. 你可以在 PowerShell 窗口中看到证书已创建。这是一个自签名证书。
- 5. 让我们打开 IIS 管理器。单击 WIN2016IIS 服务器，你会看到“服务器证书”。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_306_0.png)

## 测试 SSL 网站

在本教程中，我们将在 WIN2016IIS 上打开 Internet Explorer 并打开 mysite.com。我们还将验证我们在 mysite.com 上创建并配置的 SSL 证书。

### 准备工作

要完成本教程，你需要一台运行中的 IIS 10.0 服务器、已配置 SSL 的网站 mysite.com，以及一个管理员账户，以便在 IIS 10.0 中进行更改。

### 操作步骤...

- 1. 转到 WIN2016IIS，登录 IIS 10.0 服务器 WIN2016IIS。打开服务器管理器，进入“工具”菜单。
- 2. 打开 IIS 管理器，展开 WIN2016IIS。你会看到“应用程序池”和“网站”。展开“网站”，你会看到列出的网站 mysite.com。
- 3. 右键单击 mysite.com，单击“资源管理器”。你会看到 mysite.com 网站的物理目录，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_308_0.png)

- 4. 我们已将 `iisstart.htm` 和 `iisstart.png` 文件粘贴到 `mysite` 物理目录中。现在你需要为 `mysite.com` 添加主机条目，以便我们可以通过名称访问它。
- 5. 转到 **WIN2016IIS** 服务器的 `C:\Windows\System32\drivers\etc` 位置。你会看到 hosts 文件。用记事本将其打开，并为 `172.16.15.60` 和网站名称 `mysite.com` 添加一条记录，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_309_0.png)

- 6. 保存 hosts 文件并关闭。
- 7. 打开 Internet Explorer 并输入 `https://mysite.com`，你会收到证书错误警告。忽略该警告；出现警告是因为我们只有本地服务器上的自签名证书。如果我们安装了已购买的证书，则不会显示该警告消息，如图所示。

- 8. 单击“继续浏览此网站”，你会被重定向到 mysite.com 主页，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_310_0.png)

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_310_1.png)

- 9. 你可以看到我们正在使用 https 协议和证书访问该网站（此处标记为红色，因为它是本地服务器证书，对外部世界无效——仅用于演示）。单击该证书，你会看到证书属性：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_311_0.png)

- 10. 你可以在这里看到证书详细信息：“颁发给”、“颁发者”、“有效期从”和“有效期至”。单击“确定”。

### 工作原理...

在本教程中，我们在服务器 WIN2016IIS 上打开了 Internet Explorer，并将 `iisstart.htm` 和 `iisstart.png` 文件复制到 mysite.com 的物理目录。我们创建了一个 hosts 文件条目，并打开了 `https://mysite.com`。我们还验证了在 mysite.com 上创建并配置的 SSL 证书。

## 将 IIS 10.0 扩展到 FTP

在本章中，我们将介绍以下教程：

- 了解 FTP
- 在 IIS 10.0 上安装 FTP
- 创建、保护和配置 FTP 站点
- 创建 FTP 用户并管理用户权限
- 测试我们的 FTP 服务器

### 简介

**文件传输协议（FTP）**用于通过网络和互联网在两台计算机之间传输文件。

FTP 是一种标准网络协议，用于在计算机网络中采用客户端-服务器模型将计算机文件从服务器传输到客户端。FTP 的默认网络端口号是 20 和 21。

FTPS，也称为 FTPES、FTP-SSL、S-FTP 和 FTP Secure，是常用 FTP 的扩展，增加了对**传输层安全性（TLS）**和**安全套接层（SSL）**加密协议的支持。

### 了解 FTP

在本教程中，我们将打开已安装在 Windows 10 PC 上的 FileZilla FTP 客户端软件。我们还将打开托管在远程服务器上的 FTP 站点。我们将了解如何上传和下载文件、文件夹、媒体文件和程序文件。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_313_0.png)

FileZilla 是一款可免费在线下载的第三方应用程序。有两种类型的 FileZilla 程序可用：FileZilla Client 和 FileZilla Server。

FileZilla 专为 FTP 服务器连接设计，用于文件下载和上传。

在本书中，示例使用 FileZilla Client 应用程序进行演示，该应用程序可在 https://filezilla-project.org/ 获取。

### 准备工作

要完成本教程，你需要一个运行中的 FTP 站点。你还应该具有该 FTP 站点的管理权限、FileZilla FTP 客户端软件（或任何 FTP 客户端软件）以及互联网连接。

### 操作步骤...

- 1. 我们已在 Windows 10 PC 上安装了 FileZilla FTP 客户端软件。在桌面上找到 FileZilla 客户端快捷方式并打开它：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_314_0.png)

- 2. 你可以看到顶部有“快速连接”选项。你只需在“主机”字段中输入 FTP 服务器名称或 IP，并输入 FTP 用户名和密码。如果你的 FTP 服务器使用自定义端口，你可以在“端口”选项中设置它。让我们连接到一个远程 FTP 服务器。

- 3. 在客户端上转到“文件”菜单，单击“站点管理器”：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_315_0.png)

- 4. 如你所见，我们已经添加了几个站点。我们将选择 `ftp.learningpoint.co.in`，并将协议设置为 FTP 或 SFTP。然后，单击“连接”按钮。你将连接到远程 FTP 站点，如下所示：

### 在 IIS 10.0 上安装 FTP

在本教程中，我们将在 Windows Server 2016 上安装 FTP 服务器。我们将打开 IIS 管理器，检查 FTP 服务器是否已安装。

### 准备工作

要完成本教程，你需要一个安装了 IIS 10.0 的正在运行的 Windows Server 2016 实例。你还应该具有管理权限。

### 操作步骤...

- 1. 使用管理员账户登录 Windows Server 2016。
- 2. 单击“开始”菜单或搜索“服务器管理器”并打开它。
- 3. 你必须单击“添加角色和功能”链接，或者你也可以在顶部的“管理”菜单中找到相同的选项：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_317_0.png)

