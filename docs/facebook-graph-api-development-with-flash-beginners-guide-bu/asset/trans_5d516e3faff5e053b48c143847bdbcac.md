
```
"username": "facebook",
"founded": "February 4, 2004",
"company_overview": "Millions of people use Facebook everyday to keep up with friends, upload an unlimited number of photos, share links and videos, and learn more about the people they meet.\n\n\"Like\" this page for ongoing updates on new products, announcements and stories. \n\nOther ways to connect with us:\nVisit the Facebook Blog at http://blog.facebook.com\nFollow us on Twitter: @facebook\nSubscribe to more video at http://www.youtube.com/facebook\n",
"mission": "Facebook's mission is to give people the power to share and make the world more open and connected.",
"fan_count": 17565186
}
```

然后，ID 为 107885072567744 的页面：

https://graph.facebook.com/107885072567744

```
{
  "id": "107885072567744",
  "name": "Facebook",
  "link": "http://www.facebook.com/pages/Facebook/107885072567744",
  "category": "Interest",
  "is_community_page": true,
  "description": "<p><b>Facebook</b> is a <a href=\"/pages/w/107511275938278\">social networking</a> website launched in February 2004 that is operated and privately owned by Facebook, Inc., with more than 500 million active users in July 2010.</p>",
  "fan_count": 299474
}
```

啊哈！第二个是一个社区主页——一个由 Facebook 用户创建并拥有的、关于 Facebook 的页面，而不是 Facebook 自身创建的。如果你在浏览器中加载这个主页（使用 link 属性中的 URL），你会看到“网站”字段没有任何内容，这就是为什么我们第一次查询得到的第二个结果是空白的。谜团解开了。

## 动手实践——创建 FQL 查询构建器

创建一个 Flash 应用程序，允许你输入 FQL 查询，运行它，并显示结果。你需要使用某个东西，就像我们在第 2 章中所做的那样。这将使你在本章其余部分测试查询时容易得多！

## 连接呢？

所以这本质上是从单个 Graph 对象获取数据的 FQL 等价物。但获取 Graph 对象的连接又等价于什么呢？

理解数据库模型中连接的形成方式，比使用 Graph API 模型更难掌握。（如果这是 Graph API 背后的主要动机之一，我也不会感到惊讶。）在某种意义上，你必须反向思考。

让我们以装满照片的相册为例。首先，想象我们创建了一个名为“迈克尔超棒的假期照片”的空相册。在数据库模型中，它看起来像这样：

| album |
|---|---|---|
| aid | owner | name |
| «自动» | «我的用户ID» | 迈克尔超棒的假期照片 |

`album` 表有一个 `cover_pid` 字段，它被设置为我想设为封面照片的那张图片的 ID。那么假设我拍了一张机场的照片并上传为封面照片；它会被自动分配一个 ID。

现在我们可以将机场照片的 ID 作为 `album` 表的 `cover_pid` 字段的值：

| album |
|---|---| 
| aid | owner | name | cover_pid |
| «自动» | «我的用户ID» | 迈克尔超棒的假期照片 | «机场照片的ID» |

我们可以这样说明：

![](img/4e826fa36038b86f2087ef8b818d0acf_257_0.png)

好了，这解释了一张照片如何连接到一个相册。我们可以用以下 FQL 查询找出哪个相册以某张照片作为封面：

```
SELECT aid
FROM album
WHERE cover_pid = «specific photo's ID»
```

反过来，我们可以用这个查询找出哪个照片是某个相册的封面：

```
SELECT pid
FROM photo
WHERE pid = «album's cover_pid»
```

……实际上，我们可以用一个非常相似的查询找出某个相册中有哪些照片：

```
SELECT pid
FROM photo
WHERE aid = «album's ID»
```

与 Graph 模型相比，这可以被认为是“反向”的，因为在 Graph 中，你获取相册对象并请求所有相关的照片：`album/photos`；而在数据库模型中，你获取照片表并请求所有属于某个相册的行：`FROM photo WHERE aid = «album id»`。

## 照片、相册及其所有者

你注意到了吗？photo 和 album 表都有一个名为 “owner” 的字段，包含所有者的用户 ID？现在，我们当然可以把它们连接起来。第一次尝试可能看起来像这样：

![](img/4e826fa36038b86f2087ef8b818d0acf_259_0.png)

那条线上方该放什么词？聪明的猜测是“是”，因为所有者相同——小心，这是一个陷阱。

