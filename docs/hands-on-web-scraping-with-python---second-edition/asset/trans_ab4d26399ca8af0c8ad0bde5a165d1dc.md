

# 用户认证与 Cookie

用户认证（管理与处理用户凭证）是另一种形式的网络安全。在本节中，我们将使用 http://quotes.toscrape.com/login 站点上可用的用户认证功能，以及 Cookie。

如图 6.6 所示，http://quotes.toscrape.com/login 包含一个带有 Login 文本的页面，以及一个要求输入 Username 和 Password 的表单：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_165_0.png)

图 6.6：用户登录页面

分析页面源代码，仅找到一个单一的 <form> 元素，其中包含一个名为 csrf_token 的隐藏输入元素、两个 type="text" 的文本输入元素（username 和 password），最后是一个 submit 类型的对象，其值为 Login，如图 6.7 所示：

```
<form action="/login" method="post" accept-charset="utf-8">
  <input type="hidden" name="csrf_token" value="KGVfWfPoucWyElYtaReDdkrB5SbHCjQQsLzvTMfxgiopIZnUAhNXmq"/>
  <div class="row">
    <div class="form-group col-xs-3">
      <label for="username">Username</label>
      <input type="text" class="form-control" id="username" name="username" />
    </div>
  </div>
  <div class="row">
    <div class="form-group col-xs-3">
      <label for="username">Password</label>
      <input type="password" class="form-control" id="password" name="password" />
    </div>
  </div>
  <input type="submit" value="Login" class="btn btn-primary" />
</form>
```

图 6.7：登录页面 – <form> 源代码

## 重要说明

http://toscrape.com 是一个用于抓取相关目的而丰富内容的演示站点。站点不提供用户注册链接。为了练习认证，我们可以使用用户名和密码的任意凭证。成功登录后会加载 http://quotes.toscrape.com 并显示用户注销选项，或我们可以使用 http://quotes.toscrape.com/logout 链接。

如以下代码所示，所需的链接已定义。我们现在需要从 loginUrl 可用的 HTML 表单中收集与表单相关的数据（如图 6.7 所示）：

```
import requests
from pyquery import PyQuery as pq
baseUrl="http://quotes.toscrape.com/"  # main URL
loginUrl="http://quotes.toscrape.com/login" # POST URL (Credentials and results)

logoutUrl="http://quotes.toscrape.com/logout"
```

为了处理登录步骤，我们已声明了必要的 URL。在以下代码中，我们已使用一些测试值定义了 user 和 passw 变量。这两个值将用于在 payload 中提交。loginUrl 已被处理，我们已收集了 csrf_token、username 和 password 的值：

```
user = "test"
passw = "password"
html = requests.get(loginUrl)  # load loginUrl
response = pq(html.content)
# collect <form> data, required for submission

csrf = response.find('form:first input[name="csrf_token"]')
    .attr('value')
username = response.find('input[id="username"]')
    .attr('name')
password = response.find('input[id="password"]')
    .attr('name')
```

在获取到表单数据后，需要创建 payload 信息：payload = {"csrf_token":csrf, username:user, password:passw}。此 payload 将提交到表单属性 action 中提及的路径或 loginUrl，在本例中即为 loginUrl。经调查（使用 DevTools），看起来 loginUrl 在表单提交时使用 payload 处理，但会被重定向（如图 6.8 所示）到 baseUrl，加载 logoutUrl 的链接：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_167_0.png)

图 6.8：HTTP 状态码 302

> **重要说明**
> 
> 原始 URL 产生 **状态码：302 Found** 错误并被重定向到另一个 URL。有关更多信息，请访问 https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302。

如以下代码块所示，使用 DevTools 找到并确认的所需 `payload` 字典对象已发布到 `loginUrl`。在提交表单时（如 *图 6.8* 所示），重定向必须发生，但它（`postHTML.url`）仍显示 `loginUrl`：

```
payload = {"csrf_token":csrf, username:user,
          password:passw}

postHTML = requests.post(loginUrl, data=payload)
postHTML.url
# http://quotes.toscrape.com/login
```

对于当前场景，要么 payload 有问题，要么存在代码相关错误。经过多次尝试和验证，发现迄今为止处理的代码没有问题，但 POST 请求也在寻找可在 Cookie 中提交的会话相关数据。

