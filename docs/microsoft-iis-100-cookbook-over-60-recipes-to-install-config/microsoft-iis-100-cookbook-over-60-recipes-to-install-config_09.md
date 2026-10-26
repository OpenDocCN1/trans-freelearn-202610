## 12 保护 IIS 10.0 上的网站

在本章中，我们将介绍以下操作：

- 了解 IIS 10.0 上可用的安全功能
- 配置 IIS 10.0 上的安全设置
- URL 授权和身份验证
- IP 地址和域限制
- 测试 IIS 10.0 上的安全性

### 简介

Web 服务器安全性（IIS 服务器）、网站安全性和应用程序安全性可用于配置 IIS 服务器，以保护自身和网站。

以下是 IIS 服务器中可用的安全选项：

- 计算机密钥
- 身份验证
- 授权规则
- IP 地址和域限制
- ISAPI 和 CGI 限制
- 服务器证书
- 请求筛选
- IIS 管理器权限和 IIS 管理器用户
- NTFS 权限

我们可以为需要安全托管的网站或 Web 应用程序在 IIS 服务器上安装和配置安全组件。

在本章中，我们将从 IIS 10.0 的一些安全信息开始。如果你愿意，可以尝试更多安全选项。这里我们将介绍几个重要的部分。

### 了解 IIS 10.0 上可用的安全功能

在本节中，我们将安装 Windows Server 2016 中可用的安全组件，并浏览 IIS 管理器中已安装的安全功能。我们还将了解 NTFS 权限。

**准备工作**

我们需要一个正在运行的 IIS 10.0 实例。你应该具有管理员权限。没有其他先决条件。

**操作步骤...**

1. 使用具有管理员权限的帐户登录 Windows Server 2016。
2. 单击“开始”菜单，或在搜索窗口中输入“服务器管理器”。你将看到“服务器管理器”窗口。
3. 单击“添加角色和功能”链接，或者你也可以在“管理”菜单中找到相同的选项，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_342_0.png)

5. 如图所示，我们展开 **Web 服务器**，然后展开 **安全性**；在这里，我们发现一些安全选项已经安装（已勾选且呈灰色的复选框）。勾选此处列出的其余可用安全功能，单击 **下一步**，跳过功能向导，单击 **下一步**，你将看到确认窗口，如以下截图所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_344_0.png)

6. 你可以看到我们已经选择了将要安装的安全功能。单击 **安装** 按钮。在下一个截图中，你可以看到它们已成功安装：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_345_0.png)

7. 单击 **关闭**。现在我们必须打开 IIS 管理器。单击“开始”菜单，或在 Windows 2016 Server 的搜索窗口中输入“服务器管理器”。你将看到“服务器管理器”窗口。打开服务器管理器。在服务器管理器中，单击 **工具** 菜单，选择并打开 IIS 管理器，如以下截图所示。

9. 转到 IIS 管理器，展开 IIS 服务器，展开“站点”文件夹，然后点击任意现有站点：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_347_0.png)

10. 转到“操作”窗格（如上一张截图中所突出显示的），然后点击 **编辑权限...**。这是所选网站 mysite.com 的物理目录。将打开 mysite 物理目录属性窗口，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_348_0.png)

11. 在 mysite 属性窗口中点击 **安全** 选项卡。在“安全”选项卡中，你可以添加用户并修改文件夹级别的权限。并非所有人都能访问和更改 mysite 文件夹，只有具有权限的特定用户才能。你可以根据 NTFS 权限的规划来进行设置。

**工作原理...**

在本节中，我们在 Windows Server 2016 上安装了可用的安全组件，并浏览了 IIS 管理器中已安装的安全功能。我们了解了 mysite.com 网站的 mysite 物理文件夹上的 NTFS 权限。

### 配置 IIS 10.0 上的安全设置

在本节中，我们将配置 IIS 管理器权限和 IIS 管理器用户。我们还将配置计算机密钥。

**准备工作**

我们需要一个正在运行的 IIS 10.0 实例。应已安装安全组件。你应该具有管理员权限。

**操作步骤...**

1. 使用具有管理员权限的帐户登录 Windows Server 2016。
2. 从“开始”菜单打开服务器管理器，或使用搜索窗口查找它。
3. 转到 **工具 | IIS 管理器**。点击 IIS 服务器 **WIN2016IIS**，转到 **功能视图**，然后打开 **IIS 管理器用户**，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_349_0.png)

