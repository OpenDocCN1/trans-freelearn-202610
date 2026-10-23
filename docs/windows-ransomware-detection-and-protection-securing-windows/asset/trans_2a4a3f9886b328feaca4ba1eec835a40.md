

那么，它们是从哪里来的呢？攻击者是如何找到他们试图登录的用户账户的？

在许多情况下，攻击者拥有不同的抓取器和脚本，会爬取网站以收集他们能找到的所有电子邮件地址。这也可能包括从现有数据泄露中收集到的电子邮件地址。

一个了解凭据是从哪里被窃取的好方法是，检查受影响的电子邮件地址是否在 https://haveibeenpwned.com 上。以下截图显示了电子邮件地址未被泄露的结果：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_37_0.png)

图 1.8 – 显示未找到用户信息的资料

然而，如果信息在服务可访问的某个数据泄露中被找到，则会出现以下结果：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_38_0.png)

图 1.9 – 显示用户信息在泄露中被找到的资料

在某些情况下，服务不会显示密码已被收集，而是仅显示收集了电子邮件信息。这很可能是因为数据来源在 haveibeenpwned.com 上不可用，或者攻击者有机器人会抓取或爬取网站以获取电子邮件地址等信息。

甚至网上还有免费服务可以用来从 URL 中提取电子邮件，例如 Specrom Analytics（https://www.specrom.com/extract-emails-from-url-free-tool/）或使用一个简单的 Python 脚本也能做到同样的事情。然后，我们可以比较我们收到多次身份验证尝试的用户账户是否可以从公共网站上轻松搜索到。

减少针对用户身份的垃圾邮件和暴力攻击的一种方法是限制可用的公共信息量。

例如，如果你的企业网站发布在 **Web Application Firewall (WAF)** 后面，你就可以根据用户代理来阻止流量。

用户代理是一种身份形式，软件（浏览器）借此向 Web 服务器标识自身。如今大多数常用浏览器都使用用户代理，例如，Mozilla/5.0 (Windows NT 10.0; Win64; x64), AppleWebKit/537.36 (KHTML, like Gecko), Chrome/97.0.4692.99, Safari/537.36, 和 Edge/97.0.1072.76。

## 漏洞是如何被用于攻击的？

那么，在我们更仔细地研究了攻击者如何通过抓取、钓鱼或凭据填充等方式收集我们终端用户的信息之后，我们将更仔细地研究一些不同勒索软件运营商在攻击中已知使用的漏洞。在本节后面，我将介绍你如何监控针对你服务的漏洞。

我们将要介绍的许多漏洞要么被用于初始入侵，要么用于获得对受损机器的提升权限，最后是横向移动。原因是为了让你了解入侵一台机器或服务是多么容易，以及从一个高严重性漏洞被公开到勒索软件运营商开始利用它的时间有多短。

所以，我们将重点关注以下漏洞：

- **PrintNightmare:** CVE-2021-34527
- **Zerologon:** CVE-2020-1472
- **ProxyShell:** 它由三个不同的漏洞组成，作为单一攻击链的一部分使用：CVE-2021-34473、CVE-2021-34523 和 CVE-2021-31207
- **Citrix NetScaler ADC:** CVE-2019-19781

## PrintNightmare

让我们从 PrintNightmare 开始，这是一个于 2021 年 7 月发布的漏洞。利用此漏洞，攻击者可以在远程系统和本地系统上以系统权限运行任意代码，只要 Print Spooler 服务已启用。因此，理论上，你可以利用此漏洞使域控制器运行任意代码，只要 Print Spooler 服务正在运行。这是因为 Point and Print 功能中的功能，该功能允许用户自动从打印服务器直接下载打印机配置信息到客户端。

所有 Microsoft **Common Vulnerabilities and Exposures (CVEs)** 都在 MSRC 上发布，并附有专门的支持文章，说明受影响的系统以及在缓解措施和其他对策方面的建议，如 PrintNightmare 的情况在此处所示：`https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-34527`。

关于 PrintNightmare，InfoSec 社区制作了多个脚本，可以很容易地使用；例如，这里有一个简单的 PowerShell 有效载荷利用了该漏洞，无需管理员访问权限，并附带一个预定义的 DDL 文件，可在机器上创建本地管理员账户：`https://github.com/calebstewart/CVE-2021-1675`。

