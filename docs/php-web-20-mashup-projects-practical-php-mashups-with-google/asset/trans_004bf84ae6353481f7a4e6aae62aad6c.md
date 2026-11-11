
接下来，我们准备显示播放列表。在此之前，我们先提取有关播放列表的一些信息。XSPF 对象的 getTitle 方法返回 XSPF 文件的 title 元素。getCreator 返回文件的 creator 元素。

```
<?php foreach ($tracks as $track) { ?>
    <p>
        Title: <?= $track->getTitle() ?><br />
        Artist: <?= $track->getCreator() ?><br />
    </p>
<?php } ?>
</body>
</html>
```

最后，我们遍历 tracks 数组。我们将数组的元素（它们是 XSPF_Track 对象）赋给 $track 变量。XSPF_Track 也有 getTitle 和 getCreator 方法。与 XSPF 中同名的方法不同，getTitle 返回曲目的标题，而 getCreator 返回曲目的艺术家。

运行此文件在你的网页浏览器中将返回一个填充了来自 Last.fm 的数据的列表。

![](img/c637937d44400f1da21fb7cca101de80_162_0.png)

## Services_YouTube

Services_YouTube 的工作方式与 File_XSPF 非常相似。与 File_XSPF 一样，它是建立在更复杂系统之上的面向对象抽象层。在本例中，该系统是 YouTube API。

使用 Services_YouTube 与使用 File_XSPF 非常相似。将包包含在你的代码中，实例化一个 Services_YouTube 对象，并使用该对象的方法与服务交互。Services_YouTube 最新版本的官方文档位于 http://pear.php.net/package/Services_YouTube/docs/latest/。该包还包含在线工作示例，网址为 http://pear.php.net/manual/en/package.webservices.services-youtube.php。

许多方法处理获取成员信息，如他们的个人资料和已上传的视频。较小但非常重要的子集用于查询 YouTube 视频。我们将在我们的 mashup 中使用此子集。要获取带有特定标签的视频列表，请使用对象的 listByTag 方法。

listByTag 将查询 YouTube 服务并将 XML 响应存储在内存中。它不会返回我们可以直接管理的视频对象数组，但通过一次额外的函数调用，我们可以实现这一点。从那里开始，我们可以遍历视频数组，就像我们对 XSPF 曲目所做的那样。

示例文件 YouTubePearTest.php 说明了此过程。

```
<?php
    require_once 'Services/YouTube.php';
    $dev_id = 'Your YouTube DeveloperID';
    $tag = 'Social Distortion';
    $youtube = new Services_YouTube($dev_id, array('usesCache' => true));
    $videos = $youtube->listByTag($tag);
?>
```

首先，我们将 Services_YouTube 文件加载到脚本中。由于 YouTube 的网络服务需要 Developer ID，我们将该信息存储到一个局部变量中。之后，我们将要搜索的标签放在另一个名为 $tag 的局部变量中。在本例中，我们将查看 YouTube 上有关于有史以来最伟大的乐队之一 Social Distortion 的哪些视频。Services_YouTube 的构造函数接受此 Developer ID，并在每次查询 YouTube 网络服务时使用它。构造函数可以接受一个选项数组作为参数。其中一个选项是使用查询的本地缓存。使用缓存被认为是一种良好的做法，以避免猛击 YouTube 服务器并用完你的请求配额。

另一个选项是通过 options 数组中的 driver 键指定 REST 或 XML-RPC 作为协议。默认情况下，Services_YouTube 使用 REST。除非你有迫切需要使用 XML-RPC，否则可以保持原样。

实例化后，你可以调用 listByTag 来获取 YouTube 的响应。listByTag 只接受一个参数——我们想要的标签。

Services_YouTube 现在有了来自 YouTube 的结果。我们可以开始显示结果。

```
<html>
<head>
<title>Social Distortion Videos</title>
</head>
<body>
<h1>YouTube Query Results for Social Distortion</h1>
```

