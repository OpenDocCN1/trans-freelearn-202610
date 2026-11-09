
登录 Twitter 账号后，在 http://apps.twitter.com 页面，点击 **创建新应用**。填写创建应用所需的详细信息（为其命名，例如 My Tweet Test；简短描述；以及一个 URL，无需永久）。我的已填写应用创建表单在此供您参考：

![](img/9aa003afabe05fdb07da46ad71325de6_242_0.png)

Twitter 的“创建新应用”表单，已用示例数据填写

点击表示您同意开发者协议的复选框，您将被返回到列出所有 Twitter 应用的屏幕，您的新应用位于列表顶部。

接下来，要使用此应用，我们需要获取使其正常工作所需的一些关键信息。点击您刚刚创建的应用，您将在下一屏幕顶部看到四个标签。我们感兴趣的是写着 **密钥和访问令牌** 的那个。它看起来像这样：

![](img/9aa003afabe05fdb07da46ad71325de6_242_1.png)

您新创建的 Twitter 应用中的标签界面。

此屏幕上有许多数字和秘密代码，但有四项需要特别注意：

- CONSUMER_KEY
- CONSUMER_SECRET
- ACCESS_TOKEN
- ACCESS_TOKEN_SECRET

无论您进行何种 Twitter API 编程，至少目前，您始终需要这四项相同的项目。这无论您使用 twarc 还是其他工具都适用。这些凭据是 Twitter 用于验证您的身份并确保您拥有查看所请求内容的访问权限的方式。

![](img/9aa003afabe05fdb07da46ad71325de6_243_0.png)

> 这些 API 凭据也是 Twitter 限制您发送请求的数量以及请求速度的方式。twarc 工具代表我们处理所有这些，因此我们不必太担心超出速率限制。有关 Twitter 速率限制的更多信息，请查看其开发者文档 at https://dev.twitter.com/rest/public/rate-limiting。

## 安装 twarc

现在我们已访问到 Twitter 凭据，可以安装 twarc。

twarc 下载页面在 GitHub 上可用，地址为 https://github.com/edsu/twarc。该页面上有关于如何使用该工具以及有哪些选项的文档。

要在您的 Canopy Python 环境中安装 twarc，启动 Canopy，然后从 Tools 菜单中选择 Canopy Command Prompt。

在命令提示符中，输入：

```
pip install twarc
```

此命令将安装 twarc 并使其可作为命令行程序调用，或从您自己的 Python 程序中调用。

## 运行 twarc

现在我们可以使用命令行中的 twarc 来水合我们之前创建的 ids_1000.txt 文件。此命令行非常长，因为我们必须传入在 Twitter 网站上之前创建的四个长秘密令牌。为了避免出错，我使用文本编辑器 Text Wrangler 先创建命令行，然后再将其粘贴到我的命令提示符中。您的最终命令行将如下所示，只是凡是写着 abcd 的地方，您应改为填写相应的秘密令牌或密钥：

```
twarc.py --consumer_key abcd --consumer_secret abcd --access_token abcd --access_token_secret abcd --hydrate ids_1000.txt > tweets_1000.json
```

请注意，此命令行将把输出重定向到名为 tweets_1000.json 的 JSON 文件中。该文件内部包含我们之前仅有 ID 的每个推文的 JSON 表示。让我们检查新文件的长度：

```
wc tweets_1000.json
```

wc 实用程序显示我的文件长 894 行，这表明有些推文无法找到（因为我最初的数据集中有 1,000 条推文）。如果在撰写本文之后推文已被删除，您文件可能更小。

接下来，我们也可以查看文件内部：

```
less tweets_1000.json
```

我们也可以在文本编辑器中打开它进行查看。

JSON 文件中的每一行代表一条推文，其格式如下例所示。然而，此示例推文并非来自 Ferguson 数据集，因为我没有分发这些推文的权利。相反，我使用了在第 2 章 Fundamentals – Formats, Types, and Encodings 中为我们讨论 UTF-8 编码而创建的推文之一。由于此推文是为本书专门创建的，我拥有内容所有权，因此可以在不违反 Twitter 的 ToS 的情况下向您展示 JSON 格式。以下是我的推文在 Twitter 网页界面中的样子：

