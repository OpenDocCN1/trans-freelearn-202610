

## HTTP 代理

代理服务器充当客户端与主 Web 服务器之间的中间服务器。Web 浏览器向服务器发送请求，这些请求实际上会通过代理传递，而代理则将服务器的响应返回给客户端。

代理通常用于监控/过滤、性能提升、翻译以及互联网相关资源的保护。代理也可以作为服务购买，这可能还用于处理跨域资源。代理实现也有各种形式，例如 Web 代理（可用于绕过 IP 封锁）、CGI 代理和 DNS 代理。

你可以购买或与代理卖家或类似组织签订合同。他们会根据你运营的国家为你提供各种类型的代理。在爬取过程中频繁进行代理切换——代理允许我们绕过受限内容也是如此。通常，如果请求通过代理路由，我们的 IP 就会相对安全，不会被泄露，因为接收方在他们的详情或服务器日志中只会看到第三方代理。你甚至可以通过切换到不同的代理来访问在你所在位置不可用的网站（即你在本国会看到访问被拒绝的消息）。

使用 HTTP GET 请求传递的基于 Cookie 的参数、与 HTML 表单相关的 HTTP POST 请求，以及修改或适配头部，将在管理代码（即脚本）和在网页抓取过程中访问内容时至关重要。

> **重要说明**
> 有关 HTTP、头部、Cookie 等的详细信息将在即将到来的章节 *Data-finding techniques used in web pages* 中进一步探讨。请访问 MDN Web 文档中的 HTTP 页面（https://developer.mozilla.org/en-US/docs/Web/HTTP）以获取有关 HTTP 及相关概念的更详细信息。请访问 https://www.softwaretestinghelp.com/best-proxy-server/ 以获取有关最佳代理服务器的信息。

你现在已经了解了有关 HTTP 的一般概念（包括请求、响应、Cookie 和代理）。接下来，我们将了解用于创建 Web 内容或以某些预定义格式提供内容的[content]

## HTTP 代理

代理服务器充当客户端与主 Web 服务器之间的中间服务器。Web 浏览器向服务器发送请求，这些请求实际上会通过代理传递，而代理则将服务器的响应返回给客户端。

代理通常用于监控/过滤、性能改善、翻译以及互联网相关资源的安全。代理也可以作为服务购买，这可能还用于处理跨域资源。代理实现也有各种形式，例如 Web 代理（可用于绕过 IP 封锁）、CGI 代理和 DNS 代理。

你可以购买或与代理卖家或类似组织签订合同。他们会根据你运营的国家为你提供各种类型的代理。爬取时经常进行代理切换——代理也允许我们绕过受限内容。通常，如果请求通过代理路由，我们的 IP 相对安全且不会被泄露，因为接收方在其详情或服务器日志中只会看到第三方代理。你甚至可以通过切换到不同的代理来访问在你所在位置不可用的网站（即你在本国会看到访问被拒绝的消息）。

使用 HTTP GET 请求传递的基于 Cookie 的参数、与 HTML 表单相关的 HTTP POST 请求，以及修改或适配头部，将在管理代码（即脚本）和网页抓取过程中访问内容时至关重要。

> **重要说明**
> 有关 HTTP、头部、Cookie 等的详细信息将在即将到来的章节 *Data-finding techniques used in web pages* 中进一步探讨。请访问 MDN Web 文档中的 HTTP 页面（https://developer.mozilla.org/en-US/docs/Web/HTTP）以获取有关 HTTP 及相关概念的更详细信息。请访问 https://www.softwaretestinghelp.com/best-proxy-server/ 以获取有关最佳代理服务器的信息。

你现在已经了解了有关 HTTP 的一般概念（包括请求、响应、Cookie 和代理）。接下来，我们将了解用于创建 Web 内容或以某些预定义格式提供内容的

## HTML

网站由包含文本、图像、样式表和脚本等内容的页面或文档组成。它们通常使用 **Hypertext Markup Language (HTML)** 和 **Extensible Hypertext Markup Language (XHTML)** 等标记语言构建。

HTML 通常被称为用于构建网页的标准标记语言。自 1990 年代初以来，HTML 已独立使用，也与服务器端脚本语言（如 PHP、ASP 和 JSP）结合使用。XHTML 是 HTML 的高级扩展版本，是 Web 文档的主要标记语言。XHTML 也比 HTML 更严格，从编码角度来看，也被视为使用 **Extensible Markup Language (XML)** 构建的应用程序。

HTML 定义并包含网页的内容。可以在 HTML 页面内找到可提取的数据和任何信息揭示的数据源，这些数据位于预定义指令集或称为标签的标记元素中。HTML 标签通常是一个带有某些预定义属性的命名占位符，例如 `<a>`、`<b>`、`<table>`、`<img>` 和 `<script>`。

HTML 是一种容器或标记语言类型。构建 HTML 涉及各种因素；下一节将使用一些示例定义这些因素。

### HTML 元素和属性

