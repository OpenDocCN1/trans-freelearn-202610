

## 一次调用多个查询

如果你知道需要调用不止一个查询，并且这些查询的调用顺序并不重要，你可以通过一个 API URL 将它们全部调用来提高性能。所有结果将一次性返回。

语法是：

```
https://api.facebook.com/method/fql.multiquery?queries=["SELECT x FROM a WHERE c", "SELECT y FROM b WHERE d"]&access_token=«access_token»
```

尽管在这个示例中我只使用了非常简单的查询，但你并不限于这些。

## 小测验

+   1. 何时使用 FQL 而不是 Graph API 才有意义？
+      a. 当发布信息到 Facebook 时
+      b. 当尝试检索特定字段列表时
+      c. 当尝试了解用户的扩展权限时
+   2. 何时使用 Graph API 而不是 FQL 才有意义？
+      a. 当发布信息到 Facebook 时
+      b. 当尝试检索特定字段列表时
+      c. 当尝试了解用户的扩展权限时
+   3. 数据库表中的列和行与 AS3 模型中的对象和属性如何对应？
+      a. 列像属性，行像对象
+      b. 列像对象，行像属性

## 总结

在本章中，你从对 FQL 一无所知，到能够使用它进行比 Graph API 更高级的搜索查询。你还了解了一些 Graph API 不支持的技术，例如如何获取当前用户被授予的扩展权限列表。

要点如下：

+   - 数据库将数据安排成表、列、行和字段
+   - FQL 代表 Facebook Query Language
+   - 提交 FQL 查询的 API URL 为 https://api.facebook.com/method/fql.query?access_token=ACCESS_TOKEN&format=FORMAT&query=FQL_QUERY — FORMAT 可以是 json 或 xml
+   - FQL 查询必须查询可索引字段
+   - 通过将两张表的名称（不是列！）与线前的动词组合起来阅读实体关系线
+   - 可以查询权限表以了解经过身份验证的用户被授予了哪些扩展权限
+   - 表及其列的列表（附带哪些字段可索引的指南）可以在 http://developers.facebook.com/docs/reference/fql/ 找到

在下一章中，我们将总结本书中迄今为止学到的所有内容，并完成你的应用程序的整体构建。

## 在 Facebook 上

截至撰写时，有三种方式让用户从 Facebook 网站内访问你的应用程序：在 IFrame 中，通过 FBML，以及在 Page Tab 中。我们将依次介绍这些是什么，以及如何使用它们。

## IFrame

还记得 frames 吗？它们回来了...

在九十年代，每隔一个网站都使用 HTML <frame> 标签来布局页面。这种时尚最终被更干净、更适合书签的方法所取代，并且从未被怀念。

但现在 frames 又在普遍使用——或者更确切地说，它们的姐妹标签 <iframe>。

IFrame，或“内联框架”，允许你将一个完整的网页完全嵌入到另一个网页中，即使这两个页面位于不同的服务器和域中。如果你查看例如 FarmVille 应用程序页面（http://apps.facebook.com/onthefarm/）的源代码，你会看到情况确实如此，如下一张截图所示：

![](img/4e826fa36038b86f2087ef8b818d0acf_280_0.png)

Facebook 站点上的页面称为 Canvas（不要与 HTML5 的 canvas 混淆）。

现在，我们已经有一个托管在外部主机上的页面（嵌入了 SWF）：http://host.com/visualizer/index.html。我们不需要对该页面做任何更改即可将其加载到 Facebook 上的 IFrame 中；设置全部在 Facebook 开发者应用程序中。

## 动手时间 – 设置一个 IFrame 应用程序

浏览到 http://www.facebook.com/developers/apps.php 并在列表中找到你的应用程序。选择它，然后点击 Edit settings。从左侧菜单中选择 Facebook Integration。

![](img/4e826fa36038b86f2087ef8b818d0acf_281_0.png)

设置可以在上一张截图中看到。以下是它们的含义，以及应如何设置：

+   ♦ Canvas Page：这是用户将在 Facebook 内浏览以加载你的应用程序的 URL。正如我们上面看到的，FarmVille 使用 http://apps.facebook.com/onthefarm。输入一个简短且描述性的名称；它不必太巧妙，因为用户更可能通过 Facebook 书签到达该页面，而不是通过输入 URL。你以后可以更改此 URL。
+   ♦ Canvas URL：这是被加载到位于 Canvas 页面内的 IFrame 中的页面的 URL。你的应用程序可以包含用户可以在其间导航的多个 HTML 页面；Canvas URL 是将被加载的第一页。将其设置为你的应用程序的 URL，即 http://host.com/visualizer/index.html。
+   ♦ Canvas Type：IFrame 或 FBML。我们目前使用 IFrame；我们将在本章后面介绍 FBML。