这些“实体关系”线并不连接数据库表中的*字段*；它们连接整个表，*使用*字段作为“键”。记住从左到右阅读动词的规则。如果我们使用“是”作为动词，它就会变成“照片是指定的相册”，这显然是错误的。

我们不能直接连接这两张表，因为它们的关系是通过另一个实体建立的。让我来展示一下我的意思：

![](img/4e826fa36038b86f2087ef8b818d0acf_260_0.png)

现在我们有了三张表（photo、album、user）和四个关系：

- 照片属于指定的相册
- 照片是指定相册的封面
- 照片属于指定的用户
- 用户拥有指定的相册

所以我们可以用这个查询获取某个用户所有照片的 URL：

```
SELECT src
FROM photo
WHERE owner = «user's ID»
```

## 动手实践——获取用户的相册

看看你能否写出一个等效的查询，用于获取某个用户所有相册的 URL。你需要使用的表和列名都在上面的图中，但你可以在 http://developers.facebook.com/docs/reference/fql/ 找到更多信息。

记住，你总是可以通过 API URL 调用来检查你的查询。

## 主键

你注意到了吗？关系线的一端总是指向表的主 ID？

- `photo.pid`
- `user.uid`
- `album.aid`

这些字段中的每一个都被称为该表的**主键**。这意味着任何给定的行都可以用该字段（且仅用该字段）来标识；每一行在其主键字段中都有一个值，并且没有两行具有相同的主键值。

通过用粗体书写主键的名称，我们可以更容易地理解表之间的关系。

我们也有一个名称，用于指向其他表主键的字段，比如：

- `photo.owner`（链接到 `user.uid`）
- `photo.aid`（链接到 `album.aid`）
- `album.cover_pid`（链接到 `photo.pid`）

我们称这些为**外键**，并且倾向于在任何图中用斜体书写它们。

我们之前关于 **photo**、**user** 和 **album** 表的图可以重新绘制，如下一张截图所示：

![](img/4e826fa36038b86f2087ef8b818d0acf_261_0.png)

## 乌鸦脚

所以我们有两条线连接 album 表和 photo 表。

对于给定的相册，这两个查询的结果有什么不同？

```
SELECT pid
FROM photo
WHERE aid = «album ID»
```

和

```
SELECT pid
FROM photo
WHERE pid = «album's cover_pid»
```

答案是，第一个可能返回多个照片的 ID，而第二个只会返回一个。我们可以在关系语句的措辞中明确这一点，像这样：

> 相册包含**零个或多个**照片

> 相册的封面是**一个且仅一个**照片

注意，我必须反转句子，以“相册”而不是“照片”开头，这样才有意义。

我们也可以在图中表示这一点；我们使用所谓的**乌鸦脚标记法**：

![](img/4e826fa36038b86f2087ef8b818d0acf_262_0.png)

顶部线右侧的三个叉（“**乌鸦脚**”）表示“多”，圆形表示“零”，所以这读作“零到多”或“零或多个”。底部线右侧的一对横线表示“一个且仅一个”。

不过，这并没有说明全部情况。并非所有照片都是相册的封面照片。我们需要反转句子来用语言解释：

> “照片属于一个且仅一个相册”

> “照片是零个或一个相册的封面”

![](img/4e826fa36038b86f2087ef8b818d0acf_263_0.png)

这里，顶部线上的一对横线再次表示“一个且仅一个”，而圆和横线表示“零或一个”。我保留了线上的其他标记，但由于表现在调换了方向，它们位于左侧而不是右侧。

一开始阅读这些图有点棘手；最好的方法是将它们分解成我们在这里所做的成对句子。你最终会掌握的。

## 动手实践——绘制你自己的乌鸦脚

学习使用乌鸦脚标记法的另一种方法是绘制你自己的图。重新绘制连接 photo、user 和 album 表的图，并在每条线的每一侧添加适当的标记。记住从成对的关系语句开始，弄清楚你需要表达什么！

你可以用铅笔和纸来完成，但如果你想进一步为所有表绘制图，可以考虑使用像 DbSchema 这样的软件：http://www.dbschema.com/。记住，表和列的列表可在 http://developers.facebook.com/docs/reference/fql/ 获取。

## 链接表

我们如何在朋友之间创建同样的链接？嗯，这必须是 user 表与其自身之间的关系，所以一个不错的初步猜测是：

> “一个用户与零个或多个用户是好友”
