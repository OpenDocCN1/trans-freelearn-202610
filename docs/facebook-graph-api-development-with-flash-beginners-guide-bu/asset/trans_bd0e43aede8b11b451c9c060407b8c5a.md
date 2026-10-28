
Facebook 回复：“首先我需要看看你的 ID，以证明你确实是 Bill 的酷应用。”

应用说：“没问题，这是我的凭证。”

Facebook 检查了一下，说：“这些看起来没问题。等等，我怎么知道这是真正的用户 Joe，而不是某个戴假发的家伙？”

Joe 用他的电子邮件地址和密码登录 Facebook，同时遮挡住这些信息，以免 Bill 的酷应用看到。应用礼貌地看向别处。

Facebook 说：“嘿，Joe，很高兴见到你。你愿意让 Bill 的酷应用访问它请求的这些数据吗？”

Joe 检查了应用想要获取的内容，说：“是的，当然，我没问题。”

Facebook 说：“太好了。好的，Bill 的酷应用，这是访问令牌。如果你想访问 Joe 的任何数据，在请求信息时只需出示这个令牌，我就会把数据给你。”

应用说：“酷！那么，我可以随时使用这个令牌吗？”

Facebook 说：“嗯，不是的。它会在几个小时内过期。之后，你必须再申请一个，而且需要 Joe 保持登录状态才能获取。如果 Joe 登出，情况也一样；你需要获取另一个令牌，这意味着要让 Joe 再次登录。”

应用说：“这似乎有点不方便。你的意思是我每隔几个小时就得让 Joe 同意我访问他的数据？”

Facebook 回答：“哦，不，不，不。既然 Joe 已经同意你可以查看他的信息，我会记住这一点，不会再打扰他询问。而且如果他已经登录，我也不需要查看他的电子邮件和密码。”

Joe 和应用感谢了 Facebook，然后去做他们最初想做的事情。

当然，这些信息并不是通过对话形式传递的，那太傻了。应用凭证通过 Graph URL 传递给 Facebook，而用户通过标准的 Facebook 登录网页提供他们的凭证。Facebook 不是通过 JSON 返回访问令牌，而是通过一个参数传递给应用的 URL——这就是为什么你的应用需要一个 Web 主机。

首先，你可能想知道我们从哪里获得应用凭证。我们不能凭空捏造；我们需要先在 Facebook 上注册我们的应用。现在让我们来做这件事。

## 动手实践 – 在 Facebook 上注册应用

在 http://www.facebook.com/developers/ 注册成为 Facebook 开发者。别担心；这是免费的，而且是即时的。

你可能会被要求通过输入有效的手机号码或添加信用卡来验证你的 Facebook 账户。你不会被收费（如果你选择短信验证方式，可能需要支付标准短信的费用）；这是为了确保每个应用都对应着一个真实的人。

在开发者页面，点击右上角的**设置新应用**：

![](img/4e826fa36038b86f2087ef8b818d0acf_86_0.png)

你将被要求输入应用的名称。最好将你的名字或公司名称作为应用名称的一部分，这样更有可能确保唯一性。（如果你愿意，之后也可以更改应用名称；这并不是一成不变的。）你还需要同意**Facebook 条款和条件**——请务必先阅读它们。

![](img/4e826fa36038b86f2087ef8b818d0acf_86_1.png)

## 动手实践 – 使用浏览器请求访问令牌

打开浏览器，访问 https://graph.facebook.com/oauth/authorize?client_id=<insert_application_id> （将 <insert_application_id> 替换为你应用的应用程序 ID）。

你会看到以下错误：

```
{
  "error": {
    "type": "OAuthException",
    "message": "Missing redirect_uri parameter."
  }
}
```

Facebook 不会直接输出访问令牌；它希望将浏览器重定向到另一个网页，并将访问令牌传递给该页面。目前，这还不是很有用。我们的 Flash 应用是作为独立的 SWF 文件运行的，并未嵌入到页面中——所以我们将采用一个快速的变通方法。

### 在我们的应用中注册重定向 URI

