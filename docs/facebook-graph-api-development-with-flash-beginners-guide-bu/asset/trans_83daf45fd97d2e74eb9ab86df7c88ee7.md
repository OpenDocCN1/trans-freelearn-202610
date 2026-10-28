

3. 没有访问令牌，我们可以发布什么？

a. 什么都不能
b. 只能发布状态消息
c. 只能点赞
d. 什么都可以

## 总结

现在，你已经学会了如何创建多种不同类型的图谱对象：帖子、评论、活动等等。你还学会了在需要时如何删除它们。

对于图谱 API 无法做到的事情，你是否感到惊讶？

在下一章中，我们将重新审视图谱，但我们将使用一个强大的新工具——Facebook 查询语言。

要点回顾：

- 要通过 HTTP 发布，照常使用 URLRequest/URLLoader/URLVariables 设置，并在 URLVariables 实例中随访问令牌一起包含新图谱对象的参数。
- 使用的 URL 是 https://graph.facebook.com/«id»/«connection»。务必使用 flash.net.URLRequestMethod.POST。
- 要通过 SDK 发布，调用 Facebook.api("«id»/«connection»", «callback», {parameter1:"value1", parameter2:"value2"}, URLRequestMethod.POST)。
- 上传照片需要 FileReference。使用 FileReference.browse() 让用户从硬盘中选择图片。
- 要通过 HTTP 上传：使用 FileReference.upload() 而不是 URLLoader.load()。
- 要通过 SDK 上传：只需将 FileReference 作为 URLVariables 实例中 source 参数的值传入，然后照常调用 Facebook.api() 即可。
- 通过图谱 API，你只能“赞”帖子，不能“赞”其他内容。这意味着我们无法修改用户的兴趣或最喜欢的电视节目。
- 在编写本书时，我们只能删除使用同一应用程序创建的帖子。
- 要通过 HTTP 删除，只需使用 URLRequestMethod.POST 将 `method=delete` 传递给 `https://graph.facebook.com/<id>`。
- 如果使用 AIR，你可以直接用 `URLRequestMethod.DELETE` 加载 `https://graph.facebook.com/<id>`。
- 要通过 SDK 删除，调用 `Facebook.deleteObject("<id>", "<callback>")`。
- 例外情况是删除“赞”；在这种情况下，要删除的 URL 路径是 `<id>/likes`。
- 发布所需的完整参数和权限列表可在 `http://developers.facebook.com/docs/reference/api` 查看。
- 出于安全原因，Flash Player 10 及以上版本不允许你的应用程序通过 `HTTP POST` 方法发布任何内容，除非该操作是由用户发起的（例如，通过点击按钮）。

## 7

## FQL 相关问题

> 图谱是访问 Facebook 数据的一种极佳模型；它能够处理网站上海量的信息，并让访问变得简单易行。

> 然而，有时我们希望获得额外的控制力和更强大的能力。这时就要用到 FQL 了。它是一种专为搜索 Facebook 数据库而设计的编程语言——而且它不使用图谱。

在本章中，我们将：

- 回顾不同的数据表示和可视化模型
- 通过使用 FQL 获取一些 Facebook 数据来熟悉它
- 学习如何使用 FQL 来完成（几乎）所有图谱能做的事情
- 发现图谱比 FQL 更擅长的方面
- 找出如何检测已授予哪些扩展权限
- 了解一些高级 FQL 特性

那么，让我们开始吧……

## 什么是 FQL？

FQL（按首字母缩写读作“eff-cue-ell”）是 Facebook 版本的数据库语言 SQL。不知道 SQL 是什么？别担心，你不需要知道。在本章中，我会假设你对 FQL、SQL 或数据库一无所知。所以在深入探讨 FQL 是什么之前，让我们先退一步，看看数据库。

## 但如果我已经会 SQL 了呢？

