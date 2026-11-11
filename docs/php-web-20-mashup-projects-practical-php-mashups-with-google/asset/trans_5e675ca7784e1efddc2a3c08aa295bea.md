
<![CDATA[
工作时吹口哨以在计算机上运行命令
]]>
</title>
<description>
<![CDATA[
使用 Linux 或 Microsoft Windows、开源的 sndpeek 程序以及一个简单的 Perl 脚本，读取特定的音调事件序列——字面意义上吹口哨、哼唱或对着计算机唱歌——并根据这些音调运行命令。给计算机吹一声低沉的短哨声来检查你的电子邮件，或用贝多芬第五交响曲的开场乐段解锁屏幕保护程序。工作时吹口哨，提高效率
]]>
</description>
<link>
<![CDATA[
http://www.ibm.com/developerworks/library/os-whistle/index.html?ca=drs-
]]>
</link>
<category>文章</category>
</item>
<item>
<title>
<![CDATA[
在 Cell BE 处理器上编程高性能应用程序，第 1 部分：PLAYSTATION 3 上 Linux 的介绍
]]>
</title>
<description>
<![CDATA[
索尼 PLAYSTATION 3 (PS3) 是程序员获得新 Cell Broadband Engine (Cell BE) 处理器并对其进行试用的最简单、最便宜的方式。了解其中的热议所在，如何在 PS3 上安装 Linux，以及如何在 PS3 上开始为 Cell BE 处理器开发。
]]>
</description>
<link>
<![CDATA[
http://www.ibm.com/developerworks/linux/library/pa-linuxps3-1/index.html?ca=drs-
]]>
</link>
<category>文章</category>
</item>
</channel>
</rss>

与 1.1 feed 类似，2.0 首先提供有关 feed 和发布者的信息，然后是新闻项目。

1.1 和 2.0 之间存在一些关键差异：

-   `rss` 元素是 2.0 的根元素。
-   `channel` 元素跟在 `rss` 之后，并包含所有其他元素。
-   每个项目都由一个 `item` 元素表示，但项目没有像 1.1 中的 `items` 那样将所有项目分组在一起的父元素。

从结构上看，feed 很相似，但标签和嵌套的差异足以造成问题，尤其是考虑到程序是 RSS feed 的主要消费者。

RSS 2.0 在其命名空间中还提供了更多标准标签。RSS 2.0 认识到，除了标题、链接和描述之外，人们希望在频道和项目中包含大量通用信息。诸如发布日期、语言和类别等内容经常被使用。在 RSS 1.1 中，必须使用另一个标准并通过命名空间将其引入文档。这不仅增加了额外开销，而且为将发布日期等简单内容放入 feed 中创造了多种方式。

| 频道子元素 | 是否必需？ | 描述 |
|---|---|---|
| title | 是 | feed 的人类可读标题。 |
| link | 是 | feed 的 URL。 |
| description | 是 | feed 的人类可读描述。 |
| category | 否 | feed 的一个或多个类别。此元素的可用值没有统一标准。 |
| cloud | 否 | cloud 是一个集中式服务器，用于保存关于一组 RSS feed 的信息。此元素将保存有关在 feed 更新时在 cloud 服务器上调用的远程过程的信息。其属性为 domain、port、path、procedure 和 protocol。远程过程可以是 XML-RPC 或 SOAP。 |
| copyright | 否 | 如果内容受版权保护，则版权信息放在此元素中。 |
| docs | 否 | feed 文档的 URL。 |
| generator | 否 | 用于生成 feed 的程序名称。 |
| image | 否 | 用于存放 feed 官方图像信息的部分。 |
| language | 否 | feed 所写的语言。 |
| lastBuildDate | 否 | feed 的最后修改日期。 |
| managingEditor | 否 | 负责 feed 内容的人员的电子邮件地址。 |
| pubDate | 否 | feed 的最后发布日期。 |
| rating | 否 | feed 的 PICS（互联网内容选择平台）评级。有关 PICS 的更多信息，请参阅 http://www.w3.org/PICS/。 |
| skipDays | 否 | RSS 聚合器不应读取此 feed 的日期。 |
| skipHours | 否 | RSS 聚合器不应读取此 feed 的小时。 |
| textInput | 否 | 与此字段一起显示的文本输入字段的名称。 |
| ttl | 否 | TTL 代表“生存时间”。它是 feed 应保留在客户端缓存中的分钟数。 |
| webMaster | 否 | 负责此 feed 技术方面的人员的电子邮件地址。 |

