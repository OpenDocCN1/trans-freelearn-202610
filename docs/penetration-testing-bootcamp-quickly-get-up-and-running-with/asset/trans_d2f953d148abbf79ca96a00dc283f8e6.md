
## 跨站脚本攻击

跨站脚本，或简称为 XSS，是一种 Web 应用程序漏洞，用户可以在其中注入将在客户端运行的脚本。这些脚本通常具有恶意性质，并在互联网上随处可见。它们通常发生在输入字段未对输入进行验证或正确编码的地方。当这种情况发生时，恶意用户可以向字段中输入脚本，并且在表单提交时，脚本将被运行。

通常存在两种 XSS 攻击分类，即存储型 XSS 攻击和反射型 XSS 攻击，我将在接下来的两个部分中进一步讨论这两种攻击。

### 反射型 XSS 攻击

反射型 XSS 攻击发生在脚本被反射自 Web 服务器以便运行时。浏览器允许其运行，因为它认为该脚本来自受信任的服务器。它们通常通过诸如错误消息或搜索结果之类的内容从 Web 服务器反射出来。但从技术上讲，只要部分输入发送到 Web 服务器，它们就可以通过任何响应被反射回来。反射型 XSS 攻击，或非持久型 XSS 攻击，往往是最常见的 XSS 攻击。

为了测试反射型 XSS 攻击，我将尝试向网站中的姓名字段插入一些代码，以便在按下提交按钮时，如果 XSS 攻击成功，代码将被运行。

首先，我将浏览到该站点并验证页面是否正常工作，然后查看 URL：

![](img/095df2cd1930baecbcecaa51ee3417aa_185_0.png)

提交后，我可以看到它输出了姓名，并且也在 URL 中将其放在 name 变量之后：

![](img/095df2cd1930baecbcecaa51ee3417aa_185_1.png)

让我看看是否可以创建一个警告弹出窗口。为此，我将以下内容输入到输入字段中：

```html
<script>alert("Your site is not protecting against XSS! Please fix ASAP!!")</script>
```

当我按下提交按钮时，我可以看到 URL 中包含我的脚本作为变量传递。我也可以看到 XSS 攻击成功了，因为我可以看到带有我指定的消息的警告弹出窗口：

![](img/095df2cd1930baecbcecaa51ee3417aa_186_0.png)

### 存储型 XSS 攻击

另一种跨站脚本攻击类型是存储型 XSS 攻击。对于这些攻击，脚本被注入到存储在目标服务器上的字段中。这些包括访客留言簿、消息论坛、访客日志或数据库等项目。受害者随后会检索信息以及脚本，然后恶意脚本将被运行。存储型 XSS，或持久型 XSS，通常并不常见，但也往往更具破坏性。

为了尝试存储型 XSS 攻击，我将尝试查看表单的工作方式。我将填写表单并提交以验证一切正常工作：

![](img/095df2cd1930baecbcecaa51ee3417aa_186_1.png)

我可以看到消息已正确提交，因为我的消息现在作为有效条目显示在屏幕上：

![](img/095df2cd1930baecbcecaa51ee3417aa_187_0.png)

现在，让我看看是否可以将脚本插入表单以执行存储型 XSS 攻击。像之前的示例一样，我将尝试打印出带有消息的警告框。我将输入以下内容：

```
<script>alert("Bad Code")</script>
```

一旦我点击“签名留言簿”，我可以看到弹出窗口显示了我的消息。这验证了该特定 Web 应用程序容易受到存储型跨站脚本攻击：

![](img/095df2cd1930baecbcecaa51ee3417aa_187_1.png)

以下是带有我选择的文本的警告框：

![](img/095df2cd1930baecbcecaa51ee3417aa_187_2.png)

## 使用 OWASP ZAP 查找会话问题

OWASP ZAP 是一款出色的多用途工具，直接来自 OWASP，因此由具有深厚安全和 Web 应用程序背景的安全人员设计。它是查找 Web 应用程序中各种漏洞的绝佳渗透工具，因此任何从事 Web 应用程序测试的人都应将其纳入工具箱。OWASP ZAP 可以执行以下任务：

- 对站点进行蜘蛛发现
- 充当代理
- 主动和被动扫描
- 修改请求和响应