![](img/9aa003afabe05fdb07da46ad71325de6_245_0.png)

推文示例，如在 Twitter 网页界面中所示。

以下是 twarc 水合后该推文的 JSON 表示形式。我在每个 JSON 元素之间添加了换行符，以便更轻松地查看每条推文可用的属性：

```
{ "contributors": null, "truncated": false, "text": "Another test. Ég elska gögn. #datacleaning", "in_reply_to_status_id": null, "id": 542486101047275520, "favorite_count": 0, "retweeted": false, "coordinates": null, "entities": { "symbols": [], "user_mentions": [], "hashtags": [ {"indices": [29, 42], "text": "datacleaning"} ], "urls": [] }, "in_reply_to_screen_name": null, "id_str": "542486101047275520", "retweet_count": 0, "in_reply_to_user_id": null, "favorited": false, "user": }
```

```
{"follow_request_sent": false,
"profile_use_background_image": false,
"profile_text_color": "333333",
"default_profile_image": false,
"id": 986601,
"profile_background_image_url_https": "https://pbs.twimg.com/profile_
background_images/772436819/b7f7b083e42c9150529fb13971a52528.png",
"verified": false,
"profile_location": null,
"profile_image_url_https": "https://pbs.twimg.com/profile_
images/3677035734/d8853be8c304729610991194846c49ba_normal.jpeg",
"profile_sidebar_fill_color": "F6F6F6",
"entities":
{"url":
{"urls":
[{"url": "http://t.co/dBQNKhR6jY",
"indices": [0, 22],
"expanded_url": "http://about.me/megansquire",
"display_url": "about.me/megansquire"}]},
"description": {"urls": []}},
"followers_count": 138,
"profile_sidebar_border_color": "FFFFFF",
"id_str": "986601",
"profile_background_color": "000000",
"listed_count": 6,
"is_translation_enabled": false,
"utc_offset": -14400,
"statuses_count": 376,
"description": "Open source data hound. Leader of the FLOSSmole
project. Professor of Computing Sciences at Elon University.",
"friends_count": 82,
"location": "Elon, NC",
"profile_link_color": "038543",
"profile_image_url": "http://pbs.twimg.com/profile_images/3677035734/
d8853be8c304729610991194846c49ba_normal.jpeg",
"following": false,
"geo_enabled": false,
"profile_banner_url": "https://pbs.twimg.com/profile_
banners/986601/1368894408",
"profile_background_image_url": "http://pbs.twimg.com/profile_
background_images/772436819/b7f7b083e42c9150529fb13971a52528.png",
"name": "megan squire",
"lang": "en",
"profile_background_tile": false,
```

```
"favourites_count": 64,
"screen_name": "MeganSquire0",
"notifications": false,
"url": "http://t.co/dBQNKhR6jY",
"created_at": "Mon Mar 12 05:01:55 +0000 2007",
"contributors_enabled": false,
"time_zone": "Eastern Time (US & Canada)",
"protected": false,
"default_profile": false,
"is_translator": false},
"geo": null,
"in_reply_to_user_id_str": null,
"lang": "is",
"created_at": "Wed Dec 10 01:09:00 +0000 2014",
"in_reply_to_status_id_str": null,
"place": null}
```

不仅每个 JSON 对象包含推文本身的事实，例如文本、发送日期和时间，它还包含发布推文的人的丰富信息。

> 水合过程会产生关于单个推文的大量信息——而这是一个总共包含 13 million 条推文的数据集。请在准备在 本章末尾水合整个 Ferguson 数据集时牢记这一点。

## 第三步 – 数据清洗

在这一点上，我们准备开始清理 JSON 文件，提取我们希望保存在长期存储中的每条推文的详细信息。

## 创建数据库表

由于我们的动机问题只询问 URL，我们实际上只需要提取这些 URL，以及推文 ID。然而，为了练习清洗，并让我们能够将此练习与我们在第 7 章 RDBMS Cleaning Techniques 中使用 sentiment140 数据集所做的工作进行比较，让我们按如下方式设计一组小型数据库表：

- 一个 tweet 表，仅保存有关推文的信息
- 一个 hashtag 表，保存有关哪些推文引用了哪些 hashtag 的信息