## 动手时间 – 将应用程序添加到 Page 标签

要将你的应用程序作为你在管理的 Page 上的标签添加，你必须首先将其标记为可以嵌入到标签中。打开应用程序的设置，并导航到 Facebook Integration 部分。在 Page Tabs 子部分中，输入以下内容：

+   - **Tab Name**：要在标签上显示的名称（保持简短）
+   - **Tab URL**：应用程序网页的文件名，相对于 Canvas URL
+   - **Edit URL**：留空

![](img/4e826fa36038b86f2087ef8b818d0acf_285_0.png)

接下来，从开发者仪表板浏览到应用程序的 Application Profile Page，并从左侧菜单点击 Add To My Page：

![](img/4e826fa36038b86f2087ef8b818d0acf_285_1.png)

> 将添加到我的页面的收藏夹会将应用程序的个人资料链接到你的 Page，而不是应用程序本身。

你会得到一个你控制的 Pages 列表；如果你没有 Page，可以按照 http://www.facebook.com/FacebookPages 上的说明学习如何创建一个。

但让我们看看当你能够创建它们时，你需要了解有关标签的知识。

从技术角度来看，最重要的点是标签的宽度只有 520 像素，因此在布局你的应用程序及其标签页面时请记住这一点。你甚至可能希望创建一个单独的网页来容纳 SWF——甚至是单独的 SWF——用于标签内使用。

如果你为一家公司制作了一个广告游戏，并集成了 Facebook 以在游戏中使用玩家自己的名字并让他们将分数发布到他们的 Wall 上，你可以将游戏放在该公司 Facebook 页面的标签上，而不是仅使用独立的应用程序页面。

Adobe 就采用了这种方法，使用他们的 Flex 测验游戏 Cube Builder：

![](img/4e826fa36038b86f2087ef8b818d0acf_287_0.png)

门户可能会阻止 SWF 从外部站点加载数据，这意味着访问即使是公共 Graph 数据也会变得棘手或不可能。这不太可能——如果完全阻止外部资源，游戏内广告如何工作？——但值得留意。

真正的难题在于身份验证。这有两个原因：

+   ♦ 为了获得访问令牌，我们在 SWF 嵌入的页面内调用一个 JavaScript 函数。但 Flash 游戏门户不让我们访问网页的源代码，因此我们无法插入此 JavaScript 函数以运行。此外，门户很可能会阻止 SWF 使用 `ExternalInterface` 调用 JavaScript 函数，因为它们可能构成安全风险。
+   
+   ♦ 在第 3 章中，我们了解到我们必须将 Site URL 设置为与应用程序主机的基本域名相匹配。如果它们不匹配——比如说，如果你的应用程序位于 `http://portal.com/games/awesomegame.html` 但 Site URL 设置为 `http://host.com/~` Facebook 将不允许用户进行身份验证。这意味着我们不能允许托管在不同门户上的游戏通过同一个 Facebook 应用程序进行身份验证。

因此，很遗憾，Facebook 并未为可以从不同站点运行的 SWF 设计其应用程序模型。一个可能的变通方法是导入一个托管在你自己的服务器上的单独 SWF 到主游戏 SWF 中，并让该单独的 SWF 处理身份验证……但实现这一点超出了本书的范围。

不过，你不必自己做所有这些工作！如果你不需要访问整个 Graph API，而只想能够在用户的 Wall 上发布消息、检索他们的个人资料信息并获取他们的 Facebook 好友列表，那么你可以使用 Mochi Social Platform。

这是一个由 Mochi Media 提供的免费 ActionScript API，它与 Facebook、Twitter 和 MySpace API 集成，允许你发布到并访问这三个社交网络的数据。它还允许你存储关于玩家的任意数据并将其链接到他们的社交网络个人资料。这意味着你可以存储他们的最高分数、其他统计数据，甚至他们构建的整个自定义关卡。

更多信息，请访问 `http://www.mochimedia.com/developers/social.html`。

## 作为桌面 AIR 应用程序

尽管我们编写的大部分 `HTTPRequestor` 代码在桌面 AIR 应用程序中无需任何更改即可工作，但有一个障碍：身份验证。由于在使用 AIR 时，我们没有嵌入在 HTML 页面中的 SWF，我们如何显示 Facebook 授权页面、获取访问令牌并将其传递给 AS3 函数？