**Mimikatz** 的创建者 Benjamin Delpy 也创建了一个概念验证，他设置了一个公共 *print server*，你可以从终端点连接到该公共服务器，然后该服务器会自动创建一个以本地系统上下文运行的 CMD 窗格。

微软花了数周时间才提供补丁和修复建议。在 8 月中旬，仅一个月后，就已有新闻报道称勒索软件运营商正在利用 PrintNightmare 漏洞入侵组织。

微软在漏洞被发现时提供了建议，即在提供安全修复之前禁用 Print Spooler 服务。这也让许多管理员意识到 Print Spooler 服务不需要在非面向终端用户的服务器上运行，例如 Citrix/RDS 服务器。

> **重要说明**
> 一个普遍的最佳实践是确保服务上仅运行所需的服务——例如，Print Spooler 服务*不应*在域控制器上运行。此微软指导文档提供了不同服务的列表以及每个服务的建议：https://docs.microsoft.com/en-us/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server。

## Zerologon

接下来是 Zerologon，另一个高严重性 CVE，利用了 Active Directory 中 Netlogon 进程的漏洞，允许攻击者冒充任何计算机，包括域控制器。

要利用此漏洞，攻击需要能够与域控制器通信，例如拥有加入 Active Directory 域的 Windows 客户端。

然后，攻击者会伪造基础设施中的另一个域控制器，并使用 MS-NRPC 协议更改 Active Directory 中机器账户的密码，这就像发送一个带有新密码的简单 TCP 帧一样简单：

![](img/7b49a27a3c2adc52d097f2e77f3bf394_42_0.png)

图 1.11 – Zerologon 攻击流程

一旦新密码被接受，攻击者就可以使用该新账户以 Active Directory 域控制器上下文启动新进程，然后用于入侵剩余的基础设施。Zerologon 已在许多勒索软件攻击中被用于通过横向移动入侵 Active Directory 并获得对域控制器的访问权限。

此漏洞在微软 2020 年 8 月的补丁中得到修复。2020 年 9 月，发现该漏洞的 Secura 安全研究人员发布了他们的研究成果，一周之内就已经发布了不同的概念验证，说明如何利用该漏洞。你可以在此处找到有关该漏洞的初始白皮书的链接：https://www.secura.com/uploads/whitepapers/Zerologon.pdf。

在随后的几个月里，许多组织都遭到 Ruyk 的攻击，他们使用了 Zerologon 漏洞。平均而言，大多数安全研究人员表示，从供应商发布补丁到平均组织安装补丁，通常需要 60 到 150 天（约 5 个月）。

## ProxyShell

然后是 ProxyShell，这是一个由三个不同 CVE 组成的漏洞，作为单一攻击链的一部分使用，影响了 Microsoft Exchange 2013/2016/2019，允许攻击者进行预认证 RCE。

主要漏洞位于 Exchange 中的 Client Access Service (CAS) 服务器组件中，该组件默认暴露于互联网，以允许终端用户从外部访问电子邮件服务。

简而言之，ProxyShell 漏洞利用执行以下操作：

- 发送 Autodiscover 请求，使用已知的电子邮件地址泄露用户的 LegacyDN 信息。
- 向 CAS 服务器发送 MAPI 请求，使用 LegacyDN 泄露用户的 SID。
- 使用 SID 和电子邮件地址从 CAS 服务构建有效的身份验证令牌。
- 使用身份验证令牌对 PowerShell 端点进行身份验证并执行代码。示例代码可在 GitHub 上找到，地址为 https://github.com/horizon3ai/proxyshell。

Horizon3.ai 发布了一个 Python 脚本来展示利用此漏洞是多么容易（https://github.com/horizon3ai/proxyshell），你只需运行脚本并指向 Exchange CAS 服务器即可。

所有这些漏洞都在 2021 年 4 月得到修补，但信息在 2021 年 6 月才公开发布。

2022 年 2 月发现，相当数量的组织未能更新其 Exchange 服务，尽管这是紧急需要的。更准确地说，4.3% 的所有可公开访问的 Microsoft Exchange 服务仍未修补 ProxyShell 漏洞。在那些确实应用了 ProxyShell 补丁的组织中，16% 的组织未安装 2021 年 7 月以后发布的后续补丁，这使它们仍然容易受到攻击。因此，许多组织在六个月过去后仍未完全消除该漏洞。如以下 2022 年 2 月的 Shodan 截图所示，仍有相当数量的面向公众的 Exchange 服务器存在该漏洞：