即使你懂 SQL，也值得读一读本章，以复习一些概念，并了解 FQL 与常规 SQL 之间的差异。特别是，可以参考“限制”部分。
如果你有 SQL 经验，你可能会惊讶于 FQL 缺少多少功能。最明显的是，它没有 INSERT、DELETE 或 UPDATE 子句。此外，缺少 JOIN、GROUP BY 和 LIKE；你必须指定要返回的字段集，而不能输入 SELECT *，而且在 FROM 子句中只能指定一个表。

![](img/4e826fa36038b86f2087ef8b818d0acf_250_0.png)

## 理解 FQL 接口

我们一直把 Facebook 数据看作第 2 章中列出的图谱格式来查看，但实际上，这仅仅是一种数据模型；图谱 API 就像是一个包装器，让数据看起来像是按那种方式组织的。其实并非如此。

数据的组织方式重要吗？对于我们的目的来说并不重要。重要的是我们用来访问数据的接口。到目前为止，这个接口一直是图谱 API；我们通过图谱 URL 获取了所有数据，并用 AS3 进行处理。

但还有另一种查看数据的方式，它类似于传统的数据库结构。在这种情况下，所有数据都保存在表中，按列、行和字段组织。用一个例子来解释更容易：

| user |  |  |  |  |  |  |
|---|---|---|---|---|---|---|
| uid | first_name | middle_name | last_name | name | sex | username |
| 123456 | Michael | James | Williams | Michael James Williams | male | michael_james_williams |
| 789012 | Joe | Quincy | Faikname | Joe Q Faikname | male | joe_q_faikname |

为了简洁起见，省略了部分列。

这是一张数据表。具体来说，它是 user 表，你可以从下面的标题看出来：

![](img/4e826fa36038b86f2087ef8b818d0acf_250_1.png)

每一**列**代表数据的一个属性：

![](img/4e826fa36038b86f2087ef8b818d0acf_251_0.png)

每一**行**对应一个对象，包含其所有属性：

![](img/4e826fa36038b86f2087ef8b818d0acf_251_1.png)

行和列的交点——即某个对象的某个属性——称为一个**字段**：

![](img/4e826fa36038b86f2087ef8b818d0acf_251_2.png)

你也可以把这种结构想象成电子表格；这是以网格形式存储数据的一种简单而自然的方式。（当然，如果你熟悉关系数据库，你也可以直接把它想象成关系数据库中的一张表！）

## 数据模型

现在我们有了三种不同的模型可以用来思考 Facebook 数据：

1. AS3 面向对象模型：我们有一些包含特定属性的类，以及这些类的实例，实例中包含关于特定项目的信息。例如，我们可以创建一个名为 User 的类，它包含某些属性，并定义该类的一个实例：

```
class User
{
    public var uid:int;
```

让我们看一个例子，从 page 表中获取一些信息：

| page |
|------|
| page_id |
| name |
| website |
| type |
| location |

其中每一项——page_id、name、website、type 和 location——都是列，就像 uid、first_name、middle_name、last_name、name、sex 和 username 是 user 表的列一样。

## 动手实践——从 Page 表检索信息

那么我们如何使用它呢？当然，我们不能请求对象，因为我们不是在使用对象模型——但我们甚至也不请求行（行与对象是近似的对应物）。我们请求的是特定字段。如果我们只对页面关联网站的 URL 感兴趣，我们只需请求“website”列中的结果：

```
SELECT website
```

接下来，我们必须指定我们感兴趣的表。在本例中，就是“page”：

```
SELECT website
FROM page
```

现在我们指定一个条件来缩小搜索范围：

```
SELECT website
FROM page
WHERE name = "Facebook"
```

（请注意，在检查这个条件时，我们使用 = 而不是 ==，这与 AS3 不同。）

这三个元素构成一个“查询”。当我们运行这个查询时——类似于编译 AS3——我们将收到 http://www.facebook.com/，因为这是 page 表中 name 为 Facebook 的那一行的 website 列字段的值。

![](img/4e826fa36038b86f2087ef8b818d0acf_253_0.png)

> 通常会看到 FQL 关键字全部大写，如 SELECT、FROM 和 WHERE，但这并不是硬性要求。不过，这确实有助于提高可读性。