进入你的应用设置（访问 http://www.facebook.com/developers，然后点击你的应用名称，再点击“编辑设置”）。点击左侧导航栏中的“网站”选项卡。

![](img/4e826fa36038b86f2087ef8b818d0acf_89_0.png)

你会看到如下截图所示的面板：

![](img/4e826fa36038b86f2087ef8b818d0acf_90_0.png)

“站点 URL”框定义了在授权用户和应用后 Facebook 重定向到的 URL。这也是访问令牌将被传递到的 URL。

在我们创建自己的页面之前，我们将其设置为 Google 首页。我相信他们不会介意的。在框中输入 http://google.com/ ，然后点击“保存更改”。

![](img/4e826fa36038b86f2087ef8b818d0acf_90_1.png)

如果你再次尝试访问 https://graph.facebook.com/oauth/authorize?client_id=<your_application_id> ，你仍然会得到相同的错误信息。即使我们在应用设置中指定了重定向目标页面，我们仍然需要将它传递给授权 URL。

将凭证作为参数输入到 URL 中存在安全风险；它们不够安全，可能被任何人读取。这就产生了一个问题：Facebook 希望看到我们的应用密钥，以证明我们确实是使用所提供的应用 ID 登录的，但我们不能冒险通过 URL 传递密钥。

为了解决这个问题，我们可以告诉 Facebook，我们正在使用用户端的 Web 浏览器进行身份验证，这样 Facebook 就会让我们通过，而无需我们的应用密钥。这需要传递另一个参数：type=user_agent（user agent 是运行在用户计算机上的 Web 浏览器等应用程序的通用术语）。

所以，请访问此 URL：https://graph.facebook.com/oauth/authorize?client_id=«your_application_id»&redirect_uri=http://google.com&type=user_agent。

> 请注意，每个参数的值与下一个参数的名称之间用“&”符号分隔，而且第二个参数叫作 redirect_uri，而不是 redirect_url。（这不是笔误；URL 是 URI 的一种特定类型。）

你应该会看到一个 Facebook 登录网页：

![](img/4e826fa36038b86f2087ef8b818d0acf_91_0.png)

输入你的详细信息并点击“登录”。你将被要求允许该应用访问你的基本信息：

![](img/4e826fa36038b86f2087ef8b818d0acf_91_1.png)

### 放我进去！

点击“允许”。你将被重定向到 Google——但请仔细查看这个 URL：

```
http://google.com/#access_token=123675230995389%7C2.hX2Png6g2LUuczfd4tLPbQ__.3600.1276960311-169573433058884%7C8BAAidDtlt8fulZHA3bC3YxcCwM
```

终于！我们有了访问令牌。你可以看到它是我们被重定向到的 URL 中 access_token 参数的值。在这个例子中，它是：123675230995389%7C2.hX2Png6g2LUuczfd4tLPbQ__.3600.1276960311-169573433058884%7C8BAAidDtlt8fulZHA3bC3YxcCwM。

（你的令牌会不同，因为令牌是基于用户 ID、应用 ID 和授权时间生成的，但看起来会类似。）

### 使用访问令牌

让我们试一试。像之前一样，尝试通过 Graph API 加载你自己的个人资料：

```
https://graph.facebook.com/«your_username»
```

如果你还记得，我之前得到的是：

```
{
  "id": "«redacted»",
  "name": "Michael James Williams",
  "first_name": "Michael",
  "middle_name": "James",
  "last_name": "Williams",
  "gender": "male",
  "locale": "en_GB"
}
```

现在再试一次，但这次在 URL 中添加一个 access_token 参数：

```
https://graph.facebook.com/«your_username»?access_token=123675230995389%7C2.hX2Png6g2LUuczfd4tLPbQ__.3600.1276960311-169573433058884%7C8BAAidDtlt8fulZHA3bC3YxcCwM
```

这一次，你会得到更多数据，如下面的代码片段所示：

```
{
  "id": "«redacted»",
  "name": "Michael James Williams",
  "first_name": "Michael",
  "middle_name": "James",
  "last_name": "Williams",
  "link": "«redacted»",
}
```