同样，RSS 2.0 为 item 元素提供了更多可用的子元素。

| 项目子元素 | 是否必需？ | 描述 |
|---|---|---|
| title | 是 | 项目的人类可读标题。 |
| link | 是 | 项目的 URL。 |
| description | 是 | 项目的人类可读描述。 |
| author | 否 | 该项目的作者的电子邮件地址。 |
| category | 否 | 项目的一个或多个类别。此元素的可用值没有统一标准。 |
| comments | 否 | 项目读者评论页面的 URL。 |
| enclosure | 否 | 允许为项目包含媒体文件。媒体文件的 URL 包含在必需的属性 url 中。length 为媒体文件的字节数，type 为文件的 MIME 类型，也都是必需的。此标签最常见的用途是在播客中指定 MP3。 |
| guid | 否 | 项目的唯一标识符。 |
| pubDate | 否 | 项目的最后发布日期。 |
| source | 否 | 项目的第三方来源。用于项目的引用来源。 |

## Atom 订阅格式

有一种新的、第三种订阅 feed 称为 Atom。Atom 试图在两个 RSS 分支之间实现可扩展性和简洁性的目标。从结构上看，Atom feed 与 RSS 共享类似的模型——元数据后跟条目。然而，元素名称完全不同。我们在该项目中不会使用 Atom，但你应该了解它。尽管它是格式中最不成熟的，市场份额相对较小，但它是唯一得到行业标准机构（具体来说是互联网工程任务组）支持的格式，并且已经被 Google News 等巨头使用。

## YouTube 概述

如今 YouTube 几乎无需介绍。该网站与我们之前讨论的许多网站一样无处不在——Amazon、Google、Yahoo! 和 MSN。它的视频链接在电子邮件账户中被广泛传播。万一你生活得非常与世隔绝，我们将简要了解一下 YouTube 的功能、一些特性和其 Web API。

简而言之，YouTube 是一个允许用户通过互联网与公众分享自制视频的网站。用户可以上传任何他们想要的视频（在充分尊重版权法的同时），其他用户可以观看并对其进行评论。后者使 YouTube 除了分享视频之外，还成为一个强大的社交网络网站。

YouTube 的一些可用功能包括：

-   **视频标签**
  YouTube 依靠其用户社区来描述其存储库中的视频。这是通过允许用户将描述性词语和短语与特定视频关联来实现的。此过程称为 **tagging**。例如，如果我正在观看来自布鲁克林、纽约的乐队 They Might Be Giants 的现场表演片段，YouTube 允许我为该片段添加标签，“They Might Be Giants”、“Brooklyn”、“alternative”。
-   **视频搜索**
  YouTube 拥有一个强大的搜索引擎，可查询视频上的标签。通过按标签搜索，视频的描述得以民主化。如果 YouTube 的搜索引擎仅查询提交者提供的描述，那么该人对视频在搜索结果中的返回方式有很大影响。通过同时查询标签，搜索引擎可以找到社区认为应该返回的视频。在我们的例子中，关于“They Might Be Giants”，片段提交者可能没有注意到该乐队位于布鲁克林。然而，由于我为该片段添加了“Brooklyn”标签，任何寻找当地乐队的布鲁克林人也可能发现 They Might Be Giants。
-   **提交者订阅**
  YouTube 会跟踪用户的视频提交。然后自然允许其他用户订阅另一用户的提交。通过他们的恶作剧和抨击，少数人已在 YouTube 上获得了相当多的追随者，并找到了他们的十五分钟成名时光。
-   **社区**
  YouTube 通过允许用户对视频发表评论来增强其网站的社交属性。
-   **嵌入到其他网站**
  YouTube 允许用户在其自己的网站上嵌入视频。因此，YouTube 本质上充当视频托管服务。此外，外部网站也可以仅显示视频的快速缩略图，并附上指向视频及其在 YouTube 上的所有信息和评论的链接。

YouTube 的社交网络属性使其成为一个很好的网络服务主题，而该网站也充分利用了这一点。