接下来，我们将遍历视频。为了获得视频对象数组，我们首先需要解析 XML 响应。我们使用 Services_YouTube 的 xpath 方法来完成此操作，该方法将使用强大的 XPATH 查询语言遍历 XML 并将其转换为 PHP 对象。我们将 XPATH 查询传递给该方法，它将为我们提供一个有用的对象数组。我们将在另一个项目中更详细地研究 XPATH 和 XPATH 查询。现在，请相信查询 //video 将返回一个我们可以检查的视频对象数组。

在循环中，我们显示每个视频的标题、视频的缩略图以及指向视频本身的超链接。

```
<?php foreach ($videos->xpath('//video') as $i => $video) { ?>
<p>
    Title: <?= $video->title ?><br />
    <img src='<?= $video->thumbnail_url ?>' alt='<?= $video->title ?>' />
<br />
    <a href='<?= $video->url ?>'>URL</a>
</p>
<?php } ?>
</body>
</html>
```

在我们的网页浏览器中运行此查询将为我们提供与我们提交的搜索词匹配的视频结果页面。

![](img/c637937d44400f1da21fb7cca101de80_164_0.png)

> 请注意，在运行 YouTubePearTest.php 文件之前，你必须安装 CURL。

### Social Distortion 的 YouTube 查询结果

标题: social distortion - story of my life

![](img/c637937d44400f1da21fb7cca101de80_165_0.png)

URL

标题: Social Distortion Reach for the Sky

![](img/c637937d44400f1da21fb7cca101de80_165_1.png)

URL

标题: Social Distortion Don't Take Me for Granted

![](img/c637937d44400f1da21fb7cca101de80_165_2.png)

URL

## XML_RSS

与其它 PEAR 扩展一样，XML_RSS 将非常复杂的东西 RSS 变成了一个非常简单易用的 PHP 对象。该包的完整文档位于 http://pear.php.net/package/XML_RSS/docs/XML_RSS。

与 Services_YouTube 和 File_XSPF 相比，XML_RSS 的基本理念有一个小的差异。后两个包将我们感兴趣的信息取出并放入 PHP 对象属性中。

例如，File_XSPF 将曲目名称放入 Track 对象中，你使用 getTitle() getter 方法获取曲目的标题。在 Services_YouTube 中，原则相同，但属性是公开的，因此没有 getter 方法。你可以直接在视频对象中访问视频的属性。

在 XML_RSS 中，我们感兴趣的值存储在关联数组中。该包中的可用方法获取数组，然后你直接操作它们。这是一个小的差异，但如果你想查看代码，应该注意这一点。这也意味着你必须查看包的文档，以了解有哪些数组键可供你使用。

让我们通过一个示例来看看它是如何工作的。示例代码中的文件名为 RSSPEARTest.php。Audioscrobbler 的其中一个 feed 为我们提供了一个用户最近播放的歌曲的 RSS 文件。该 feed 并非总是被填充，因为在几个小时后，播放的歌曲不再被视为最近的。换句话说，歌曲最终会因为太旧而从 feed 中删除。因此，最好将此 feed 用于 Last.fm 的重度用户。

RJ 是一个很好的例子。他似乎总是在听些什么。我们将从 Audioscrobbler 获取他的 feed：

```
<?php
    include ("XML/RSS.php");
    $rss =& new XML_RSS("http://ws.audioscrobbler.com/1.0/user/RJ/
                        recenttracks.rss");
    $rss->parse();
```

我们首先包含模块并创建一个 XML_RSS 对象。XML_RSS 是所有数组 get 方法所在的地方，也是该包的核心。它的构造函数接受一个变量——RSS 文件的路径。在实例化时，包会将 RSS 文件加载到内存中。

parse() 是实际执行 RSS 解析的方法。之后，get 方法将返回有关 feed 的数据。毋庸置疑，在对文件进行任何有建设性的操作之前，必须调用 parse()。

```
$channelInfo = $rss->getChannelInfo();
?>
```

包的 getChannelInfo() 方法返回一个数组，其中包含有关文件元数据（频道）的信息。该数组保存 RSS 文件的 title、description 和 link 元素。这些元素中的每一个都以与元素相同的键名存储在数组中。

```
<?= "<?xml version=\"1.0\" encoding=\"UTF-8\" ?>" ?>