4. 你可以从 **IIS 管理器用户** 中添加新的 IIS 用户（这些用户不同于 Windows 用户）。让我们转到 **IIS 管理器权限**。展开 IIS 服务器 **WIN2016IIS**，展开 **站点** 文件夹，点击 **mysite.com**，转到 **功能视图**，然后打开 **IIS 管理器权限**：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_350_0.png)

5. 转到 **操作** 窗格，然后点击 **允许用户...**。IIS 管理器权限用于允许本地计算机上的用户或用户组管理网站。让我们选择 **mysite.com**。从 **操作** 窗格中点击 **允许用户** 后，你将看到 **允许用户...** 窗口：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_351_0.png)

6. 点击 **选择...** 选项。你将看到 **选择用户或组** 窗口，如以下截图所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_351_1.png)

7. 在这里，我们将搜索并选择 Windows Server 用户。我想选择管理员用户，因此我输入 `Administrator` 并点击 **确定** 按钮。你可以看到管理员用户已被选中：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_352_0.png)

8. 我们已选择 `WIN2016IIS\Administrator`（`<本地服务器名称>\<用户名>`）。点击 **确定**。你可以在 IIS 管理器权限窗格中看到管理员用户对 mysite.com 具有权限：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_352_1.png)

9. 此操作指定只有被允许的用户才能访问你的网站。让我们转到计算机密钥。
10. 转到 WIN2016IIS IIS 服务器并点击 WIN2016IIS。转到功能视图并选择“计算机密钥”，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_353_0.png)





12. **Machine Key** 是我们在 IIS 服务器上应用配置设置时使用的。Machine key 配置设置会对 IIS 服务器级别产生影响。现在，单击 **Validation method** 下拉菜单。你会看到可用的验证方法列表。我们将选择默认选项，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_355_0.png)

你可以使用所需的类型和方法进行加密。

#### How it works...

在本教程中，我们配置并检查了 IIS Manager 权限，并为 IIS Manager 权限添加了 Windows 用户。我们检查了 IIS Manager 用户，并配置了机器密钥。

### URL authorization and authentication

在本教程中，我们将配置 URL 授权规则并启用和禁用网站的身份验证类型。

#### Getting ready

我们需要一个正在运行的 IIS 10.0 实例。应安装安全组件。你应具有管理员权限。

#### How to do it...

- 1. 使用具有管理员权限的账户登录 Windows Server 2016。
- 2. 从开始菜单打开服务器管理器，或使用搜索窗口查找它。
- 3. 在服务器管理器中单击 Tools 菜单；你会找到 IIS Manager。打开它并单击 WIN2016IIS IIS 服务器。进入 Features view 并选择 Authorization Rules：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_356_0.png)

- 4. 进入 **Authorization Rules** 的 **Actions** 面板。你有两个选项：**Add Allow Rule...** 和 **Add Deny Rule...**，如下图所示。单击 **Add Allow Rule...**。

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_357_0.png)

5. 弹出以下 **Add Allow Authorization Rule** 窗口：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_358_0.png)

6. 在这里，我们可以从 **All users**、**All Anonymous users** 和 **Specified roles or user groups** 中选择其一。接下来，你可以选择所需的任何指定用户。我们可以在这里选择允许他们。你可以在其中使用特定的动词，例如 **GET** 和 **POST**。让我们看看如何添加拒绝授权规则。

7. 进入授权规则的 Actions 面板并单击 Deny rule，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_359_0.png)

8. 在这里，在 Add Deny Authorization Rule 中，我们可以选择 All users、All Anonymous users 或 Specified roles or user groups。接下来，你可以选择任何指定的用户。根据我们的需求，我们可以在这里选择拒绝他们访问网站；你可以使用特定的动词，例如 GET 和 POST。让我们看看如何禁用授权规则。

9. 让我们转到身份验证类型；进入 mysite.com 的 Features View，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_360_0.png)

10. 打开 Authentication 选项。你将看到列出的可用的身份验证类型方法：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_361_0.png)

11. 在 Authentication Features View 面板中，我们可以启用和禁用身份验证类型。我们有 Anonymous Authentication、ASP.NET Impersonation、Basic Authentication、Digest Authentication、Forms Authentication 和 Windows Authentication。

#### How it works...

在本教程中，我们配置了 URL 授权允许和拒绝规则。我们还启用和禁用了网站的身份验证类型。

4. 你将看到 **IP Address and Domain Restrictions** 配置窗口，如下图所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_363_0.png)

5. 进入 **Actions** 面板，单击 **Add Allow Entry**，你将看到允许 IP 和域窗口，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_364_0.png)

