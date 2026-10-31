
## URL 列表
URL 列表允许手动逐行输入 URL。它采用轻量级的 `regular expression` (`RegEx`) 与地址匹配，因此不需要 `http://` 或 `https://` 即可匹配。

## 类别匹配
类别匹配允许您将任意预定义类别组合到自定义类别中。这在对一组预定义类别应用安全规则或解密策略时非常有用。

自定义 URL 类别中使用的字符串会被分隔符分割为子字符串或标记。`./?&=;+` 字符被视为分隔符，因此 `www.example.com` 有三个标记和两个分隔符。每个标记都可以用通配符 (`*`) 替换，以匹配子域或整个 `top-level domains` (`TLDs`)。通配符不能作为标记的一部分使用；例如，`www.ex*.com` 是非法的通配符。每个字符串可以以正斜杠 (`/`) 结束，也可以不加结束斜杠而保持开放。不结束字符串如果字符串很短或很常见，可能会产生后果，因为它可能匹配 unintended 较长的地址。例如，`*.com` 字符串可能匹配 `www.communicationexample.org`，因此添加结束斜杠可以防止这种情况。

还有一个 `Append Trailing Slash` 功能位于 `Device | Setup | Content-ID | URL Filtering`，默认情况下已启用。它会自动为未添加斜杠的任何 EDL 或自定义 URL 类别添加尾部斜杠。如果确实需要开放式条目，请禁用此功能。

使用多个通配符需要使用子域，因此 `*.*.com` 将匹配 anything.anything.com，但不匹配 anything.com。

### 配置 URL 过滤配置文件
配置 URL 过滤配置文件时，您需要在 `Categories` 选项卡中为每个 URL 过滤类别在 `Site Access` 列中选择要应用的操作，如以下屏幕截图所示：

![](Images/B31589_03_12.png)

图 3.12：URL 过滤配置文件

可用的操作如下：

* `none`：仅用于自定义 URL 类别。这将排除自定义类别中包含的 URL 在此 URL 过滤配置文件中的使用。
* `allow`：允许类别且不记录日志。
* `alert`：允许类别并在 URL 过滤日志中记录访问。
* `block`：阻止请求，注入 HTTP 503 错误并重定向到防火墙托管的页面，向用户解释其访问已被拒绝且操作已记录。
* `continue`：注入交互式网页，告知用户即将访问受限网站，并提供 `Continue` 按钮供其确认访问该网站相关的风险。
* `override`：注入交互式网页，允许用户在提供继续密码时继续。此密码可在 `Device | Setup | Content-ID | URL Admin Override` 中设置。

此外，要使用 `override` 选项，需要创建一个接口管理配置文件 (`Network | Network Profiles | Interface Mgmt`)。该配置文件应启用 `Response Pages` 服务并添加到用户连接的接口上，以便此页面正常工作，如以下屏幕截图所示：

![](Images/B31589_03_13.png)

图 3.13：接口管理配置文件

如您在 *图 3.10* 中看到的，URL 过滤配置文件要求为站点访问单独为每个 `Category` 字段设置操作，并且如果启用了 `User Credential Submission`，则可以应用额外的过滤来决定用户是否允许向某个类别提交企业凭据。这有助于防止钓鱼攻击。

如以下屏幕截图所示，如果您想一次更改大量（或全部）操作，有一个快捷方式可以帮助您。如果将鼠标悬停在 `Site Access` 或 `User Credential Submission` 上，会出现一个小箭头，让您选择 `Set All Actions` 或 `Set Selected Actions`：

![](Images/B31589_03_14.png)

图 3.14：URL 过滤配置文件中的“设置所有操作”

可以按如下方式设置一个良好的基线 URL 过滤策略：

1. 将所有类别设置为 `alert`。这将确保所有 URL 类别都被记录。
2. 将 `Adult`、`Command and Control`、`Copyright Infringement`、`Extremism`、`Malware`、`Peer-to-Peer`、`Scanning-activity`、`Real-time-detection` 和 `Phishing and Proxy Avoidance and Anonymizers` 设置为 `Block`。
3. 将 `Dating`、`Gambling`、`Games`、`Hacking`、`Insufficient Content`、`Not-Resolved`、`Parked`、`Questionable`、`Unknown` 和 `Web Advertisements` 设置为 `Continue`。
4. 根据公司政策或当地法律法规调整设置（例如，出于法律原因某些 URL 类别不能被记录）。设置为 `Continue` 的 `Categories` 通常处于可接受边缘，但仍可能出于合法目的需要访问。`Continue` 操作使用户有机会在实际打开网页之前确认其确实打算访问该 URL。

`URL Filtering Settings` 选项卡包含多个日志选项，具体取决于您的需求可能很有用：

* `Log container page only`：此设置仅记录用户请求的实际访问，并会抑制相关网页链接，例如用户正在访问的页面上的嵌入式广告和内容链接，从而减少日志量。
* `Safe Search Enforcement`：如果客户端未启用严格的安全搜索，则阻止访问搜索引擎。目前支持 Google、Bing、Yahoo、Yandex 和 YouTube。

还可以启用额外的日志记录：

* `User-Agent`：这是用户用于访问网页的网络浏览器。
* `Referer`：这是链接到正在访问的资源的网页（例如，Google 或 CNN 链接到资源页面）。
* `x-forward-for`：如果用户使用下游代理，则会掩盖其原始来源。如果下游代理支持启用 `x-forward-for` 功能，它将在 `c` 头中添加客户端的原始 IP，从而允许识别原始用户。

以下步骤和屏幕截图向您展示如何在 URL 过滤配置文件中启用这些设置：

1. 启用 `Log container page only` 以为用户提供一定的隐私并防止记录嵌入式广告页面。
2. 启用 `Safe Search Enforcement`。
3. 为 `User-Agent` 和 `Referer` 启用额外的日志记录。

![](Images/B31589_03_15.png)

图 3.15：URL 过滤设置

`User Credential Detection` 选项卡允许您启用凭据检测（有关更多详细信息，请参阅 *第 6 章*、*识别用户并控制访问*）。

`HTTP Header Insertion` 通过向应用程序提供商的 HTTP/1.x 请求中插入 HTTP 头来控制 Web 应用程序访问。如以下示例所示，这可以帮助您控制可在 Dropbox 中访问的团队 ID，以及可在 Office 365 和 Google 应用允许的域中访问的租户和内容。您可以创建任何需要插入特定头的 URL，以确保用户访问的是适当的实例：

![](Images/B31589_03_16.png)

图 3.16：HTTP 头插入

`Inline Categorization` 选项卡可让您启用额外的扫描以帮助识别钓鱼网站或恶意 JavaScript，并需要 `Advanced URL` 订阅许可证：

![](Images/B31589_03_17.png)

图 3.17：内联分类

您可以单独启用本地和云分类，以防您不想执行云查找或不想给防火墙增加额外负载。

### URL 过滤优先级