它是 Web 应用程序渗透工具的瑞士军刀，这只是它能做的事情的子集。

在本次练习的实验室测试中，我将使用扫描功能查找任何与会话相关的信息/漏洞/问题。以下是我在实验室中采取的逐步操作。

- 1. 一旦我启动 OWASP ZAP，我会看到默认屏幕，此时不应有任何内容。OWASP ZAP 是一个非常强大的工具，可用于许多 Web 应用程序测试程序，但在我的示例中，我将使用它来扫描基于会话的漏洞，以便我可以利用其他工具检索和使用该会话信息。以下是呈现给我的默认屏幕：

![](img/095df2cd1930baecbcecaa51ee3417aa_188_0.png)

- 2. 然后我将输入要攻击的 URL。输入后，我将点击攻击按钮：

![](img/095df2cd1930baecbcecaa51ee3417aa_189_0.png)

- 3. 一旦我点击攻击，工具首先会对站点运行蜘蛛程序以将其全部列出。完成后，它将开始主动扫描。整个过程中它会显示进度条，显示扫描的进度。取决于站点的大小，此部分可能需要一些时间才能完成：

![](img/095df2cd1930baecbcecaa51ee3417aa_189_1.png)

- 4. 现在进入重点部分。然后你可以浏览到 Alerts 选项卡，查看 OWASP ZAP 在 Web 应用程序中发现的任何问题。如下面所示，发现了许多问题：

![](img/095df2cd1930baecbcecaa51ee3417aa_189_2.png)

- 5. 我将重点关注 Cookie No HttpOnly Flag 以及 Web Browser XSS Protection Not Enabled。如果我深入研究其中每一个，我将能够找到包含这些问题的页面，以及如下截图所示的对问题的详细描述：

![](img/095df2cd1930baecbcecaa51ee3417aa_190_0.png)

## 基础设施和设计弱点

许多人理解基础设施对坚固且安全的网络的重要性，但他们忽略了基础设施对 Web 应用程序也很重要的事实。设计不良的基础设施也可能导致 Web 应用程序中的安全问题。例如，如果你的 Web 服务器和数据库服务器位于完全相同的网络上，那么被攻破的 Web 服务器很可能会导致数据库服务器被攻破。

就像基础设施设计一样，Web 应用程序的实际设计对于成功且安全的 Web 应用程序至关重要。因此，作为渗透测试人员，你希望确保 Web 应用程序安全，并且没有任何可能导致安全事件的设计缺陷。

我将使用以下工具检查我的 Web 应用程序是否存在任何基础设施或设计弱点。

### Uniscan

Uniscan 是一款快速易用的漏洞扫描器，旨在检测远程文件包含、本地文件包含和远程命令执行漏洞。根据我的测试，它是一款功能强大的 Web 应用程序扫描器，能够完成工作。它包含基于 CLI 的版本以及基于 GUI 的选项。我倾向于使用命令行版本，因此在下面的实验室示例中使用了此版本。

运行 Uniscan 有几个 CLI 选项。在帮助指南底部，有几个示例可帮助你根据想要指定主机的方式进行指导，以及大量选项可根据你的最终目标打开某些检查。

```
###########################
# Uniscan project       #
# http://uniscan.sourceforge.net/  #
###########################
V. 6.3
OPTIONS:
    -h      help
    -u      <url> example: https://www.example.com/
    -f      <file> list of url's
    -b      Uniscan go to background
    -q      Enable Directory checks
    -w      Enable File checks
    -e      Enable robots.txt and sitemap.xml check
    -d      Enable Dynamic checks
    -s      Enable Static checks
    -r      Enable Stress checks
    -i      <dork> Bing search
    -o      <dork> Google search
    -g      Web fingerprint
    -j      Server fingerprint
usage:
[1] perl ./uniscan.pl -u http://www.example.com/ -qweds
[2] perl ./uniscan.pl -f sites.txt -bqweds
[3] perl ./uniscan.pl -i uniscan
[4] perl ./uniscan.pl -i "ip:xxx.xxx.xxx.xxx"
[5] perl ./uniscan.pl -o "inurl:test"
[6] perl ./uniscan.pl -u https://www.example.com/ -r
```
