

"about": "Twitter: @MichaelJW",
"gender": "male",
"timezone": 1,
"locale": "en_GB",
"verified": true,
"updated_time": "2010-06-16T20:38:01+0000"
}

好，虽然不多，但它证明了我们已成功获得授权，并且可以看到在获得访问令牌之前无法看到的更多信息。

尝试在没有访问令牌的情况下访问你的好友列表：

```
https://graph.facebook.com/«your_username»/friends
{
  "error": {
    "type": "OAuthAccessTokenException",
    "message": "An access token is required to request this resource."
  }
}
```

几乎什么都没有。现在，使用访问令牌（将 «your_access_token» 替换为实际的访问令牌）：

```
https://graph.facebook.com/«your_username»/friends?access_token=«your_access_token»
{
  "data": [
    {
      "name": "JohnFaikname",
      "id": "«redacted»"
    },
    {
      "name": "Sarah Toetalee-Maidup",
      "id": "«redacted»"
    },
    {
      "name": "Bob Frendomyn e",
      "id": "«redacted»"
    },
  ]
}
```

- 2. 你通过直接通过 `Facebook.com` 登录网页登录到自己的账户，证明了自己作为 Facebook 用户的身份。
- 3. Facebook 问你是否愿意让该应用程序访问你的信息，而你授予了它权限。
- 4. Facebook 将访问令牌传回应用程序先前指定的页面（在本例中为 Google 主页）。

这种工作流程并不独特于 Facebook。它实际上是 **OAuth 2.0 协议** 的一部分，被称为 **用户代理流程**。这意味着一旦你掌握了允许应用程序使用 Facebook 进行身份验证所需的工作流程，你就能将该知识应用到编写可使用任何其他使用 OAuth 2.0 的系统进行身份验证的应用程序。

> OAuth 2.0 协议的官方文档在这里：http://tools.ietf.org/html/draft-ietf-oauth-v2-05#section-3.5。

## 保持秘密

看起来我们不需要在 Graph URL 中传递 **应用程序密钥** 来进行身份验证，这很奇怪。其原因在于，如果我们以这种方式传递密钥，那么用户或用户计算机上运行的另一个程序就可以获取它，并冒充我们的应用程序使用它——请记住，密钥类似于密码。因此，用户代理流程让我们能够在不使用其密钥的情况下对应用程序进行身份验证，适用于这种不安全的情况。

用户或桌面应用程序仍然有可能获取访问令牌并用它冒充用户登录到我们的应用程序。但用户这样做没有意义，因为他使用此方法可以访问的任何信息，他本来就可以通过 Facebook 网站访问。至于桌面应用程序，好吧，这只是运行计算机的日常风险之一，与病毒和间谍软件一样。

## Facebook 给了我们什么？

查看 Facebook 在身份验证后重定向我们的 URL：

```
http://google.com/#access_token=123675230995389%7C2.hX2Png6g2LUuczfd4tLPbQ__.3600.1276960311-169573433058884%7C8BAAidDt1t8fulZHA3bC3YxcCwM.&expires_in=6561
```

有两个参数：`access_token` 和 `expires_in`。

第二个参数告诉我们访问令牌在多少秒后过期。它看起来像一个随机数字，但有一个规律：如果我们在 14:32:07 请求令牌，它将在 16:00:00 过期；9:15:34 请求的令牌将在 11:00:00 过期。将令牌生成时间的两小时后，去掉分钟和秒，你就会找到过期时间。

乍一看，访问令牌似乎是一堆字符。实际上，它包含了一些有用的信息。

首先，你应该知道 %7C 是 “管道” 字符 | 的代码，这样它才能在 URL 中传递。因此将两个 %7C 替换为管道：

```
123675230995389|2i.hX2Png6g2LUuczfd4tLPbQ__.3600.1276960311-169573433058884|8BAAidDtl8fulZHA3bC3YxcCwM.
```

这里有三条有用的信息：

- 1. 应用程序 ID：第一个管道之前的数字——本例中为 123675230995389。
- 2. 当前用户的 ID：在破折号之后、最后一个管道之前——本例中为 169573433058884。
- 3. 过期时间：在用户 ID 之前——本例中为 1276960311。这是以 Unix 时间给出的。

Unix 时间系统将时间点描述为从 1970 年 1 月 1 日午夜 UTC 起算的秒数，不考虑闰秒。它提供了一种简单的方法，将自 1970 年以来的任何时间点编码为单个整数。