6. Add Allow Restriction Rule 将帮助我们允许特定的 IP 地址或域名（xyz.com），或带子网掩码或前缀的 IP 地址范围（域名）。添加 IP 172.16.15.60 并单击 OK。你现在已创建允许限制规则。让我们继续创建拒绝规则。

7. 进入 IP Address and Domain Restrictions Actions 面板，单击 Add Deny Entry...，你将看到 Add Deny Restriction Rule 窗口：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_364_1.png)

8. 在这里，我们为 IP 172.16.15.65 创建拒绝规则。我们可以为特定 IP、域名或 IP 地址范围添加拒绝规则。我们现在添加了一个单个 IP：172.16.15.65。单击 OK 按钮。我们现在已创建拒绝规则。

9. 我们需要配置动态 IP 地址限制设置。进入 IP Address and Domain Restrictions Actions 面板。单击 Dynamic IP Restriction Settings。你将看到一个新窗口：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_365_0.png)

10. 在这里，我们根据并发请求的数量设置拒绝 IP 地址规则；例如，使用第一个选项，我们可以允许单个 IP 地址以最多四个同时会话访问网站。超过四个网站访问请求将被拒绝（终止）。使用第二个选项，我们可以要求每个 IP 在指定时间内只能进行特定数量的网站访问请求。我们在这里没有配置这部分，但在你将网站发布到互联网时，你可以这样做以防止某些网络攻击。

#### How it works...

在本教程中，我们查看了 IP 地址和域限制功能。然后我们为 IP 地址 `172.16.15.60` 添加了允许规则，并为 `172.16.15.65` 地址添加了拒绝规则。我们讨论并概述了动态 IP 限制。

### Testing security on IIS 10.0

在本教程中，我们将登录 Windows Server 2016 WIN2016IIS 并检查我们设置的 IP 地址访问和拒绝规则。我们将从 `172.16.15.60` 服务器访问网站 `https://172.16.15.60 (mysite.com)`，并从 `172.16.15.65` 服务器访问网站 `https://172.16.15.60(mysite.com)`。

#### Getting ready

我们需要一个正在运行的 IIS 10.0 实例。应安装安全组件。你应具有管理员权限。

#### How to do it...

- 1. 使用具有管理员权限的账户登录 Windows Server 2016。
- 2. 从开始菜单打开服务器管理器，或使用搜索窗口查找它。

### Securing Your Websites on IIS 10.0

- 3. 在服务器管理器中单击 **Tools** 菜单，你会找到 IIS Manager。打开它并单击 **WIN2016IIS** IIS 服务器。展开 **Sites** 文件夹并单击 **mysite.com**。进入 **mysite.com** 的 **Features View**，选择 **IP Address and Domain Restrictions**，并打开 **IP Address and Domain Restrictions** 设置，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_367_0.png)

- 4. 你可以在图中看到，我们已经为 IP 地址 `172.16.15.60` 创建了 Allow 规则，并为 IP 地址 `172.16.15.65` 创建了 Deny 规则。让我们测试 IP `172.16.15.60`。

5. 我们需要检查 **WIN2016IIS** 服务器的 IP 地址。打开命令提示符并输入 `ipconfig`，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_368_0.png)

6. 你可以在结果中看到，我们有 `172.16.15.60`，即服务器 WIN2016IIS 的 IP 地址。现在，让我们在 Internet Explorer 中打开 `https://172.16.15.60` URL，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_368_1.png)

7. 你现在可以看到我们如何轻松访问服务器 172.16.15.60 上的 URL https://172.16.15.60 (https://mysite.com)。我们为服务器 IP 172.16.15.60 添加了 Allow 规则，以便我们可以访问它。现在我们必须在服务器 IP 172.16.15.65 上测试 URL https://172.16.15.60。

8. 登录到服务器 172.16.15.65，打开命令提示符并输入命令 ipconfig 以检查服务器 IP 地址，如下所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_369_0.png)

9. 现在让我们在服务器 172.16.15.65 上打开 URL https://172.16.15.60 (https://mysite.com)。你将收到拒绝消息，如下图所示：

![](img/1e026bc33e65e0f63d5e9b5081b6d8c8_369_1.png)





#### 如何运作...

在本教程中，我们登录到 Windows Server 2016 WIN2016IIS 并检查服务器上的 IP 地址限制规则以及访问和拒绝规则。我们从服务器 `172.16.15.60` 访问网站 `https://172.16.15.60` (**mysite.com**) 以及从服务器 `172.16.15.65` 访问网站 `https://172.16.15.60` (**mysite.com**).

## 