如 *图 6.8* 所示，我们可以看到 **Request Headers** 选项卡中有一个 **Cookie** 条目，其中包含一些 *session* 相关的值。我们需要收集 HTTP 头和 Cookie 才能成功处理表单。有关 HTTP 头和 Cookie 的详细信息，请参阅 *第 2 章* 的 *URL 处理和操作* 部分。

为了继续认证部分，如 *图 6.9* 所示，`html.headers` 输出 HTTP 请求头：

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_168_0.png)

**图 6.9：** 收集 HTTP 头和所需值

我们正尝试从返回的头数据中收集 Set-Cookie (`html.headers['Set-Cookie']`) 的值。

如以下代码所示，Python requests 库支持各种参数，例如 data 和 headers：

```
```python
payload = {"csrf_token":csrf, username:user,
        password:passw}

postHTML = requests.post(
    loginUrl,
    data=payload,
    headers={'Cookie':html.headers['Set-Cookie']}
    )    # POST request

postHTML.url      # http://quotes.toscrape.com/
verify =
    pq(postHTML.content).find('a[href="/logout"]').text()
    # Logout
```
```

向 `loginUrl` 发送 HTML POST 请求并使用 `headers={'Cookie':html.headers['Set-Cookie']}` 参数似乎现在可以正常工作，这显示了重定向后的 URL，`http://quotes.toscrape.com`，我们也可以在那里找到注销的 URL（`verify`）。用户认证的完整代码可在 GitHub 上找到：https://github.com/PacktPublishing/Hands-On-web-scraping-with-Python-Second-Edition/blob/main/Chapter06/chapter06_authentication.ipynb。

在 Cookie 和会话的帮助下，认证过程已成功完成。用户在完成任何任务后注销会话是其道德责任。`requests.get(logoutUrl)` 仅终止现有的用户会话。

## 重要说明
还需考虑一个重要因素是，在当前情况下，仅 `Cookie` 头值就足以成功提交表单。HTTP 头中还有大量其他键（例如 `Accept`、`Origin`、`Host` 和 `User-Agent`），它们也很重要，必须与 HTTP GET 或 POST 参数一起传递才能成功提交表单。

除了表单元素之外，HTTP 头和 Cookie 值在表单提交中也起着重要作用，甚至在从处理的链接接收预期响应时也起着重要作用。在下一节中，我们将介绍并学习如何使用 HTTP 代理。

# 使用代理

代理（HTTP 代理或网络代理）被视为网络的中介。代理是用于在客户端和服务器之间通信的网关。简单来说，代理是一个 **Internet Protocol (IP)** 地址，并分配了一些随机端口。在网络上，HTTP 请求从客户端端以其自己的 IP 开始，当通过代理路由时，其 IP 会更新为新的 IP，然后转发到实际目的地以寻找响应。因此，代理在这里充当位于客户端和服务器或目的地之间的传输层。

在互联网上，有大量服务（付费和免费）由各种组织提供，这些代理作为其客户的过滤层运作，并处理不同种类的安全相关威胁、恶意软件、内容过滤、间谍保护以及许多其他活动。使用代理有许多好处，其中一些列在这里：

- **隐私**：目标服务器无法识别实际客户端。隐私问题得到缓解，因为客户端使用网络代理发起请求。
- **安全**：考虑到隐私，客户端信息不会与最终位置即服务器共享。安全功能得以实施，例如阻止多个相关请求的范围以及在一定时间内阻止和列入黑名单 IP。
- **内容过滤**：除了安全和隐私之外，代理服务器或中间方充当客户端和服务器之间的中间件。在此过程中，中间方可以充当安全层，根据某些预配置设置向客户端过滤内容。例如，父母可以使用关于向其子女交付内容的设置（基于年龄、电影证书等诸多因素）。此外，此类设施为终端用户提供针对恶意软件、间谍软件、基于地理位置的垃圾邮件以及任何被检测为有害或可能对客户端机器有害的内容的安全保护。
- **速度**：大多数代理提供商使用其服务器接受客户端请求并代表其执行任务。这些服务器还拥有内置的缓存管理系统，如果内容已存在于缓存中，则会立即将其传回客户端。这加快了请求和响应周期所需的时间。