网站 UnixTime.info 包含一个简单的转换器，用于在 Unix 时间戳和常规日期格式之间切换。

> 在 AS3 中，Date 类可用于在两种时间格式之间转换。将值 epochTime*1000 作为第一个参数传递给 Date() 构造函数以用该时间初始化 Date。Date 对象的 time 属性将以 Unix 格式返回日期，尽管 Flash 使用毫秒作为其基本时间单位，因此乘以了 1000。

这意味着，给定任何访问令牌，你都可以找出哪个用户请求了它、何时过期以及它适用于哪个应用程序——尝试浏览 https://graph.facebook.com/your_application_id。

```
var variables:URLVariables = new URLVariables();

//We construct a URL from the parameters of the GraphRequest
urlRequest.url = "https://graph.facebook.com/" + a_request.objectID;
if (a_request.connectionID)
{
    urlRequest.url += "/" + a_request.connectionID;
}

variables.metadata = 1;
variables.access_token = "123675230995389%7C2.hX2Png6g2LUuczfd4tLPbQ__.3600.1276960311-169573433058884%7C8BAAidDtl8fulZHA3bC3YxcCwM."
urlRequest.data = variables;

//this is used to figure out which GraphRequest created the loader later
_requests[loader] = a_request;

loader.addEventListener(Event.COMPLETE, onGraphDataLoadComplete);
loader.addEventListener(IOErrorEvent.IO_ERROR, onIOError);
loader.load(urlRequest);
}
```

测试应用程序；如果它没有加载任何 Renderer，你的访问令牌可能已过期。在这种情况下，Graph API 将返回以下内容（在浏览器中加载时）：

```
{
  "error": {
    "type": "OAuthException",
    "message": "Error processing access token."
  }
}
```

并且 I/O 错误将被传递给你的应用程序。我们可以通过将 IOErrorEvent 监听器添加到 URLRequest 对象来对这些错误做出反应；第 2 章《Time for Action: Creating an HTTP Requestor》中的 What Just happened? 部分提供了此方面的说明。

因此，如果你确实收到 I/O 错误，请在浏览器中像我们之前那样生成一个新的访问令牌。在浏览器中加载 https://graph.facebook.com/oauth/authorize?client_id=«your_application_id»&redirect_uri=http://google.com&type=user_agent，提取加载的 URL 参数中的访问令牌，然后将其粘贴到你的代码中。

## 让我进入！

这次测试应用程序时，你应该会看到下一个截图：

![](img/4e826fa36038b86f2087ef8b818d0acf_100_0.png)

太好了！我们可以看到列表中出现了新的字段，比如 verified 和 about。

再试着点击列表中的 friends 连接：

![](img/4e826fa36038b86f2087ef8b818d0acf_100_1.png)

它工作得正如我们所期望的那样。 （请记住，HTTPRequestor 将访问令牌作为参数传递给所有请求的 Graph URL。）

看看我们请求用户资料时加载的内容：

```
https://graph.facebook.com/your_user_id?access_token=«your_access_token»&metadata=1
{
  "id": "169573433058884",
  "name": "Michael James Williams",
  "first_name": "Michael",
  "middle_name": "James",
  "last_name": "Williams",
  "link": "http://www.facebook.com/«redacted»",
  "about": "Twitter: @MichaelJW",
  "gender": "male",
  "timezone": 1,
  "locale": "en_GB",
  "verified": true,
  "updated_time": "2010-06-16T20:38:01+0000",
  "metadata": {
    "connections": {
      "home": "https://graph.facebook.com/«redacted»/home?access_token=«your_access_token»",
      "feed": "https://graph.facebook.com/«redacted»/feed?access_token=«your_access_token»",
      "friends": "https://graph.facebook.com/«redacted»/friends?access_token=«your_access_token»",
      «...some datacut out here, for length...»
    }
  },
  "type": "user"
}
```

请注意，connections 列表中的 Graph URL 已预先填入了 access_token 参数。Visualizer 的代码不使用这些 Graph URL——它使用形式为 https://graph.facebook.com/<id_of_object>/<name_of_connection> 构造它们——但这项知识可能对你的项目有用。

正如你从 Visualizer 中看到的，friends 连接的 Graph List 只包含非常基本的信息：用户的 ID 和姓名。然而，你朋友的完整 Graph 对象中加载了更多信息——不过具体数量取决于你朋友的隐私设置。