HTML 元素（也称为文档节点）是 Web 文档的构建块。HTML 元素由开始标签 `<...>` 和结束标签 `</...>` 构建，中间包含某些内容。HTML 元素还可以包含属性，通常定义为 `attribute-name = attribute-value`，为元素提供额外信息：

```
```html
<p>normal paragraph tags</p>
<h1>heading tags there are also h2, h3, h4, h5, h6</h1>
<a href="https://www.google.com">Click here for Google.com</a>
<img src="myphoto1.jpg" width="300" height="300" alt="Picture" />
<br />
```
```

前面的代码可以分解如下：

- `<p>` 和 `<h1>` 是包含一般文本信息（元素内容）的 HTML 元素。
- `<a>` 定义了 href 属性，其中包含当点击文本 **Click here for Google.com** 时将被处理的实际链接。链接指向 `https://www.google.com/`。
- `<img>` 图像标签还包含一些属性，如 src 和 alt 及其各自的值。`src` 的值保存资源，即图像地址或图像 URL，而 `alt` 保存 `<img>` 的替代文本值（主要在连接缓慢或图像无法加载时显示）。
- `<br />` 在 HTML 中表示换行符，没有属性或文本内容。它用于在文档布局中插入新行。

HTML 元素也可以以树状结构嵌套，具有父子层级关系，如下所示：

```
```html
<div class="article">
    <p id="mainContent" class="content">
        <b>Paragraph Content</b>
        <img src="myLogo.png" id="pageLogo" alt="Logo"
             class="logo" />
    </p>
    <p>
```

```
```html
<h3> Paragraph Title: Web Scraping</h3>
</p>
</div>
```
```

如前面的代码所示，两个 `<p>` 子元素位于 HTML `<div>` 块内。两个子元素都带有某些属性以及作为其内容的不同子元素。通常，HTML 文档以上述结构构建。

如最后一个示例中前面的代码块所示，有一些额外的键值对。下一节将探讨这一点。

### 全局属性

HTML 元素可以包含一些额外信息，例如键值对。这些也称为 HTML 元素属性。属性保存值并提供标识，或包含在抓取活动中的许多方面可能有帮助的额外信息，例如识别确切的 Web 元素并从中提取值或文本以及遍历（*移动沿着*）元素。

某些属性是 HTML 元素的通用属性或可应用于所有 HTML 元素。以下列表提到了一些被识别为全局属性的属性（https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes）：

- `id`：该属性的值应唯一于其应用的元素
- `class`：该属性的值主要与 CSS 结合使用，提供相等状态的格式化选项，可用于多个元素
- `style`：这为元素指定内联 CSS 样式
- `lang`：这有助于识别文本的语言

> **重要说明**
> 
> `id` 和 `class` 属性主要用于标识或格式化单个元素或其组。这些属性也可以由 CSS 和其他脚本语言管理。当与 CSS 一起使用或在遍历和应用解析技术时，可以通过在属性名称前分别放置 `#` 和 `.` 来识别这些属性。

HTML 元素属性也可以使用脚本语言动态覆盖或实现。如以下示例所示，`itemscope` 属性用于向元素添加属性，而 `data-*` 用于存储元素本身固有的数据：

```
```html
<div itemscope itemtype="http://schema.org/Place">
    <h1 itemprop="university">University of Helsinki</h1>
    <span>Subject: <span itemprop="subject1">Artificial
        Intelligence</span>
```

```
</span><span itemprop="subject2">Data Science</span>
</div>
<img class="dept" src="logo.png" data-course-id="324" datatitle="Predictive Analysis" data-x="12345" data-y="54321" data-z="56743" onclick="schedule.load()"/>
```

HTML 标签和属性在提取数据时非常有帮助。

> **重要说明**
> 请访问 https://www.w3.org 或 https://www.w3schools.com/html 获取有关 HTML 的更详细信息。

在第 3 章中，我们将使用不同的工具探索这些属性。我们还将执行各种逻辑操作并将它们用于提取或抓取目的。

我们现在对 HTML 和与 HTML 相关的一些重要属性有了一些了解。在下一节中，我们将学习 XML 的基础知识，也称为标记语言之父。

## XML

XML 是一种用于在互联网上分发数据的标记语言，具有用于编码可读且易于在机器和文档之间交换的文档的规则集。XML 文件由 .xml 扩展名识别。

XML 强调文本数据在各种格式和系统中的可用性。XML 设计用于携带便携式数据或存储在标签中的数据，而这些数据不是使用 HTML 标签预先定义的。在 XML 文档中，标签由文档开发者或自动化程序创建以描述内容。

以下代码显示了一些示例 XML 内容：

```
<employees>
  <employee>
    <fullName>Shiba Chapagain</fullName>
    <gender>Female</gender>
  </employee>
  <employee>
    <fullName>Aasira Chapagain</fullName>
    <gender>Female</gender>
  </employee>
</employees>
```

在前面的代码中，<employees> 父节点有两个 <employee> 子节点，而每个子节点又包含 <fullName> 和 <gender> 的其他子节点。

